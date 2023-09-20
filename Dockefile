FROM node:20-slim AS base
ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
RUN corepack enable
WORKDIR /app
COPY pnpm-lock.yaml .
COPY package.json .

FROM base AS prod-deps
RUN --mount=type=cache,id=pnpm,target=/pnpm/store pnpm install --prod --frozen-lockfile


FROM base AS build
RUN --mount=type=cache,id=pnpm,target=/pnpm/store pnpm install --frozen-lockfile


FROM build AS dev-deps
COPY . .
RUN pnpm run build


FROM base
COPY --from=dev-deps /app/.next/standalone ./

EXPOSE 3000

CMD [ "node", "server.js" ]


