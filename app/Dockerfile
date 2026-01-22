FROM node:25-alpine3.23 AS base

WORKDIR /app

COPY package.json pnpm-lock.yaml ./

RUN PNPM_VERSION=$(node -e "console.log(require('./package.json').packageManager.split('@')[1])") && \
    npm install -g pnpm@${PNPM_VERSION}

FROM base AS deps

RUN --mount=type=cache,id=pnpm,target=/pnpm/store \
    pnpm install --frozen-lockfile --prod=false

FROM base AS builder

COPY --from=deps /app/node_modules ./node_modules
COPY . .

RUN pnpm build

FROM node:25-alpine3.23 AS runner

WORKDIR /app

COPY --chown=1001:1001 package.json ./

RUN PNPM_VERSION=$(node -e "console.log(require('./package.json').packageManager.split('@')[1])") && \
    npm install -g pnpm@${PNPM_VERSION}

RUN addgroup --system --gid 1001 nodejs && \
    adduser --system --uid 1001 expressjs

ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"

COPY --from=builder --chown=expressjs:nodejs /app/node_modules ./node_modules
COPY --from=builder --chown=expressjs:nodejs /app/dist ./dist

USER expressjs

WORKDIR /app

CMD ["pnpm", "start"]
