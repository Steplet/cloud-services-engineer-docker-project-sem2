# Stage 1

FROM golang:1.17-alpine AS builder

WORKDIR /src

RUN apk add --no-cache ca-certificates git

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build \
    -trimpath \
    -o /out/api \
    ./cmd/api


# Stage 2

FROM alpine:3.19


RUN apk add --no-cache ca-certificates wget \
    && addgroup -g 10001 -S app \
    && adduser -u 10001 -S -G app -h /app -s /sbin/nologin app \
    && rm -rf /var/cache/apk/*

WORKDIR /app

COPY --from=builder --chown=app:app /out/api ./api

USER app

EXPOSE 8081

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD wget -q --spider http://127.0.0.1:8081/health || exit 1

ENTRYPOINT ["./api"]
