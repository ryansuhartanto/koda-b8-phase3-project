FROM oven/bun:1 AS build
WORKDIR /app

COPY frontend/package.json ./
RUN bun install --ignore-scripts

COPY frontend/ ./

ARG VITE_API_URL
ARG VITE_BASE_URL
RUN bun run build

FROM caddy:alpine
COPY --from=build /app/dist /srv
COPY <<EOF /etc/caddy/Caddyfile
:3002

root * /srv
encode
try_files {path} /index.html
file_server
EOF
