FROM nginx:1.25-alpine

RUN apk add --no-cache wget \
    && rm -rf /var/cache/apk/*

USER nginx

HEALTHCHECK --interval=30s --timeout=5s --start-period=15s --retries=3 \
    CMD wget -q -O- http://127.0.0.1:8081/health >/dev/null 2>&1 || exit 1

ENTRYPOINT ["nginx", "-g", "daemon off;"]
