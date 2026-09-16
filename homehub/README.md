***

# HomeHub: Семейная панель управления

**HomeHub** — это универсальная, легковесная веб-панель для организации быта всей семьи. Разместите её на собственном сервере (идеально подходит для **Raspberry Pi**) и получите приватное пространство для общих задач.

> [Ссылка на оригинальный репозиторий Homehub](https://github.com/surajverma/homehub)

---

## 📋 Инструкция по установке

### 1. Подготовка окружения

Склонируйте репозиторий приложения:

```bash
git clone https://github.com/surajverma/homehub.git
```

Перейдите в папку проекта и создайте пустой файл конфигурации:

```bash
cd homehub && touch config.yml
```

### 2. Настройка Docker Compose

Откройте файл `compose.yml` (он уже существует в репозитории) и **замените** его содержимое на следующее:

```yaml
# compose.yml
services:
  homehub:
    container_name: homehub
    image: ghcr.io/surajverma/homehub:latest
    ports:
      - "5000:5000" # Приложение слушает порт 5000 внутри контейнера
    environment:
      - FLASK_ENV=production
      - SECRET_KEY=${SECRET_KEY:-} # Задается через .env или генерируется случайно
    volumes:
      - ./uploads:/app/uploads
      - ./media:/app/media
      - ./pdfs:/app/pdfs
      - ./data:/app/data
      - ./config.yml:/app/config.yml:ro
```

<img width="1895" height="1335" alt="1" src="https://github.com/user-attachments/assets/4247f7cf-1308-45fe-b2ad-d7dab629800f" />

### 3. Конфигурация приложения (`config.yml`)

Откройте созданный ранее файл `config.yml` и вставьте следующую конфигурацию. Вы можете изменить имена членов семьи, цвета темы и включенные функции под свои нужды.

```yaml
instance_name: "My Home Hub"
password: "" # Оставьте пустым для доступа без пароля
admin_name: "Administrator"

feature_toggles:
  shopping_list: true
  media_downloader: true
  pdf_compressor: true
  qr_generator: true
  notes: true
  shared_cloud: true
  who_is_home: true
  personal_status: true
  chores: true
  recipes: true
  expiry_tracker: true
  url_shortener: true
  expense_tracker: true

family_members:
  - Mom
  - Dad
  - Dipanshu
  - Vivek
  - India

reminders:
  # Формат времени: "12h" (по умолчанию) или "24h"
  time_format: 12h
  
  # День начала недели в календаре: sunday, monday и т.д.
  calendar_start_day: monday 

  # Категории напоминаний
  categories:
    - key: health
      label: Health
      color: "#dc2626"
    - key: bills
      label: Bills
      color: "#0d9488"
    - key: school
      label: School
      color: "#7c3aed"
    - key: family
      label: Family
      color: "#2563eb"

# Настройки темы (опционально)
theme:
  primary_color: "#1d4ed8"
  secondary_color: "#a0aec0"
  background_color: "#f7fafc"
  card_background_color: "#fff"
  text_color: "#333"
  sidebar_background_color: "#2563eb"
  sidebar_text_color: "#ffffff"
  sidebar_link_color: "rgba(255,255,255,0.95)"
  sidebar_link_border_color: "rgba(255,255,255,0.18)"
  sidebar_active_color: "#3b82f6"
```

<img width="1896" height="1325" alt="2" src="https://github.com/user-attachments/assets/cc325922-695c-47f3-be7c-a10284123af9" />

### 4. Запуск проекта

Находясь в корневой папке `homehub`, запустите контейнер в фоновом режиме:

```bash
docker compose up -d
```

<img width="1896" height="126" alt="3" src="https://github.com/user-attachments/assets/0a8d0743-9949-40f7-bec3-8c291a45cf80" />


#### Проверка работоспособности

1.  **Проверить статус контейнера:**
    ```bash
    docker compose ps
    ```

2.  **Просмотр логов (в реальном времени):**
    ```bash
    docker compose logs -f
    ```
    *(Для выхода из режима логов нажмите `Ctrl+C`)*

3.  **Открыть приложение:**
    Перейдите в браузере по адресу: [http://localhost:5000](http://localhost:5000)


---

<img width="2538" height="1266" alt="4" src="https://github.com/user-attachments/assets/81dfa170-263d-465e-9490-1d85e6a1fc60" />


## 🗑️ Удаление проекта

Если вы хотите полностью удалить HomeHub и все связанные данные, используйте один из следующих способов.

### Способ 1: Полная очистка (Рекомендуемый)

Эта команда остановит контейнер, удалит тома с данными и образы.

```bash
docker compose down --rmi all -v
```

Затем удалите папку проекта:

```bash
cd ..
rm -rf homehub
```

### Способ 2: Пошаговое удаление

1.  Остановить контейнер и удалить тома:
    ```bash
    docker compose down -v
    ```
2.  Проверить, что контейнеры остановлены:
    ```bash
    docker ps -a
    # или
    docker compose ps -a
    ```
3.  Найти ID образа:
    ```bash
    docker images
    ```
4.  Удалить образ вручную (замените `id-образа` на реальный ID):
    ```bash
    docker rmi id-образа
    ```
5.  Выйти из директории и удалить файлы:
    ```bash
    cd ..
    rm -rf homehub
    ```

---
