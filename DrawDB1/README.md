## Docker compose проект c drawDB

**DrawDB** — бесплатный, простой и интуитивно понятный редактор схем баз данных и генератор SQL прямо в браузере

[Ссылка на drawDB в Github](https://github.com/drawdb-io/drawdb)

Перед началом работы над этим проектом, проверье другие запущенные у вас **docker-compose** приложения:
```shell
docker compose ls
```
их лучше остановить, чтобы снизить риск возникновения конфликтов использования портов!

### 1. Получение drawDB из аппстрима

> appstream (аппсмтрим) - автор-разработчик, предоставляющий единую инфраструктуру для описания своего программного обеспечения

Клонируем репозиторий (клонируйте в корень домашней папки текущего пользователя `cd ~`):
```shell
git clone https://github.com/drawdb-io/drawdb
```
<img width="394" height="531" alt="image" src="https://github.com/user-attachments/assets/4ba0ff90-eea1-4389-9ec5-504dba53cf26" />

Переходим в локальную папку склонированного репозитория
```shell
cd drawdb/
```

### 2. Установка и запуск drawDB локально

Запускаем проект
```shell
docker compose up -d
```
Убедиться, что проект запущен
```shell
docker compose ls
```
> Внимание! Проекту нужно несколько минут для запуска контейнера, подождите немного, прежде чем открыть его в браузере

[Открыть **drawDB** локально в браузере](http://localhost:5173/)

<img width="2544" height="1201" alt="image" src="https://github.com/user-attachments/assets/aaa25f4e-d90c-405f-a9c4-9d0100e987c7" />


### 3. Удалить проект

1. Остановить контейнер с удалением данных
```shell
docker compose down -v
```
2. Проверить, не запущен ли удаляемый контейнер
```shell
docker ps -a
```
и
```shell
docker compose ps -a
```
3. Получить id образа
```shell
docker images
```
4. Удалить образ
```shell
docker rmi id-образа
```
5. Удалить каталог проекта
```shell
rm -rf drawdb
```

Упрощённый вариант способ удаления этого проекта:

1. Остановить и удалить контейнер с данными
```shell
docker compose down --rmi all -v
```
2. Выйти из каталога
```shell
cd ..
```
3. Удалить каталог проекта
```shell
rm -rf drawdb
```
```mermaid
erDiagram
    "Люк Скайволкер" ||--|| "Ситх" : "references"
    "Люк Скайволкер" ||--|| "Джедай" : "references"
    "Джедай" ||--|| "Но возможно" : "references"
    "Ситх" ||--|| "Но возможно" : "references"
    "Люк Скайволкер" {
        INTEGER id
        INTEGER not_very_cool "Не очень крутой перец"
        INTEGER cool "Крутой перец"
    }
    "Ситх" {
        INTEGER Id
        INTEGER da "Да"
    }
    "Джедай" {
        INTEGER id
        INTEGER net "Нет"
    }
    "Но возможно" {
        INTEGER id
        INTEGER obivan "Обиван"
        INTEGER dart_veider "Дарт Вейдер"
    }
```
---
> Если вы обнаружили ошибку в этом тексте - сообщите пожалуйста автору!
