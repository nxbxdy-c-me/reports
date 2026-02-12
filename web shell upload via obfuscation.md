# Write-up: PortSwigger -  RCE via Null Byte / Extension Obfuscation

## 1. Описание задачи
Цель: У лаборатории имеется уязвимость в функции загрузки изображений. Также часть расширений добавлена в чёрный список. Требуется обойти данный механизм защиты и получить данные другого пользователя.

## 2. Разведка (Reconnaissance)
Загрузка изображений в веб приложении ограничена. Некоторые расширения не удаётся загрузить, сервер выдаёт ошибку.

## 3. Анализ и Вектор атаки (Analysis)
Загружаем картинку с расширением png. Сервер её принимает. GET запрос перехватываем через burpsuite и закидываем в repeater, пока оставляем.

![upload](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/web1.png)
![upload1](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/web2.png)

Создаём локально файл с расширением .php и прописываем скрипт.

![script](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/web3.png)

При попытке загрузить данный файл сервер выдаёт ошибку.

![error](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/web4.png)

POST запрос также перехватываем и кидаем в Repeater, там добавляем нулевой байт (%00) к имени файла, сервер отрежет часть после нулевого байта при сохранении.

![upload3](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/web5.png)

Файл успешно загружен.

![upload4](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/web6.png)

Возвращаемся к первоначальному GET запросу задания. В пути запроса меняем имя файла на rce.php и отправляем запрос и получаем ответ.

![upload5](https://raw.githubusercontent.com/nxbxdy-c-me/screenshots/refs/heads/main/web7.png)

## Заключение
Рекомендации:
Whitelisting: Разрешать загрузку только строго определенных MIME-типов и расширений (jpg, jpeg, png), полностью блокируя всё остальное.
