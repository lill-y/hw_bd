## Подготовка данных
- Создаем векторы
```
curl -X PUT "http://localhost:6333/collections/articles" \
-H "Content-Type: application/json" \
-d '{
  "vectors": {
    "size": 384,
    "distance": "Cosine"
  }
}'
```
<img width="330" height="19" alt="image" src="https://github.com/user-attachments/assets/85b86420-f355-4c31-a468-faadff9fd466" />

- Добавляем 5 статей
```
import requests
from sentence_transformers import SentenceTransformer

model = SentenceTransformer("all-MiniLM-L6-v2")

def make_point(id, text, title, category):
    vector = model.encode(text).tolist()

    return {
        "id": id,
        "vector": vector,
        "payload": {
            "title": title,
            "content": text,
            "author": "auto",
            "category": category,
            "published_at": "2024-02-01",
            "views": 1000 + id * 100,
            "rating": 3.5 + id * 0.3
        }
    }

points = [
    make_point(1, "бег и спорт улучшают здоровье", "Бег", "sport"),
    make_point(2, "новости технологий и AI", "AI news", "tech"),
    make_point(3, "футбол и тренировки", "Футбол", "sport"),
    make_point(4, "обзор новых смартфонов", "Гаджеты", "tech"),
    make_point(5, "последние новости мира", "Новости", "news"),
]

url = "http://localhost:6333/collections/articles/points?wait=true"

res = requests.put(url, json={"points": points})
print(res.json())
```
> Я заморочилась и буду генерировать векторы с помощью SentenceTransformer в питоне 
<img width="1343" height="171" alt="image" src="https://github.com/user-attachments/assets/c587324f-0c8b-4943-bb92-dfd7c602d527" />

## Поиск
- Простой поиск — найти 3 статьи, похожие на запрос "бег и спорт"
```
from sentence_transformers import SentenceTransformer
import requests

model = SentenceTransformer("all-MiniLM-L6-v2")

vector = model.encode("бег и спорт").tolist()

res = requests.post(
    "http://localhost:6333/collections/articles/points/search",
    json={
        "vector": vector,
        "limit": 3
    }
)

print(res.json())
```
<img width="407" height="57" alt="image" src="https://github.com/user-attachments/assets/32e5fdd4-7a53-465d-a330-1801fd33779c" />

- Поиск с фильтром по категории — найти статьи в категории "tech" с рейтингом >= 4.0
```
res = requests.post(
    "http://localhost:6333/collections/articles/points/search",
    json={
        "vector": vector,
        "limit": 3,
        "filter": {
            "must": [
                {
                    "key": "category",
                    "match": {"value": "tech"}
                },
                {
                    "key": "rating",
                    "range": {"gte": 4.0}
                }
            ]
        }
    }
)

print(res.json())
```
<img width="513" height="32" alt="image" src="https://github.com/user-attachments/assets/7e39e054-0b1b-4793-bc7a-abbb2f657f4b" />

- Поиск с диапазоном дат — найти статьи, опубликованные после "2024-01-01", с просмотрами > 1000
```
res = requests.post(
    "http://localhost:6333/collections/articles/points/search",
    json={
        "vector": vector,
        "limit": 3,
        "filter": {
            "must": [
                {
                    "key": "published_at",
                    "range": {"gte": "2024-01-01"}
                },
                {
                    "key": "views",
                    "range": {"gt": 1000}
                }
            ]
        }
    }
)

print(res.json())
```
<img width="522" height="46" alt="image" src="https://github.com/user-attachments/assets/03a28f45-44ab-477f-874d-a98d40accc2d" />

- Сложный фильтр — найти статьи:
    - Категория: "sport" ИЛИ "tech"
    - Рейтинг >= 3.5
    - Просмотры от 500 до 5000
    - Отсортировать по релевантности (score)
```
res = requests.post(
    "http://localhost:6333/collections/articles/points/search",
    json={
        "vector": vector,
        "limit": 5,
        "filter": {
            "must": [
                {
                    "key": "rating",
                    "range": {"gte": 3.5}
                },
                {
                    "key": "views",
                    "range": {
                        "gte": 500,
                        "lte": 5000
                    }
                }
            ],
            "should": [
                {
                    "key": "category",
                    "match": {"value": "sport"}
                },
                {
                    "key": "category",
                    "match": {"value": "tech"}
                }
            ]
        }
    }
)

print(res.json())
```
<img width="516" height="57" alt="image" src="https://github.com/user-attachments/assets/29239ccb-a527-4e98-9e50-53448e468718" />

## Индексы и оптимизация
```
import requests

url = "http://localhost:6333/collections/articles/index"

# category (keyword)
requests.put(url, json={
    "field_name": "category",
    "field_schema": "keyword"
})

# rating (float)
requests.put(url, json={
    "field_name": "rating",
    "field_schema": "float"
})

# published_at (datetime)
requests.put(url, json={
    "field_name": "published_at",
    "field_schema": "datetime"
})

# views (integer)
requests.put(url, json={
    "field_name": "views",
    "field_schema": "integer"
})
```
> для проверки взяла Поиск с диапазоном дат, разница действительно есть, только время выполнения не изменилось. Возможно, это из за того, что это нужно проверять но большем объеме данных, но как есть
>
> - до: 0.0097
> - после: 0.0097
<img width="514" height="47" alt="image" src="https://github.com/user-attachments/assets/e2086454-ed50-4ead-a14c-754cb7ef8291" />

## Для умных
я не умная так что эту часть не делала
