### Hexlet tests and linter status:
[![Actions Status](https://github.com/EgorYuraGeorge/devops-for-developers-project-74/actions/workflows/hexlet-check.yml/badge.svg)](https://github.com/EgorYuraGeorge/devops-for-developers-project-74/actions)
![CI Status](https://github.com/EgorYuraGeorge/devops-for-developers-project-74/workflows/CI/badge.svg)

# DevOps for Developers - Project 74

[![CI Status](https://github.com/EgorYuraGeorge/devops-for-developers-project-74/workflows/CI/badge.svg)](https://github.com/EgorYuraGeorge/devops-for-developers-project-74/actions)

## Описание

Проект демонстрирует упаковку приложения JS Fastify Blog в Docker с использованием Docker Compose. Включает настройку окружения разработки, тестирования и CI/CD.

## Требования к системе

* Docker версии 20.10 или выше
* Docker Compose версии 1.27.0 или выше
* Make

## Структура проекта

```text
.
├── .github/
│   └── workflows/
│       └── push.yml                     # CI/CD pipeline
├── app/                                # Приложение JS Fastify Blog
├── services/
│   └── caddy/
│       └── Caddyfile                    # Конфигурация обратного прокси
├── .dockerignore                        # Файлы, исключаемые из контекста сборки Docker
├── .env.example                         # Пример файла с переменными окружения
├── .gitignore                           # Файлы, исключаемые из Git
├── docker-compose.yml                   # Базовая конфигурация (production/тесты)
├── docker-compose.override.yml          # Переопределение для разработки (Caddy, volumes)
├── Dockerfile                           # Для разработки (Node.js 22)
├── Dockerfile.production                # Для production-сборки (Node.js 22)
├── Makefile                             # Команды для управления проектом
└── README.md                            # Документация проекта
```

## Быстрый старт

### Подготовка окружения

```bash
# Клонируйте репозиторий
git clone https://github.com/EgorYuraGeorge/devops-for-developers-project-74.git
cd devops-for-developers-project-74

# Создайте файл с переменными окружения

# Linux/macOS:
cp .env.example .env

# Windows (PowerShell):
Copy-Item .env.example .env

# Установите зависимости
make setup
```

## Запуск в режиме разработки

```bash
make dev
```

Приложение работает на порту 8080 внутри Docker-сети. Caddy принимает запросы извне на портах 80 и 443, проксируя их в приложение. Доступ осуществляется по адресу:

```text
https://localhost
```

> Примечание: При первом запуске браузер покажет предупреждение о небезопасном соединении. Это нормально, так как Caddy использует самоподписанный сертификат для localhost. Нажмите "Дополнительно" → "Принять риск и продолжить".

## Запуск тестов

```bash
make test
```

Тесты выполняются в Docker с использованием PostgreSQL. Используется файл `docker-compose.yml` без override.

## Production-сборка

```bash
# Собрать образ
docker compose -f docker-compose.yml build app

# Запустить production-версию
docker compose -f docker-compose.yml up
```

## Docker Hub

Образ доступен на Docker Hub:

```text
egoryurageorge/devops-for-developers-project-74
```

```bash
# Загрузка образа
docker pull egoryurageorge/devops-for-developers-project-74:latest

# Запуск production-версии
docker run -p 8080:8080 egoryurageorge/devops-for-developers-project-74
```

Приложение запустится с `NODE_ENV=production` (установлено в `Dockerfile.production`).

## Переменные окружения

Файл `.env.example` содержит все необходимые переменные для подключения к базе данных. Эти переменные используются как в production, так и в тестовом окружении.

| Переменная        | Назначение       | Значение по умолчанию |
| ----------------- | ---------------- | --------------------- |
| DATABASE_HOST     | Хост базы данных | db                    |
| DATABASE_NAME     | Имя базы данных  | postgres              |
| DATABASE_USERNAME | Пользователь БД  | postgres              |
| DATABASE_PASSWORD | Пароль БД        | password              |
| DATABASE_PORT     | Порт БД          | 5432                  |

Все переменные обязательны для работы приложения с PostgreSQL. При локальной разработке они загружаются из файла `.env`. В CI значения заданы по умолчанию в `docker-compose.yml`.

## Сервисы

Проект состоит из следующих сервисов:

* app — Приложение JS Fastify Blog (внутренний порт 8080)
* db — База данных PostgreSQL (внутренний порт 5432)
* caddy — Обратный прокси-сервер (внешние порты 80, 443)

Схема запросов:

```text
Browser → https://localhost:443 → Caddy → http://app:8080
```

## Makefile команды

| Команда    | Описание                                                           |
| ---------- | ------------------------------------------------------------------ |
| make setup | Установка зависимостей приложения                                  |
| make dev   | Запуск приложения в режиме разработки                              |
| make test  | Запуск тестов                                                      |
| make ci    | Запуск тестов в CI-окружении (с принудительной пересборкой образа) |

## CI/CD

При пуше в ветку `main` GitHub Actions автоматически:

* Пересобирает образ с флагом `--no-cache`
* Запускает тесты в Docker с PostgreSQL
* При успешном прохождении тестов публикует образ на Docker Hub с тегом `latest`

## Технологии

* Node.js 22
* Fastify — веб-фреймворк
* PostgreSQL — база данных
* Sequelize — ORM
* Docker & Docker Compose — контейнеризация
* Caddy — обратный прокси с автоматическим HTTPS
* GitHub Actions — CI/CD
* Webpack — сборка фронтенда
* Vitest — тестирование

## Примечания

* Приложение использует Sequelize с диалектом postgres для тестового и production окружения
* Caddy автоматически выпускает самоподписанный TLS-сертификат для localhost
* В CI используется принудительная пересборка образа без кеша (`--no-cache`) для гарантии актуальности зависимостей
* `docker-compose.override.yml` автоматически применяется при запуске `docker compose up` и добавляет Caddy, проброс портов и монтирование томов для разработки