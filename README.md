# Momo Store — Docker

## Запуск (prod, по умолчанию)

Обычная команда поднимает **prod-стек**: `backend` + `gateway` + `frontend`.

```bash
sudo docker compose up -d --build
```

Открыть:
- **Frontend**: `http://localhost/momo-store/` (порт 80)
- **Backend (API через gateway)**: `http://localhost:8081` (порт 8081)

## Dev (отдельный файл, без gateway)

Перед dev останови prod, чтобы не было конфликта портов:

```bash
sudo docker compose down
sudo docker compose -f docker-compose.dev.yml up -d --build
```

В dev поднимаются только `backend` (порт 8081) и `frontend` (порт 80).

Остановка dev:

```bash
sudo docker compose -f docker-compose.dev.yml down
```

## Переопределение через переменные

```bash
FRONTEND_PUBLISH_PORT=8088 \
API_PUBLISH_PORT=18081 \
VUE_APP_API_URL=http://localhost:18081 \
sudo docker compose up -d --build
```

Или через `.env`:

```bash
cp .env.example .env
sudo docker compose up -d --build
```

## Проверки

```bash
curl -i http://localhost/health
curl -i http://localhost:8081/health
curl -i http://localhost:8081/products
```

## Порты и доступность (prod)

- **frontend**: внутри контейнера `8080`, на хосте `80:8080`
- **gateway**: на хосте `8081:8081`
- **backend**: наружу не публикуется (только `expose: 8081` внутри `backend-net`)

## Лимиты ресурсов

В compose-файлах заданы лимиты CPU/RAM для сервисов.

## Переменные / build args

Фронтенд использует `VUE_APP_API_URL` как build-time переменную.
По умолчанию: `${VUE_APP_API_URL:-http://localhost:8081}`.

## Секреты

В compose подключён secret `app_secret` из файла `secrets/app_secret.txt.example`.
Реальные секреты в git не коммитятся.
