# Momo Store — Docker


## Запуск

```bash
sudo docker compose up -d --build
```

### Dev-профиль (без gateway)

```bash
sudo docker compose --profile dev up -d --build
```

В dev профиле поднимутся `backend-dev` (порт 8081) и `frontend-dev` (порт 80) без reverse-proxy.

Открыть:
- **Frontend**: `http://localhost/momo-store/` (порт 80)
- **Backend (API через gateway)**: `http://localhost:8081` (порт 8081)

Проверки:

```bash
curl -i http://localhost/health
curl -i http://localhost:8081/health
curl -i http://localhost:8081/products
```

Остановка:

```bash
sudo docker compose down
```

## Порты и доступность

- **frontend**: внутри контейнера `8080`, на хосте `80:8080`
- **gateway**: на хосте `8081:8081`
- **backend**: наружу не публикуется (только `expose: 8081` внутри `backend-net`)

## Лимиты ресурсов

В `docker-compose.yml` заданы лимиты CPU/RAM для сервисов 

## Переменные / build args

Фронтенд использует `VUE_APP_API_URL` как build-time переменную
По умолчанию в `docker-compose.yml`: `http://localhost:8081`.

## Секреты

В compose подключён secret `app_secret` из файла `secrets/app_secret.txt.example`.
