## Дисклеймер

Инструмент работает только с открытыми данными и чатами, где состоит ваш аккаунт.  
Используйте его в рамках законодательства вашей страны и правил Telegram.

# Telegram OSINT TXT Tool

Инструмент для OSINT-аналитики Telegram-аккаунтов и каналов.  
Собирает информацию о профиле, публичные сообщения, извлекает ссылки и контакты, анализирует активность - все автоматически и сохраняет в текстовые файлы.

## Быстрый старт

```bash
python main.py analyze "@username"

python main.py analyze @username
```

Инструмент автоматически выполнит полный анализ и сохранит все результаты в папке `output/`.

## Команды

### analyze - полный анализ цели

Выполняет все операции автоматически:
1. Снимок профиля - ID, username, имя, тип, время снимка
2. Глубокий поиск сообщений - поиск во всех открытых источниках
3. Общие чаты - где вы оба состоите, роль цели
4. Извлечение ссылок - все URL из сообщений
5. Извлечение алиасов - email, телефоны, другие username
6. Анализ активности - статистика по часам, дням недели
7. Генерация Google dorks - автоматическое создание запросов

Примеры:
```bash
python main.py analyze "@username"
python main.py analyze "@username" --limit 1000
python main.py analyze "@username" --skip-common-chats
```

### full-osint - полный OSINT с внешними инструментами

Выполняет полный анализ с интеграцией внешних OSINT инструментов:
1. Telegram анализ (снимок, сообщения, алиасы, карта чатов, темы)
2. Blackbird - поиск аккаунтов по username и email
3. Holehe - проверка email на различных сервисах
4. Phone OSINT - проверка номера телефона
5. Генерация итогового отчета

Пример:
```bash
python main.py full-osint "@username"
python main.py full-osint "@username" --limit 1000
```

### report - генерация итогового отчета

Создает единый отчет со всей собранной информацией по цели.

Пример:
```bash
python main.py report "@username"
```

### extract-aliases - расширенное извлечение алиасов

Извлекает все алиасы и контакты в расширенном формате:
- Username/логины
- Email адреса
- Телефоны
- Криптокошельки (BTC, ETH и др.)
- Ссылки на социальные сети (VK, Instagram, Twitter, GitHub и др.)

Пример:
```bash
python main.py extract-aliases "@username"
```

### chat-map - карта чатов и ролей

Строит карту чатов, где проявлялась цель:
- Chat ID, username, title
- Роль цели (создатель/админ/участник)
- Количество сообщений в каждом чате

Пример:
```bash
python main.py chat-map "@username"
```

### topics - тематический анализ

Анализирует темы сообщений:
- Частотный словарь (топ-50 слов)
- Ключевые темы (криптовалюты, города, технологии и др.)
- Географические названия

Пример:
```bash
python main.py topics "@username"
```

### monitor - автоматический мониторинг

Обрабатывает все цели из `targets.txt`:
- Делает снимки профилей
- Собирает новые сообщения
- Проверяет триггеры из `keywords.txt`
- Создает отчеты об изменениях

Пример:
```bash
python main.py monitor
```

### google-dorks - генерация Google dorks запросов

Создает список Google dorks запросов на основе собранных данных.

Пример:
```bash
python main.py google-dorks "@username"
```

### history - история профиля

Показывает все ранее сделанные снимки профиля.

Пример:
```bash
python main.py history "@username"
```

### search - поиск по сообщениям

Ищет по тексту в собранных сообщениях.

Пример:
```bash
python main.py search "@username" --text "ключевое слово"
```

## Дополнительные OSINT команды

### Интеграция с Blackbird

Blackbird - инструмент для поиска аккаунтов по username и email.

Установка:
```bash
git clone https://github.com/p1ngul1n0/blackbird.git
cd blackbird
```

Использование:
```bash
python main.py blackbird-username "someuser"
python main.py blackbird-email "test@example.com"
```

Результаты сохраняются в `output/blackbird_username_<username>_<timestamp>.txt` и `output/blackbird_email_<email>_<timestamp>.txt`.

### Интеграция с Holehe

Holehe - инструмент для проверки, на каких сервисах зарегистрирован email.

Установка:
```bash
pip install holehe
```

Использование:
```bash
python main.py holehe-email "test@example.com"
```

Результаты сохраняются в `output/holehe_email_<email>_<timestamp>.txt`.

### Интеграция с Phone OSINT

PhoneOsint - инструмент для проверки номера телефона.

Установка:
```bash
git clone https://github.com/kalmux1/PhoneOsint.git
cd PhoneOsint
```

Использование:
```bash
python main.py phone-osint "+79991234567"
```

Результаты сохраняются в `output/phone_<phone>_<timestamp>.txt`.

Примечание: Если внешний инструмент не установлен, команда выведет понятное сообщение об ошибке и создаст файл с инструкцией по установке.

## Требования

- Python 3.10+
- Аккаунт Telegram с `api_id` и `api_hash` с https://my.telegram.org

Опционально (для полного функционала):
- Blackbird (для поиска аккаунтов)
- PhoneOsint (для проверки телефонов)

## Установка

### 1. Основные зависимости

```bash
git clone https://github.com/exe1011101/telegram-osint-txt-tool.git
cd telegram-osint-txt-tool

python -m venv .venv

.venv\Scripts\activate
source .venv/bin/activate

pip install -r requirements.txt

python -m pip install -r requirements.txt
```

### 2. Настройка Telegram API

1. Перейдите на https://my.telegram.org/apps
2. Войдите в свой аккаунт Telegram
3. Создайте новое приложение
4. Скопируйте `api_id` и `api_hash`
5. Откройте файл `config.py` и вставьте свои данные:

```python
API_ID = ваш_api_id
API_HASH = "ваш_api_hash"
SESSION_NAME = "osint_session"
OUTPUT_DIR = "output"
```

### 3. Внешние инструменты (опционально)

#### Blackbird (поиск аккаунтов)

```bash

git clone https://github.com/p1ngul1n0/blackbird.git
cd blackbird


pip install -r requirements.txt

cd ..
```

#### PhoneOsint (проверка телефонов)

```bash

git clone https://github.com/kalmux1/PhoneOsint.git

cd PhoneOsint

pip install -r requirements.txt

cd ..
```

**Примечание:** Внешние инструменты опциональны. Инструмент будет работать без них, но соответствующие команды будут выводить сообщения об ошибке с инструкциями по установке.

### 4. Проверка установки

```bash
python main.py --help
```

Должен отобразиться список доступных команд.

## Настройка мониторинга

### Файл targets.txt

Создайте файл `targets.txt` в корне проекта:

```
@username1|Описание цели 1
123456789|Описание цели 2 (по ID)
@channel_name|Канал для мониторинга
```

Формат: `username|описание` (каждая строка - одна цель)

### Файл keywords.txt

Создайте файл `keywords.txt` для настройки триггеров:

```
ключевое слово
важная фраза
email
bitcoin
```

При обнаружении этих слов в сообщениях создаются файлы с алертами.

## Структура результатов

После выполнения `analyze` или `full-osint` создаются файлы:

```
output/
├── dossier_<target>_<timestamp>.txt        - Полное досье
├── google-dorks_<target>_<timestamp>.txt   - Google dorks запросы
├── aliases_<target>_<timestamp>.txt         - Расширенные алиасы
├── chats_<target>_<timestamp>.txt           - Карта чатов
├── topics_<target>_<timestamp>.txt          - Тематический анализ
├── geo_<target>_<timestamp>.txt             - Географические данные
└── report_<target>_<timestamp>.txt          - Итоговый отчет
```

После выполнения `full-osint` дополнительно создаются:
```
output/
├── blackbird_username_<username>_<timestamp>.txt
├── blackbird_email_<email>_<timestamp>.txt
├── holehe_email_<email>_<timestamp>.txt
├── phone_<phone>_<timestamp>.txt
└── full_report_<target>_<timestamp>.txt     - Сводный отчет
```

## Автоматизация

Для автоматического запуска мониторинга используйте планировщик задач:

Windows (Task Scheduler):
- Создайте задачу, которая запускает: `python main.py monitor`
- Установите расписание (например, каждые 6 часов)

Linux/Mac (cron):
```bash
crontab -e

0 */6 * * * cd /path/to/telegram-osint-txt-tool && .venv/bin/python main.py monitor
```

## Использование результатов

После анализа используйте полученные данные:
- Ссылки - проверьте домены, найдите другие соцсети
- Алиасы - ищите по email/телефонам в других сервисах
- Карта чатов - изучайте коммьюнити цели
- Тематический анализ - понимайте интересы и активность
- Google dorks - используйте готовые запросы для поиска
- Внешние инструменты - результаты Blackbird, Holehe, Phone OSINT
