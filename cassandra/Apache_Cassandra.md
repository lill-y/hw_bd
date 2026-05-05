# 1. Создаем кластер из 3 нод и проверяем их доступность
> файл docker_compose для касандры лежит в этой же папке
- Создали все с помощью докера
<img width="566" height="298" alt="image" src="https://github.com/user-attachments/assets/a03c31f7-5d7d-4156-9231-dd0c4045e51c" />

### Проверяем что кластер жив
- Ура все ноды работают
<img width="713" height="137" alt="image" src="https://github.com/user-attachments/assets/d5a5c19a-1ea9-42ae-88a4-d999338621cf" />

- Подключаемся (тут без порта почему то не работало, видимо не туда подключался)
<img width="740" height="128" alt="image" src="https://github.com/user-attachments/assets/6d7b3d62-094e-4868-bc89-98ce76ea2f53" />

- Создаем keyspase
<img width="272" height="87" alt="image" src="https://github.com/user-attachments/assets/b8546a17-0b29-472d-80a4-90c9f34a6e91" />

# 2. Создаем 2 таблицы под разные запросы с одинаковыми данными
> Тут сделаны две таблицы с одинаковыми данными, но под разные запросы
>
> Одна таблица нужна, чтобы быстро получать все сообщения пользователя по user_id
>
> Вторая — чтобы быстро находить конкретное сообщение по message_id
```
CREATE TABLE messages_by_user (
    user_id UUID,
    created_at TIMESTAMP,
    message_id UUID,
    text TEXT,
    PRIMARY KEY (user_id, created_at)
);
```
> В верхней таблице используется created_at, чтобы у одного пользователя могло быть несколько сообщений и они хранились по времени.
```
CREATE TABLE messages_by_id (
    message_id UUID PRIMARY KEY,
    user_id UUID,
    text TEXT,
    created_at TIMESTAMP
);
```

- Вот такие сообщения делаем
```
-- Сообщение 1
INSERT INTO messages_by_user (user_id, created_at, message_id, text)
VALUES (11111111-1111-1111-1111-111111111111,
        '2026-05-01 10:00:00',
        22222222-2222-2222-2222-222222222222,
        'Hello world');

INSERT INTO messages_by_id (message_id, user_id, text, created_at)
VALUES (22222222-2222-2222-2222-222222222222,
        11111111-1111-1111-1111-111111111111,
        'Hello world',
        '2026-05-01 10:00:00');

-- Сообщение 2
INSERT INTO messages_by_user (user_id, created_at, message_id, text)
VALUES (11111111-1111-1111-1111-111111111111,
        '2026-05-01 10:05:00',
        33333333-3333-3333-3333-333333333333,
        'How are you?');

INSERT INTO messages_by_id (message_id, user_id, text, created_at)
VALUES (33333333-3333-3333-3333-333333333333,
        11111111-1111-1111-1111-111111111111,
        'How are you?',
        '2026-05-01 10:05:00');

-- Сообщение 3
INSERT INTO messages_by_user (user_id, created_at, message_id, text)
VALUES (44444444-4444-4444-4444-444444444444,
        '2026-05-01 11:00:00',
        55555555-5555-5555-5555-555555555555,
        'Hiiii');

INSERT INTO messages_by_id (message_id, user_id, text, created_at)
VALUES (55555555-5555-5555-5555-555555555555,
        44444444-4444-4444-4444-444444444444,
        'Hiiii',
        '2026-05-01 11:00:00');

-- Сообщение 4
INSERT INTO messages_by_user (user_id, created_at, message_id, text)
VALUES (44444444-4444-4444-4444-444444444444,
        '2026-05-01 11:10:00',
        66666666-6666-6666-6666-666666666666,
        'Im good how about you');

INSERT INTO messages_by_id (message_id, user_id, text, created_at)
VALUES (66666666-6666-6666-6666-666666666666,
        44444444-4444-4444-4444-444444444444,
        'Im good how about you',
        '2026-05-01 11:10:00');
```
> итог: Мы дублируем данные в двух таблицах, потому что Cassandra оптимизирована под конкретные запросы, а не под универсальность как обычные SQL базы
# 3. INSERT выше, остальное:
- SELECT (сначала по user_id, потом по message_id)
<img width="908" height="239" alt="image" src="https://github.com/user-attachments/assets/321f76ab-51ef-40c9-841c-858c38e2cb28" />

- UPDATE с селектом
<img width="905" height="161" alt="image" src="https://github.com/user-attachments/assets/f6a92c08-f7d5-4808-be65-82f8a0196bb7" />

- DELETE с селектом
<img width="897" height="132" alt="image" src="https://github.com/user-attachments/assets/70b885f1-20e3-4c5d-bb16-0cd685f42844" />

- SELECT ERROR
<img width="923" height="71" alt="image" src="https://github.com/user-attachments/assets/ed1dd42b-10f0-4dd6-8c9d-cc61f3023392" />

# Отказоустойчивость:
<img width="479" height="30" alt="image" src="https://github.com/user-attachments/assets/52fc058d-fadb-4950-a8b6-f99a0c428d5c" />
<img width="925" height="157" alt="image" src="https://github.com/user-attachments/assets/da73f928-72db-4ae7-8e7f-9156d7ccf6a6" />
# После остановки одной ноды данные всё равно доступны, потому что они хранятся сразу на нескольких нодах :)
