# Stage 1: Build
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: Production
FROM node:20-alpine AS runtime
WORKDIR /app
ENV NODE_ENV=production

# Copiar solo los archivos necesarios de la etapa anterior
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/dist ./dist

RUN npm ci --only=production && npm cache clean --force

EXPOSE 8080
CMD ["node", "dist/main"]