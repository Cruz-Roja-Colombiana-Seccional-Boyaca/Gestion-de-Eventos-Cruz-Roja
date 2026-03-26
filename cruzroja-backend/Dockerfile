# Backend Dockerfile (NestJS)
FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json ./
COPY yarn.lock .
# npm users: replace with npm ci
RUN corepack yarn install --frozen-lockfile

FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN corepack yarn build

FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 8080
CMD ["node", "dist/main"]
