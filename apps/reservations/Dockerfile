FROM node:alpine AS development

WORKDIR /usr/src/app

COPY package.json pnpm-lock.yaml pnpm-workspace.yaml tsconfig.json nest-cli.json ./

RUN apk add --no-cache python3 make g++ \
    && npm install -g pnpm @nestjs/cli

RUN pnpm install --frozen-lockfile --ignore-scripts

COPY apps/reservations apps/reservations
COPY libs libscd

RUN pnpm run build reservations

FROM node:alpine AS production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
RUN npm install -g pnpm

COPY --from=development /usr/src/app/node_modules ./node_modules
COPY --from=development /usr/src/app/dist ./dist

CMD ["node", "dist/apps/reservations/main"] 