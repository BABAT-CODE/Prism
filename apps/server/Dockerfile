FROM node:20-slim AS dependencies

WORKDIR /workspace
RUN corepack enable && corepack prepare pnpm@9.15.9 --activate
COPY . .
RUN pnpm install --frozen-lockfile

FROM dependencies AS builder
RUN pnpm build:server

FROM node:20-slim AS production-dependencies
WORKDIR /workspace
RUN corepack enable && corepack prepare pnpm@9.15.9 --activate
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY apps/server/package.json apps/server/package.json
RUN pnpm install --filter grat-server --prod --frozen-lockfile

FROM node:20-slim AS runner
ENV NODE_ENV=production \
    PORT=3001
WORKDIR /workspace
COPY --from=production-dependencies --chown=node:node /workspace/node_modules ./node_modules
COPY --from=production-dependencies --chown=node:node /workspace/apps/server/node_modules ./apps/server/node_modules
COPY --from=builder --chown=node:node /workspace/apps/server/dist ./apps/server/dist
USER node
EXPOSE 3001
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD node -e "fetch('http://127.0.0.1:3001/health').then((response) => process.exit(response.ok ? 0 : 1)).catch(() => process.exit(1))"
CMD ["node", "apps/server/dist/index.js"]