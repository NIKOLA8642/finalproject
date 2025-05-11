# Распределённый калькулятор с персистентностью и многопользовательским режимом

Этот проект представляет собой распределённый калькулятор с поддержкой пользователей, хранением данных в СУБД и аутентификацией через JWT.

## Функциональность

*   **Регистрация пользователя:** Создание новых учетных записей.
*   **Вход пользователя:** Получение JWT токена для аутентификации.
*   **Вычисление выражений:** Вычисление математических выражений с учётом контекста пользователя.
*   **Персистентность:** Сохранение данных между перезапусками сервиса.

## Запуск проекта

1.  Клонируйте репозиторий:

    ```
    git clone <ваш_репозиторий>
    cd <папка_проекта>
    ```

2.  Установите необходимые зависимости (если есть, например, через `go mod`):

    ```
    go mod tidy
    ```

3.  Настройте подключение к базе данных:

    *   Укажите параметры подключения в файле конфигурации (например, `config.yaml` или переменные окружения).
    *   Убедитесь, что база данных запущена и доступна.

4.  Запустите сервис:

    ```
    go run ./cmd/calc_service/...
    ```

## Использование

### Регистрация пользователя

curl --location 'localhost/api/v1/register'
--header 'Content-Type: application/json'
--data '{
"login": "user1",
"password": "password123"
}'

text

**Успешный ответ:** `200 OK`

**Неудачный ответ:** Соответствующий код ошибки и сообщение.

### Вход пользователя

curl --location 'localhost/api/v1/login'
--header 'Content-Type: application/json'
--data '{
"login": "user1",
"password": "password123"
}'

text

**Успешный ответ:** `200 OK` и JWT токен в теле ответа.

{
"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}

text

**Неудачный ответ:** Соответствующий код ошибки и сообщение.

### Вычисление выражения

curl --location 'localhost/api/v1/calculate'
--header 'Content-Type: application/json'
--header 'Authorization: Bearer <ваш_JWT_токен>'
--data '{
"expression": "2+2*2"
}'

text

**Успешный ответ:** `200 OK` и результат вычисления в теле ответа.

{
"result": 6
}

text

**Неудачный ответ:** Соответствующий код ошибки и сообщение.

## Примеры использования с ошибками

### Неверный логин или пароль при входе

curl --location 'localhost/api/v1/login'
--header 'Content-Type: application/json'
--data '{
"login": "wronguser",
"password": "wrongpassword"
}'

text

**Ожидаемый ответ:** Код ошибки `401 Unauthorized` и сообщение об ошибке.

### Отсутствие токена при вычислении

curl --location 'localhost/api/v1/calculate'
--header 'Content-Type: application/json'
--data '{
"expression": "2+2*2"
}'

text

**Ожидаемый ответ:** Код ошибки `401 Unauthorized` и сообщение об ошибке.

### Некорректное выражение для вычисления

curl --location 'localhost/api/v1/calculate'
--header 'Content-Type: application/json'
--header 'Authorization: Bearer <ваш_JWT_токен>'
--data '{
"expression": "2+2*"
}'

text

**Ожидаемый ответ:** Код ошибки `400 Bad Request` и сообщение об ошибке.

## Дополнительная информация

*   Описание структуры базы данных и используемых таблиц.
*   Описание алгоритмов вычисления выражений.
*   Информация о масштабировании и развёртывании проекта.
