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
- Node.js 20.x (опционально, для локальной разработки)
- Make

## Структура проекта

.
├── app/ # Приложение JS Fastify Blog
├── services/
│ └── caddy/
│ └── Caddyfile # Конфигурация обратного прокси
├── .github/
│ └── workflows/
│ └── push.yml # CI/CD pipeline
├── docker-compose.yml # Базовая конфигурация (production)
├── docker-compose.override.yml # Переопределение для разработки
├── Dockerfile # Для разработки
├── Dockerfile.production # Для production-сборки
├── Makefile # Команды для управления проектом
└── .env.example # Пример переменных окружения


## Быстрый старт

### Подготовка окружения

```bash
# Клонируйте репозиторий
git clone https://github.com/EgorYuraGeorge/devops-for-developers-project-74.git
cd devops-for-developers-project-74

# Скопируйте файл с переменными окружения
cp .env.example .env

# Установите зависимости
make setup

## Запуск в режиме разработки

bash

make dev

Приложение будет доступно по адресу: https://localhost

## Запуск тестов

bash

make test

## Production-сборка

bash

# Собрать образ
docker compose -f docker-compose.yml build app

# Запустить
docker compose -f docker-compose.yml up

## Docker Hub

Образ доступен на Docker Hub:
egoryurageorge/devops-for-developers-project-74
bash

docker pull egoryurageorge/devops-for-developers-project-74:latest
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

    make dev - Запуск приложения в режиме разработки

    make test - Запуск тестов

    make ci - Запуск тестов в CI-окружении

## CI/CD

При пуше в ветку main автоматически запускаются тесты и собирается Docker образ, который публикуется на Docker Hub.

## Технологии

    Node.js 20

    Fastify

    PostgreSQL

    Docker & Docker Compose

    Caddy (reverse proxy)

    GitHub Actions

    Sequelize ORM
