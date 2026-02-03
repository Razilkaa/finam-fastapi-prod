# Calendar Generator API

FastAPI приложение для генерации экономического календаря в форматах Excel и Word.

## Структура проекта

```
main_auto/
├── app/
│   ├── __init__.py
│   ├── main.py                 # Точка входа приложения
│   ├── api/                    # API роутеры
│   │   ├── __init__.py
│   │   └── v1/
│   │       ├── __init__.py
│   │       └── endpoints/
│   │           ├── __init__.py
│   │           ├── calendar.py  # Эндпоинты календаря
│   │           └── template.py  # Управление Word-шаблоном
│   ├── core/                   # Конфигурация
│   │   ├── __init__.py
│   │   └── config.py           # Настройки приложения
│   ├── models/                 # Pydantic схемы
│   │   ├── __init__.py
│   │   └── schemas.py          # Модели данных
│   ├── services/               # Бизнес-логика
│   │   ├── __init__.py
│   │   ├── calendar_service.py # Обработка данных календаря
│   │   ├── data_store.py       # Хранилище данных
│   │   ├── excel_service.py    # Генерация Excel
│   │   ├── template_service.py # Управление Word-шаблоном
│   │   └── word_service.py     # Генерация Word
│   └── utils/                  # Утилиты
│       ├── __init__.py
│       ├── constants.py        # Константы
│       ├── date_utils.py       # Работа с датами
│       └── text_utils.py       # Обработка текста
├── Template.docx               # Word-шаблон по умолчанию (fallback)
├── volumes/
│   └── word-template/
│       └── Template.docx       # Активный Word-шаблон (создаётся после загрузки/первой генерации)
├── .env.example
├── Dockerfile
├── docker-compose.yml
└── requirements.txt
```

## Запуск

### Docker Compose

```bash
docker-compose up --build
```

### Локально

#### С использованием uv (рекомендуется)

```bash
# Установка uv (если еще не установлен)
# Windows: powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
# Linux/Mac: curl -LsSf https://astral.sh/uv/install.sh | sh

# Установка зависимостей
uv pip install -r requirements.txt

# Запуск приложения
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

#### С использованием pip (альтернатива)

```bash
pip install -r requirements.txt
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

## API Endpoints

- `GET /` - Информация об API
- `POST /api/calendar/receive` - Приём данных от n8n
- `GET /api/calendar/status` - Статус данных
- `GET /api/calendar/generate` - Генерация Excel файла
- `GET /api/calendar/generate-word` - Генерация Word файла
- `POST /api/calendar/clear` - Очистка данных
- `GET /api/template` - Информация о текущем Word-шаблоне
- `POST /api/template` - Загрузка нового Word-шаблона (.docx, multipart/form-data)
- `GET /api/template/download` - Скачать текущий Word-шаблон (.docx)

## Переменные окружения

=======

- `WORD_TEMPLATE_PATH` - Путь к активному Word-шаблону.

По умолчанию в `docker-compose.yml` шаблон хранится в volume (bind mount) и переживает перезапуски контейнеров:
- `WORD_TEMPLATE_PATH=/data/Template.docx`
- `./volumes/word-template:/data`

=======
- `WORD_TEMPLATE_PATH` - Путь к шаблону Word (по умолчанию: `/app/Template.docx`)

=======


В репозитории хранится файл `.env.example`.

При появлении паролей, токенов или ключей:
1. Скопировать `.env.example` в `.env`
2. Заполнить реальные значения в `.env`

Файл `.env` в репозиторий не коммитится.
