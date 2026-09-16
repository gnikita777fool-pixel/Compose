***

```markdown
# LibreDB Studio

**LibreDB Studio** — это открытая (лицензия MIT) веб-IDE для работы с базами данных. Приложение разворачивается как Docker-контейнер непосредственно рядом с базой данных (на сервере или в облаке), а не на локальной машине разработчика.

По сути, это «браузерный аналог DataGrip/DBeaver» — единая точка входа для выполнения запросов, визуализации данных и администрирования.

### Преимущества
*   **Централизация:** Вместо установки десктопных приложений и поиска строк подключения каждым разработчиком, вы разворачиваете один контейнер на сервере.
*   **Доступность:** Пользователи заходят через браузер с любого устройства (включая мобильные), что критически важно для оперативного решения задач («горящих» запросов).
*   **Безопасность:** Доступ контролируется через единую точку входа с авторизацией.

---

## 📋 Предварительные требования

Перед началом работы убедитесь, что у вас установлен **Docker** и **Docker Compose**.

Проверьте другие запущенные приложения, чтобы избежать конфликтов портов:
```bash
docker compose ls
```

## 🚀 Быстрый старт

### 1. Создание каталога проекта

Создайте директорию и перейдите в нее:

```bash
mkdir -p libredb-studio && cd libredb-studio
```

Структура проекта будет следующей:
```text
libredb-studio/
├── compose.yaml
└── .env
```

### 2. Конфигурация Docker Compose

Создайте файл `compose.yaml`:

```bash
touch compose.yaml
```

Добавьте в него следующее содержимое:

```yaml
services:
  libredb-studio:
    image: ghcr.io/libredb/libredb-studio:latest
    container_name: libredb-studio
    ports:
      - "3000:3000"
    environment:
      # Email администратора
      ADMIN_EMAIL: ${ADMIN_EMAIL:-admin@libredb.org}
      # Пароль администратора (обязательно задайте в .env)
      ADMIN_PASSWORD: ${ADMIN_PASSWORD:?set ADMIN_PASSWORD in .env}
      # Секретный ключ для JWT (минимум 32 символа, обязательно задайте в .env)
      JWT_SECRET: ${JWT_SECRET:?set JWT_SECRET in .env (min 32 chars)}
      # Провайдер хранения конфигурации
      STORAGE_PROVIDER: sqlite
      STORAGE_SQLITE_PATH: /app/data/libredb-storage.db
    volumes:
      - libredb-data:/app/data
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "wget", "--spider", "-q", "http://localhost:3000"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

volumes:
  libredb-data:
```
<img width="1894" height="1059" alt="2" src="https://github.com/user-attachments/assets/d89b42cc-b016-4c30-a3bd-b0605278c052" />

### 3. Настройка переменных окружения (.env)

Создайте файл `.env` для хранения секретов. **Не коммитьте этот файл в публичные репозитории!**

```bash
cat > .env << 'EOF'
# Обязательные переменные
ADMIN_EMAIL=admin@libredb.org
ADMIN_PASSWORD=YourStrongPassword123!
JWT_SECRET=jirweH6r53yxlN0Ei/IjO4a6lYdi+k9iFrkdzD9BPrk=

# Опционально: обычный пользователь
USER_EMAIL=user@libredb.org
USER_PASSWORD=UserPassword123!
EOF
```
<img width="1880" height="1038" alt="1" src="https://github.com/user-attachments/assets/1e01506c-1cef-4481-8697-d9c03e0b5410" />

> ⚠️ **Важно:** Замените `ADMIN_PASSWORD` и `JWT_SECRET` на свои уникальные значения. `JWT_SECRET` должен быть длиной не менее 32 символов.

### 4. Запуск сервиса
#### Проверка порта
Убедитесь, что порт `3000` свободен:
```bash
ss -tulpn | grep :3000
```

#### Проверка имени контейнера
Убедитесь, что контейнер с именем `libredb-studio` еще не существует:
```bash
docker ps -a | grep libredb-studio
```

#### Запуск
Находясь в директории `libredb-studio`, выполните:

```bash
docker compose up -d
```

<img width="1782" height="131" alt="3" src="https://github.com/user-attachments/assets/6d4893d5-f4bd-4f89-945a-d3e912f3e676" />


### 5. Проверка статуса и логи

Проверьте, что сервис запустился:
```bash
docker compose ps -a
```

Просмотр последних 20 строк логов:
```bash
docker compose logs --tail=20 libredb-studio
```

Для просмотра логов в реальном времени (выйти через `Ctrl+C`):
```bash
docker compose logs -f
```

## 🔐 Вход в систему

Откройте браузер и перейдите по адресу:
👉 **[http://localhost:3000](http://localhost:3000)**

Используйте учетные данные из файла `.env`:

| Роль | Логин (Email) | Пароль |
| :--- | :--- | :--- |
| **Admin** | `admin@libredb.org` | `YourStrongPassword123!` |
| User | `user@libredb.org` | `UserPassword123!` |

*(Если вы изменили значения в `.env`, используйте их)*

<img width="2548" height="1267" alt="4" src="https://github.com/user-attachments/assets/1446dcd5-758e-417e-8988-2dfd50aa6e2f" />
<img width="2537" height="1257" alt="5" src="https://github.com/user-attachments/assets/c2c08ff1-55fd-4f4e-8051-f7cc1d23ab1f" />


## 🗑️ Удаление проекта

Если вам нужно полностью удалить приложение и данные:

1. **Остановить и удалить контейнеры + тома данных:**
   ```bash
   docker compose down -v
   ```

2. **Удалить образ:**
   ```bash
   docker image rm ghcr.io/libredb/libredb-studio:latest
   ```

3. **Проверить очистку:**
   ```bash
   docker ps -a | grep libredb-studio
   docker volume ls | grep libredb
   ```

4. **(Опционально) Полная очистка Docker (осторожно!):**
   Это удалит все неиспользуемые образы, контейнеры и сети.
   ```bash
   docker system prune -a --volumes
   ```

5. **Удалить файлы проекта:**
   ```bash
   cd ..
   rm -rf libredb-studio
   ```

---
