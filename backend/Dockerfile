# -------------------- Stage 1: Build & Test --------------------
FROM node:21 AS backend-builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run test

# -------------------- Stage 2: Runtime --------------------
FROM node:21-slim

WORKDIR /app

# Only bring needed files
COPY --from=backend-builder /app .

# Install only production deps
RUN npm ci --omit=dev

# Use env vars from Kubernetes, not hardcoded file
# (Optionally support local testing with .env)
# ENV NODE_ENV=production

EXPOSE 8080

CMD ["npm", "start"]
