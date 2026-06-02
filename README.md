### Hexlet tests and linter status:
[![Actions Status](https://github.com/EgorYuraGeorge/devops-for-developers-project-74/actions/workflows/hexlet-check.yml/badge.svg)](https://github.com/EgorYuraGeorge/devops-for-developers-project-74/actions)
![CI Status](https://github.com/EgorYuraGeorge/devops-for-developers-project-74/workflows/CI/badge.svg)

# DevOps for Developers - Project 74

[![CI Status](https://github.com/EgorYuraGeorge/devops-for-developers-project-74/workflows/CI/badge.svg)](https://github.com/EgorYuraGeorge/devops-for-developers-project-74/actions)

## Описание

Проект демонстрирует упаковку приложения JS Fastify Blog в Docker с использованием Docker Compose. Включает настройку окружения разработки, тестирования и CI/CD.

## Требования к системе

- Docker версии 20.10 или выше
- Docker Compose версии 1.27.0 или выше
- Make

## Структура проекта

devops-for-developers-project-74/
├── .github/workflows/push.yml          # CI/CD пайплайн
├── app/                                # Приложение JS Fastify Blog
├── services/caddy/Caddyfile            # Конфигурация обратного прокси
├── .dockerignore                       # Исключения для Docker
├── .env.example                        # Пример переменных окружения
├── .gitignore                          # Исключения для Git
├── docker-compose.yml                  # Production конфигурация
├── docker-compose.override.yml         # Development конфигурация
├── Dockerfile                          # Для разработки
├── Dockerfile.production               # Для production
├── Makefile                            # Команды управления
└── README.md                           # Документация с бейджем

## Быстрый старт

### Подготовка окружения

# Клонируйте репозиторий

git clone https://github.com/EgorYuraGeorge/devops-for-developers-project-74.git
cd devops-for-developers-project-74

# Скопируйте файл с переменными окружения
cp .env.example .env

# Установите зависимости
make setup

### Запуск в режиме разработки

make dev

Приложение будет доступно по адресу: https://localhost

    Примечание: При первом запуске браузер покажет предупреждение о небезопасном соединении. Это нормально, так как Caddy использует самоподписанный сертификат для localhost. Нажмите "Дополнительно" → "Принять риск и продолжить".

## Запуск тестов

make test

Тесты выполняются в Docker с использованием PostgreSQL.

## Production-сборка

# Собрать образ
docker compose -f docker-compose.yml build app
# Запустить
docker compose -f docker-compose.yml up

## Docker Hub

# Загрузка образа
docker pull egoryurageorge/devops-for-developers-project-74:latest
# Запуск приложения из образа
docker run -p 8080:8080 -e NODE_ENV=development egoryurageorge/devops-for-developers-project-74 make dev

## Переменные окружения
Переменная	Описание	Значение по умолчанию
DATABASE_HOST	Хост базы данных	db
DATABASE_NAME	Имя базы данных	postgres
DATABASE_USERNAME	Пользователь БД	postgres
DATABASE_PASSWORD	Пароль БД	password
DATABASE_PORT	Порт БД	5432

## Сервисы

Проект состоит из следующих сервисов:

    app - Приложение JS Fastify Blog (порт 8080)

    db - PostgreSQL база данных (порт 5432)

    caddy - Обратный прокси-сервер (порты 80, 443)

## Makefile команды

Команда	Описание
make setup	Установка зависимостей приложения
make dev	Запуск приложения в режиме разработки
make test	Запуск тестов
make ci	Запуск тестов в CI-окружении (с пересборкой)

## CI/CD

При пуше в ветку main автоматически:

    Запускаются тесты в Docker с PostgreSQL

    При успешном прохождении тестов собирается Docker образ

    Образ публикуется на Docker Hub с тегом latest

## Технологии

    Node.js 22

    Fastify - веб-фреймворк

    PostgreSQL 18 - база данных

    Sequelize - ORM

    Docker & Docker Compose - контейнеризация

    Caddy - обратный прокси с автоматическим HTTPS

    GitHub Actions - CI/CD

    Webpack - сборка фронтенда

    Vitest - тестирование

## Примечания

    Приложение использует Sequelize с диалектом PostgreSQL для тестового и production окружения

    Локальная разработка использует SQLite через docker-compose.override.yml

    Caddy автоматически выпускает самоподписанный сертификат для localhost

    В CI используется принудительная пересборка образа без кеша для гарантии актуальности зависимостей