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
