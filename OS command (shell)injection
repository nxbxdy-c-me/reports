# Write-up: PortSwigger - OS command injection, simple case

## 1. Описание задачи
Цель: Обнаружить уязвимость OS Command Injection в функционале проверки остатков товара и определить имя текущего пользователя системы.
Платформа: PortSwigger Academy

## 2. Разведка (Reconnaissance)
При анализе карточки товара был обнаружен функционал "Check stock", который отправляет POST-запрос с параметрами productId и storeId.

![Описание товара](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/OS%20inj1.png)

## 3. Анализ и Вектор атаки (Analysis)
Сервер использует значение storeId как аргумент для запуска внутренней системной команды. Для проверки вводим спецсимвол конвейера (pipe) или разделитель команд.

Оригинальный запрос:
POST /product/stock HTTP/1.1
...
productId=1&storeId=1

Перехватываем запрос через burpsuite
![request](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/OS%20inj2%20.png)

Добавляем в поле storeId=1 пайп " | " и затем команду "whoami"
![edit request](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/OS%20inj3.png)

После отправки модифицированного запроса сервер выполнил команду и вернул результат выполнения в теле HTTP-ответа.
![response](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/OS%20inj4.png)

## Заключение
Данная уязвимость возникает из-за отсутствия фильтрации входных данных перед их передачей в командную оболочку (shell).
Рекомендации:
Использовать API-функции языка программирования вместо вызова системных команд.
Внедрить строгую валидацию (например, разрешить только числовые значения для storeId).
