# Stage 1 - Build app
FROM node:20-alpine AS builder

WORKDIR /app

# Copy package files
COPY package*.json ./

# Disable prepare script during docker build
RUN npm pkg delete scripts.prepare

# Install dependencies
RUN npm install

# Copy remaining files
COPY . .

# Create env file
RUN cp .env.sample .env

# Build project
RUN npm run build

# Stage 2 - Serve with nginx
FROM nginx:stable-alpine

RUN rm -rf /usr/share/nginx/html/*

COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
