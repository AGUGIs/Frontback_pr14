# Тестирование API с помощью Postman (ПР3)

## Тестирование внутреннего API

### Запрос 1: GET /api/products — Получение списка товаров
- **Метод:** GET
- **URL:** http://localhost:3000/api/products
- **Ожидаемый результат:** Массив товаров (JSON), статус 200

### Запрос 2: POST /api/auth/register — Регистрация пользователя
- **Метод:** POST
- **URL:** http://localhost:3000/api/auth/register
- **Body (JSON):**
```json
{
  "email": "test@mail.ru",
  "first_name": "Тест",
  "last_name": "Тестов",
  "password": "qwerty123",
  "role": "seller"
}
```
- **Ожидаемый результат:** Объект пользователя, статус 201

### Запрос 3: POST /api/auth/login — Вход в систему
- **Метод:** POST
- **URL:** http://localhost:3000/api/auth/login
- **Body (JSON):**
```json
{
  "email": "test@mail.ru",
  "password": "qwerty123"
}
```
- **Ожидаемый результат:** `{ accessToken, refreshToken }`, статус 200

### Запрос 4: POST /api/products — Создание товара (с токеном)
- **Метод:** POST
- **URL:** http://localhost:3000/api/products
- **Headers:** `Authorization: Bearer <accessToken>`
- **Body (JSON):**
```json
{
  "title": "Новый товар",
  "category": "Тест",
  "description": "Описание",
  "price": 999,
  "stock": 10
}
```
- **Ожидаемый результат:** Объект товара, статус 201

### Запрос 5: GET /api/auth/me — Получение текущего пользователя (с токеном)
- **Метод:** GET
- **URL:** http://localhost:3000/api/auth/me
- **Headers:** `Authorization: Bearer <accessToken>`
- **Ожидаемый результат:** Объект пользователя, статус 200

## Тестирование внешнего API

Для тестирования внешних API рекомендуется использовать один из открытых API:

### Пример: JSONPlaceholder (https://jsonplaceholder.typicode.com)

1. **GET** https://jsonplaceholder.typicode.com/posts — список постов
2. **GET** https://jsonplaceholder.typicode.com/posts/1 — пост по ID
3. **POST** https://jsonplaceholder.typicode.com/posts — создание поста
4. **PUT** https://jsonplaceholder.typicode.com/posts/1 — обновление поста
5. **DELETE** https://jsonplaceholder.typicode.com/posts/1 — удаление поста

> Примечание: скриншоты Postman необходимо сделать самостоятельно при запуске проекта на локальной машине и добавить в данную директорию.
