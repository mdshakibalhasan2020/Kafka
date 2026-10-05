# ---------- build stage ----------
FROM node:22-alpine AS builder
WORKDIR /app
RUN apk add --no-cache openssl

COPY package*.json ./
RUN npm ci

COPY . .
RUN npx prisma generate
RUN npm run build

# ---------- production stage ----------
FROM node:22-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
RUN apk add --no-cache openssl

# prisma CLI (migrate deploy) + generated client lagbe, tai builder-er node_modules-i nichchi
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/prisma ./prisma

EXPOSE 50051

# migration chalie tarpor app start
CMD ["sh", "-c", "npx prisma migrate deploy && node dist/main.js"]