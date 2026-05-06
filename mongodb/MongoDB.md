# 1. Создание коллекции
```
db.createCollection("books")

db.books.insertOne({
  title: "Clean Code",
  genre: "programming",
  price: 45,
  available: true,
  tags: ["code", "best practices", "software"],
  author: {
    name: "Robert Martin",
    country: "USA"
  }
})
```
# 2. Простой поиск по одному условию
```
db.books.find({ available: true })
```
<img width="408" height="174" alt="image" src="https://github.com/user-attachments/assets/72cd3368-7ce6-4d0c-b79a-3e5a88c4c032" />

# 3. Добавление нескольких документов
```
db.books.insertMany([
  {
    title: "JavaScript: The Good Parts",
    genre: "programming",
    price: 30,
    available: true,
    tags: ["javascript", "frontend"],
    author: {
      name: "Douglas Crockford",
      country: "USA"
    }
  },
  {
    title: "Harry Potter",
    genre: "fantasy",
    price: 25,
    available: false,
    tags: ["magic", "adventure"],
    author: {
      name: "J.K. Rowling",
      country: "UK"
    }
  },
  {
    title: "Data Structures",
    genre: "programming",
    price: 60,
    available: true,
    tags: ["algorithms", "cs"],
    author: {
      name: "Mark Weiss",
      country: "USA"
    }
  },
  {
    title: "Cooking Basics",
    genre: "cooking",
    price: 20,
    available: true,
    tags: ["food", "beginner"],
    author: {
      name: "Jamie Oliver",
      country: "UK"
    }
  }
])
```

# Запрос посложнее

```
db.books.find(
  {
    genre: "programming",
    price: { $gt: 40 },
    available: true
  },
  {
    _id: 0,
    title: 1,
    price: 1
  }
)
```
<img width="321" height="62" alt="image" src="https://github.com/user-attachments/assets/1a7e9196-fd5d-424a-a639-2c66ff16e479" />
