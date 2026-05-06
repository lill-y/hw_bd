## Подготовка
- 1) Запустить Neo4j контейнер
> docker-compose в этой папке 
<img width="917" height="131" alt="image" src="https://github.com/user-attachments/assets/87a56460-a63d-447a-ba09-5ea4284c222e" />

- 2) Импортировать датасет из README.md
<img width="871" height="707" alt="image" src="https://github.com/user-attachments/assets/e4d36ea0-8f7f-4a89-b431-1bec265bfabf" />

## Вставка
- 1) Добавить категорию
<img width="639" height="170" alt="image" src="https://github.com/user-attachments/assets/2850a157-fbf0-41c2-aaf9-7b28c73ff918" />

- 2) Добавить статью
<img width="583" height="113" alt="image" src="https://github.com/user-attachments/assets/aa9b9839-a0b6-421f-ab20-380fef2ef8d4" />

- 3) Добавить читателя, добавить связь с 3-5 статьями
  - читатели
<img width="583" height="172" alt="image" src="https://github.com/user-attachments/assets/c7e62f62-4b5a-48b0-85a0-ac3bae48030d" />
  - связи
  <img width="690" height="172" alt="image" src="https://github.com/user-attachments/assets/5ea08fb7-b3dc-4b51-9a69-6770a29b4940" />

## Запросы
- 1) Отобразить всех пользователей, статьи и связи между ними
<img width="941" height="606" alt="image" src="https://github.com/user-attachments/assets/b8431185-801a-4fae-bf4a-ae40f7a5d771" />

- 2) Выбрать пользователя и найти категории, которые он читает
<img width="566" height="169" alt="image" src="https://github.com/user-attachments/assets/3220cf51-63cc-4f09-86bb-433f566f8e2d" />

- 3) Найти самых активных читателей (посчитать, кто читает больше всего статей)
<img width="464" height="216" alt="image" src="https://github.com/user-attachments/assets/71d7f723-e218-48dc-b2bc-f4c8fb719c3d" />

- 4) Выбрать статью и найти похожие статьи (статьи, которые читают те же пользователи)
<img width="1368" height="308" alt="image" src="https://github.com/user-attachments/assets/1ac3dffa-14bc-40b5-b787-c3e52e4fe00d" />

- 5) Рекомендации по категориям
    - найти категории, которые читает пользователь
    - <img width="555" height="155" alt="image" src="https://github.com/user-attachments/assets/4c2c413c-c4ed-4e01-814c-c10b2e9d2f6a" />

    - предложить статьи из этих категорий, которые он ещё не читал
    - <img width="656" height="173" alt="image" src="https://github.com/user-attachments/assets/cd2bc985-9385-4556-8928-4005dfdc6469" />

