## CloudBeaver

**CloudBeaver** — это веб-версия популярного desktop-инструмента **DBeaver**.

1. Создайте отдельный каталог для **DBeaver**
```shell
mkdir DBeaver && cd DBeaver
```

2. Создайте в каталоге `DBeaver` файл `compose.yaml`

```yaml
services:
  cloudbeaver:
    image: dbeaver/cloudbeaver:latest
    container_name: cloudbeaver
    restart: unless-stopped
    ports:
      - "8978:8978"
    volumes:
      - ./workspace:/opt/cloudbeaver/workspace
```
3. Создайте проект (скачать нужные образы, создать контейнеры, запустить сервисы) командой:
```shell
docker compose up -d
```

<img width="726" height="102" alt="image" src="https://github.com/user-attachments/assets/c26023d1-20c5-4cf8-a152-e1e9394816ca" />


[После этого откройте http://localhost:8978](http://localhost:8978) и начните работу. Все ваши подключения и настройки сохранятся в папке `./workspace`

<img width="2551" height="1283" alt="image" src="https://github.com/user-attachments/assets/ba00a67b-0359-4829-b6c6-52c95257fef2" />

<img width="2544" height="1269" alt="image" src="https://github.com/user-attachments/assets/3b5cfb4d-4436-4c06-8f27-c81b4ecbd6e2" />


Создайте новый сервер с именем администратора `cbadmin` и своим паролем > 8 символов, включая хотя бы одну прописную и строчную букву

4. Управление проектом

### Состояние проекта как сервиса

Показать запущенные проекты
```shell
docker compose ps
```
<img width="1177" height="89" alt="image" src="https://github.com/user-attachments/assets/9c79dd7a-d207-4ba7-80ba-aea7352e1457" />

или все (в т.ч. остановленные)
```shell
docker compose ps -a
```

### Логи

```shell
docker compose logs cloudbeaver
```
<img width="1203" height="441" alt="image" src="https://github.com/user-attachments/assets/33c21af1-476d-4d45-874d-c7f03c504526" />

или в режиме ожидания (лучше запускать в отдедльном терминале)
```shell
docker compose logs -f cloudbeaver
```
<img width="1119" height="361" alt="image" src="https://github.com/user-attachments/assets/27204410-2b76-4133-a127-33278461ba53" />

### Остановка, запуск, вход и выход

Остановить сервис:
```shell
docker compose stop
```
<img width="444" height="82" alt="image" src="https://github.com/user-attachments/assets/1b58388d-9ee3-4e32-a52a-e326e8253465" />

Запустить остановленный сервис:
```shell
docker compose start
```

<img width="378" height="78" alt="image" src="https://github.com/user-attachments/assets/9af94c41-c781-4f54-bbfa-32264ed17b53" />

Перезапустить
```shell
docker compose restart
```

<img width="358" height="84" alt="image" src="https://github.com/user-attachments/assets/44710458-80c9-491d-b7da-db3976a6ff44" />

Показать конфигурацию текущего проекта:
```shell
docker compose config
```
<img width="589" height="387" alt="image" src="https://github.com/user-attachments/assets/463b9867-4069-4a30-a5a6-a044a45473be" />

Вход в сервис (имя контейнера можно узнать командой `docker compose ps`)
```shell
docker compose exec mysql bash
```
Выйти из сервиса
```shell
exit
```

### Удаление проекта

1. Остановка контейнеров этого проекта (нужно находится в папке проекта):
```shell
docker compose down
```
2. Остановка с полным удалением всех данных (тома, базы данных и файлы) - опционально:
```shell
docker compose down -v
```
(**Будьте осторожны:** эта команда удалит всё, что вы создали в проекте!).
3. Удалить образ проекта
```shell
docker image rm dbeaver/cloudbeaver:latest
```
4. Удалить каталог проекта
Выходим из каталога проекта
```shell
cd ..
```
и удаляем
```shell
rm -rf DBeaver
```
если вы в Linux, то возможно придётся использовать `sudo` или `su -`

### Полезные ссылки

- []()

> Если вы обнаружили ошибку в этом тексте - сообщите пожалуйста автору!
