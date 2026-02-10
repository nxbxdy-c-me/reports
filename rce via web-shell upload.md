# Write-up: PortSwigger - Remote Code Execution via web-shell file upload

## 1. Описание задачи
Цель: У лаборатории отсутствует валидация загружаемых пользователем файлов. Требуется згрузить вредоносный код и получить данные другого пользователя.
Платформа: PortSwigger Academy

## 2. Разведка (Reconnaissance)
Функция загрузки изображений не выполняет никакой проверки загружаемых пользователями файлов перед их сохранением в файловой системе сервера.

![download](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/rce.png)

## 3. Анализ и Вектор атаки (Analysis)
Загружаем любое произвольное изображение и возвращаемся на главную страницу.

![file upload](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/rce1.png)

Перехваченный burpsuite запрос кидаем в repeater и пока оставляем.

![reqs](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/rce3.png)

Создаём на своём рабочем столе тип файла .php и прописываем туда скрипт.

![script](https://github.com/nxbxdy-c-me/screenshots/blob/main/rce4.png)

Возвращаемся в браузер и загружаем файл в окно изображений.
Заходим снова во вкладку repeater в burpsuite и редактируем запрос, указываем загруженный скрипт.
Нажимаем отправить, сервер обрабатывает запрос и выдаёт response.

![upload](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/rce5.png)

## Заключение
Сервер не только сохранил файл, но и выполнил код внутри файла с расширением .php
Рекомендации:
White-list расширений: Разрешать только .jpg, .jpeg, .png.
No-Execute Bit: Настройка веб-сервера (Apache/Nginx) так, чтобы в папке /uploads было запрещено выполнение любых скриптов.
