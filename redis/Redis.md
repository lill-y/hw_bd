# Часть 1. Запуск Redis

1. Запустите Redis через Docker.
<img width="516" height="196" alt="image" src="https://github.com/user-attachments/assets/39ad14ac-66da-49f3-80d0-2a5093ca068e" />

2. Подключитесь к Redis CLI.
<img width="505" height="33" alt="image" src="https://github.com/user-attachments/assets/3a8116cf-d2d6-48cd-ae9c-f494f1ded35f" />

# Часть 2. Счётчик просмотров
<img width="320" height="228" alt="image" src="https://github.com/user-attachments/assets/05e153c9-b097-4e25-bb56-f055cf356eb6" />

# Часть 3. Рейтинг статей
> cоздаем статьи с просмотрами и выводим топ 3
<img width="398" height="214" alt="image" src="https://github.com/user-attachments/assets/97feb7e8-c278-4200-9e58-af235275dfa5" />

> обновляем просмотры у одной из статей и смотрим снова топ 3
<img width="400" height="129" alt="image" src="https://github.com/user-attachments/assets/28d54ae9-6d46-4e31-a040-84fcd3b205a7" />

# Часть 4. Ограничение действий пользователя

<img width="337" height="200" alt="image" src="https://github.com/user-attachments/assets/e9ca32aa-0166-46d4-8a97-79ddd289f384" />

> что тут происходит:
>
> поставила таймер на лайки
> поставила несколько (INCR)
> посмотрела сколько осталось до испарения(TTL)
> после истечения времени посмотрела количество

