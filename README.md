# devops_test

## Запуск

```bash
docker-compose up --build
```

## Проверка

```bash
curl http://localhost/
```

Ожидаемый ответ: `Hello from Effective Mobile!`

## Как работает

Клиент отправляет запрос на `localhost:80` → nginx слушает порт 80 и проксирует запрос на upstream `backend:8080` → Python-сервер в контейнере `backend` отвечает текстом. Оба контейнера в сети `localnet`, связь между ними идёт по имени сервиса.
```
Клиент
  │
  │  http://localhost:80
  ▼
┌─────────────────────────────────────────┐
│  nginx (контейнер)                      │
│  listen 80                              │
│  proxy_pass http://backend              │
│  заголовки: Host, X-Real-IP,            │
│            X-Forwarded-For,             │
│            X-Forwarded-Proto            │
└─────────────────────────────────────────┘
  │
  │  proxy → backend:8080 (имя сервиса)
  ▼
┌─────────────────────────────────────────┐
│  backend (контейнер)                    │
│  Python http.server                     │
│  GET / → "Hello from Effective Mobile!" │
└─────────────────────────────────────────┘
```

Сеть: localnet (bridge)
Взаимодействие между контейнерами по имени сервиса.
Backend не доступен напрямую с хоста.

## Использованные технологии

- **Docker / Docker Compose** — контейнеризация и оркестрация двух сервисов
- **Nginx** — обратный прокси, слушает порт 80, передаёт запросы на backend
- **Python(http.server)** — простой HTTP-сервер, отвечает на GET / текстом
- **Alpine Linux** — облегчённый образ
- **Docker bridge network (localnet)** — изолированная сеть для взаимодействия контейнеров
- **DNS имён сервисов Docker Compose** — обращение к backend по имени сервиса из nginx
