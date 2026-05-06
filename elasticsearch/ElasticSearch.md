# 1. Поднимаем Elasticsearch через Docker (сразу проверяем что все работает)
<img width="540" height="352" alt="image" src="https://github.com/user-attachments/assets/1aab3254-de3f-4c15-9cdf-2e1111c49fc3" />

# 2. Создаем индекс products
<img width="593" height="30" alt="image" src="https://github.com/user-attachments/assets/92a3a2be-440c-491f-81ae-064a500578d8" />

>
> Проверяем, что все ок
> <img width="846" height="43" alt="image" src="https://github.com/user-attachments/assets/2813469b-aabf-415c-96bd-4d9ef6e3b106" />

# 3. Заполняем индекс тестовыми данными с помощью методов PUT или POST
> В Elasticsearch документы можно добавлять двумя способами:
> через POST (автоматическая генерация id)
> и PUT (с явным указанием id)
> 
> Каждый документ хранится в формате JSON.

- POST
```
curl -X POST "http://localhost:9200/products/_doc" \
-H "Content-Type: application/json" \
-d '{
  "name": "iPhone 15",
  "price": 1200,
  "category": "phone",
  "in_stock": true
}'
```
```
curl -X POST "http://localhost:9200/products/_doc" \
-H "Content-Type: application/json" \
-d '{
  "name": "MacBook Pro",
  "price": 2500,
  "category": "laptop",
  "in_stock": false
}'
```

- PUT
```
curl -X PUT "http://localhost:9200/products/_doc/1" \
-H "Content-Type: application/json" \
-d '{
  "name": "Xiaomi Redmi Note 13",
  "price": 300,
  "category": "phone",
  "in_stock": true
}'
```

```
curl -X PUT "http://localhost:9200/products/_doc/2" \
-H "Content-Type: application/json" \
-d '{
  "name": "Dell XPS 13",
  "price": 1800,
  "category": "laptop",
  "in_stock": true
}'
```

# 4. Выполняем операции с документами
- создать документ
```
curl -X POST "http://localhost:9200/products/_doc" \
-H "Content-Type: application/json" \
-d '{
  "name": "Samsung Galaxy S23",
  "price": 1000,
  "category": "phone",
  "in_stock": true
}'
```

- добавить документ с указанным id
```
curl -X PUT "http://localhost:9200/products/_doc/3" \
-H "Content-Type: application/json" \
-d '{
  "name": "Apple Watch",
  "price": 500,
  "category": "watch",
  "in_stock": true
}'
```

- обновить документ
```
curl -X POST "http://localhost:9200/products/_update/1" \
-H "Content-Type: application/json" \
-d '{
  "doc": {
    "price": 950
  }
}'
```

- удалить документ
```
curl -X DELETE "http://localhost:9200/products/_doc/1"
```

### Пишем запросы
- поиск по названию товара
```
curl -X GET "http://localhost:9200/products/_search?pretty" \
-H "Content-Type: application/json" \
-d '{
  "query": {
    "match": {
      "name": "iphone"
    }
  }
}'
```
<img width="325" height="424" alt="image" src="https://github.com/user-attachments/assets/1dee487d-6551-4821-8f65-a1915e9424f4" />

- запрос с использованием match
```
curl -X GET "http://localhost:9200/products/_search?pretty" \
-H "Content-Type: application/json" \
-d '{
  "query": {
    "match": {
      "category": "phone"
    }
  }
}'
```
<img width="349" height="579" alt="image" src="https://github.com/user-attachments/assets/3a7f0d68-9c98-4166-b265-c7857f2defd8" />

- запрос с использованием term
```
curl -X GET "http://localhost:9200/products/_search?pretty" \
-H "Content-Type: application/json" \
-d '{
  "query": {
    "term": {
      "category": "phone"
    }
  }
}'
```
<img width="435" height="584" alt="image" src="https://github.com/user-attachments/assets/b9407769-1647-45bd-b1a4-46a8ebfa0309" />
> ну тут у меня одинаковый вывод, но так то match ищет частичные совпадения, а term уже точные

- запрос с использованием range
```
curl -X GET "http://localhost:9200/products/_search?pretty" \
-H "Content-Type: application/json" \
-d '{
  "query": {
    "range": {
      "price": {
        "gte": 500,
        "lte": 1500
      }
    }
  }
}'
```
<img width="324" height="579" alt="image" src="https://github.com/user-attachments/assets/8ffd9393-ec9b-47c5-b989-e82d4bed03f7" />

- запрос с использованием bool с комбинацией условий
```
curl -X GET "http://localhost:9200/products/_search?pretty" \
-H "Content-Type: application/json" \
-d '{
  "query": {
    "bool": {
      "must": [
        { "match": { "category": "phone" } }
      ],
      "filter": [
        { "range": { "price": { "lte": 1200 } } }
      ]
    }
  }
}'
```
<img width="358" height="574" alt="image" src="https://github.com/user-attachments/assets/6164172d-a9e5-48db-ad68-b99aa30847c7" />
