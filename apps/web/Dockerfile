FROM node:20-slim AS dependencies

WORKDIR /workspace
RUN corepack enable && corepack prepare pnpm@9.15.9 --activate
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
COPY apps/web/package.json apps/web/package.json
RUN pnpm install --filter grat-web --frozen-lockfile

FROM dependencies AS builder
COPY apps/web apps/web
ARG NEXT_PUBLIC_API_URL=http://localhost:3001
ENV NEXT_PUBLIC_API_URL=${NEXT_PUBLIC_API_URL}
RUN pnpm --filter grat-web build

FROM node:20-slim AS runner
ENV NODE_ENV=production PORT=3000 HOSTNAME=0.0.0.0
WORKDIR /workspace
COPY --from=builder --chown=node:node /workspace/apps/web/.next/standalone ./
COPY --from=builder --chown=node:node /workspace/apps/web/.next/static ./apps/web/.next/static
USER node
EXPOSE 3000
CMD ["node", "apps/web/server.js"]