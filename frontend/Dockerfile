# ------------------- Stage 1: Build Stage ------------------------------
FROM node:21 AS builder

# Set working directory
WORKDIR /app

# Copy package files and install deps
COPY package*.json ./
RUN npm install

# Copy source code
COPY . .

# Set production environment variables before build (if needed)
# ARG VITE_API_PATH
# ENV VITE_API_PATH=$VITE_API_PATH

# Build the Vite app
RUN npm run build

# ------------------- Stage 2: NGINX Server Stage ------------------------------
FROM nginx:alpine

# Remove default nginx static files
RUN rm -rf /usr/share/nginx/html/*

# Copy built frontend files to Nginx's public directory
COPY --from=builder /app/dist /usr/share/nginx/html

# Copy custom nginx config (you'll create this file next)
COPY nginx.conf /etc/nginx/nginx.conf

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
