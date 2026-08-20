# ShortLink

URL shortener with accounts, custom codes, and QR codes. Monorepo shell holding
[`backend`](https://github.com/ryansuhartanto/koda-b8-phase3-backend) and
[`frontend`](https://github.com/ryansuhartanto/koda-b8-phase3-frontend) as git submodules,
plus the Docker Compose stack that runs both against Postgres.

## Stack

| Piece   | Tech                                                       |
| ------- | ---------------------------------------------------------- |
| API     | Express 5, Sequelize 7, Postgres 18, JWT                   |
| Web     | React 19, React Router 8, Redux Toolkit, Tailwind 4        |
| Tooling | Bun, vite-plus (`vp`), oxlint, oxfmt, TypeScript (checkJs) |

## Clone

```sh
git clone --recurse-submodules https://github.com/ryansuhartanto/koda-b8-phase3-project
cd koda-b8-phase3-project
```

Already cloned without submodules:

```sh
git submodule update --init --recursive
```

## Run with Docker

```sh
cp .env.example .env
# fill PGPASSWORD, and paste `openssl rand -hex 32` into JWT_SECRET and LINK_KEY
docker compose up --build
```

Web on `http://localhost:$WEB_PORT`, API on `http://localhost:$API_PORT`. The `migrate`
service runs migrations once and exits before `api` starts.

### Environment

| Variable     | Purpose                                                |
| ------------ | ------------------------------------------------------ |
| `API_PORT`   | Host port for the API (container listens on 3001)      |
| `WEB_PORT`   | Host port for the web app (container listens on 3002)  |
| `PG*`        | Postgres credentials, shared by `db`, `migrate`, `api` |
| `JWT_SECRET` | Signing key for auth tokens                            |
| `LINK_KEY`   | SipHash key for short-code generation                  |

`VITE_API_URL` and `VITE_BASE_URL` are build args baked into the web image at build time,
derived from `API_PORT` and `WEB_PORT`. Changing ports means rebuilding `web`.

## Run without Docker

Each submodule is standalone. See [`backend/README.md`](backend/README.md) and
[`frontend/README.md`](frontend/README.md); both need their own `.env`.

## Repo scripts

```sh
bun run lint   # oxlint across the workspace shell
bun run fmt    # oxfmt
```

Open `project.code-workspace` in VS Code to get all three folders as one workspace.
Commits are linted by commitlint (conventional commits).

## License

MIT
