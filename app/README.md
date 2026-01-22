# App (Express + TypeScript)

Minimal Express server written in TypeScript.

## Requirements

- Node.js (recommended: 18+)
- pnpm (this repo uses `pnpm@10.x`)

## Install

```bash
pnpm install
```

## Run (dev / hot reload)

```bash
pnpm dev
```

- Runs the server with file watching (`tsx watch`).
- Default port: `3000`.

## Build + Run (prod)

```bash
pnpm build
pnpm start
```

- `build` compiles TypeScript to `dist/`.
- `start` runs `node dist/index.js`.

## Docker

From the repo root:

```bash
docker compose up -d --build
```

Or build the image directly:

```bash
docker build ./app -t dynamic-preview-site-app
```