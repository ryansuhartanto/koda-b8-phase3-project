FROM oven/bun:1
WORKDIR /app

COPY backend/package.json ./
RUN bun install --ignore-scripts

COPY backend/ ./
RUN bun run build

CMD ["bun", "dist/index.mjs"]
