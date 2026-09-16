

```markdown
# Docker Compose: PostgreSQL + pgAdmin

Данный проект представляет собой готовую конфигурацию для развёртывания связки **PostgreSQL** и **pgAdmin** с использованием Docker Compose.

- **PostgreSQL** (часто — Postgres) — свободная объектно-реляционная система управления базами данных (ORDBMS) с открытым исходным кодом.
- **pgAdmin** — официальный графический инструмент для администрирования PostgreSQL.

---

## ⚠️ Предварительная подготовка

Перед началом работы проверьте другие запущенные `docker-compose` приложения, чтобы снизить риск возникновения конфликтов использования портов (особенно `5432` и `5050`):

```bash
docker compose ls
```
Если есть активные проекты, их рекомендуется временно остановить.

---

## 1. Создание каталога проекта

Необходимая структура проекта:
```text
postgres-pgadmin-app/
└── compose.yaml
```

Создать структуру и перейти в неё можно одной bash-командой:

```bash
mkdir -p postgres-pgadmin-app && cd postgres-pgadmin-app && touch compose.yaml
```

---

## 2. Конфигурация `compose.yaml`

Откройте файл `compose.yaml` (или `docker-compose.yml` для совместимости со старыми версиями) и добавьте следующую конфигурацию:

```yaml
services:
  postgres:
    image: postgres:17-alpine
    container_name: postgres-db
    environment:
      POSTGRES_USER: myuser
      POSTGRES_PASSWORD: mypassword
      POSTGRES_DB: mydatabase
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  pgadmin:
    image: dpage/pgadmin4:latest
    container_name: pgadmin-web
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@example.com
      PGADMIN_DEFAULT_PASSWORD: admin
    ports:
      - "5050:80"

volumes:
  postgres_data:
```

<img width="1892" height="466" alt="2" src="https://github.com/user-attachments/assets/dce3ae3d-a688-4164-bdb6-83ef44aae576" />
---

## 3. Установка и запуск

Находясь в терминале в папке `postgres-pgadmin-app`, выполните команду для запуска всех сервисов в фоновом режиме:

```bash
docker compose up -d
```
<img width="1790" height="190" alt="1" src="https://github.com/user-attachments/assets/c3d4d3af-5f93-4095-ba96-f7de8b42a89f" />


Дождитесь полной загрузки образов и запуска контейнеров. Убедиться, что всё работает, можно командой:

```bash
docker compose ps -a
```
> Оба контейнера (`postgres-db` и `pgadmin-web`) должны иметь статус **Up**.

---

## 4. Доступ к pgAdmin

Откройте в браузере адрес: [http://localhost:5050](http://localhost:5050)

На странице входа используйте данные, указанные в переменных окружения:
- **Email/Username:** `admin@example.com`
- **Password:** `admin`

<img width="2548" height="1274" alt="3" src="https://github.com/user-attachments/assets/48247dc1-c403-4df5-9c1c-76ab49d7be76" />
<img width="2545" height="1267" alt="4" src="https://github.com/user-attachments/assets/8f252ffc-eb29-45ce-8ec9-fb97f05f7229" />

---

## 5. Подключение pgAdmin к PostgreSQL

1. В интерфейсе pgAdmin нажмите **Add New Server**.
2. На вкладке **General** задайте любое понятное имя для сервера (например, `My Local PostgreSQL`).
3. Перейдите на вкладку **Connection** и заполните следующие поля:
   - **Host name/address:** `postgres-db` *(имя сервиса PostgreSQL из файла `compose.yaml`)*
   - **Port:** `5432`
   - **Maintenance database:** `mydatabase`
   - **Username:** `myuser`
   - **Password:** `mypassword`
4. Нажмите **Save**.

<img width="2542" height="1270" alt="6" src="https://github.com/user-attachments/assets/ce54cfe3-6327-4011-94be-64d1c054aded" />
<img width="2550" height="1272" alt="7" src="https://github.com/user-attachments/assets/bed98bd9-6f60-4057-8988-f7ec7c8852eb" />

---

## 6. Управление и полезные команды

Находясь в папке `postgres-pgadmin-app`, вы можете использовать следующие команды:

1. **Просмотр логов pgAdmin в реальном времени:**
   ```bash
   docker compose logs -f pgadmin
   ```
   *(Флаг `-f` включает режим ожидания. Для выхода нажмите `Ctrl+C`)*

2. **Просмотр логов PostgreSQL в реальном времени:**
   ```bash
   docker compose logs -f postgres
   ```
   *(Для выхода нажмите `Ctrl+C`)*

3. **Приостановить запущенные контейнеры:**
   ```bash
   docker compose stop
   ```

4. **Запустить приостановленные контейнеры:**
   ```bash
   docker compose start
   ```

5. **Перезапустить контейнеры:**
   ```bash
   docker compose restart
   ```

6. **Показать итоговую конфигурацию текущего проекта:**
   ```bash
   docker compose config
   ```

7. **Вход в контейнер PostgreSQL** (интерактивная bash-сессия):
   ```bash
   docker compose exec postgres bash
   ```
   ![Screen](/content/Docker/DockerCompose/img/20.png)
   *(Для выхода из контейнера введите команду `exit`)*

---

## 7. Удаление проекта

Находясь в папке `postgres-pgadmin-app`:

1. **Остановка и удаление контейнеров проекта:**
   ```bash
   docker compose down
   ```

2. **Полная остановка с удалением всех данных (базы данных и томов):** *(Опционально)*
   ```bash
   docker compose down --volumes
   # или кратко:
   docker compose down -v
   ```
   > ⚠️ **Будьте осторожны:** эта команда безвозвратно удалит всё, что вы создали и сохранили в базах данных этого проекта!

### Полная очистка
Для полного удаления проекта выполните следующие шаги:
```bash
# 1. Остановить и удалить контейнеры и тома
docker compose down -v

# 2. Выйти из каталога проекта
cd ..

# 3. Удалить каталог проекта
rm -rf postgres-pgadmin-app
```
*(При необходимости также можно удалить загруженные Docker-образы командой `docker rmi postgres:17-alpine dpage/pgadmin4:latest`)*

---
