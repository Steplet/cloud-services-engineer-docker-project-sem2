# Stage 1 

FROM node:16-alpine AS builder

WORKDIR /app

COPY package.json package-lock.json ./
RUN npm ci --ignore-scripts

COPY . .

ARG VUE_APP_API_URL=http://localhost:8081
ENV NODE_ENV=production \
    VUE_APP_API_URL=${VUE_APP_API_URL}

RUN npm run build

# Stage 2

FROM nginx:1.25-alpine

RUN apk add --no-cache wget \
    && rm -rf /var/cache/apk/* \
    && mkdir -p /usr/share/nginx/html/momo-store /var/cache/nginx /var/log/nginx /tmp \
    && chown -R nginx:nginx /usr/share/nginx/html /var/cache/nginx /var/log/nginx /tmp

COPY nginx.conf /etc/nginx/nginx.conf

COPY --from=builder --chown=nginx:nginx /app/dist /usr/share/nginx/html/momo-store

USER nginx

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD wget -q --spider http://127.0.0.1:8080/health || exit 1

ENTRYPOINT ["nginx", "-g", "daemon off;"]
