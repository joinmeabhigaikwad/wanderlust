# Stage 1 - Build
FROM node:20-alpine AS builder

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Create env file
RUN cp .env.sample .env

# Build TypeScript
RUN npm run build

# Stage 2 - Production
FROM node:20-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install only production dependencies
RUN npm install --omit=dev

# Copy compiled app
COPY --from=builder /app/dist ./dist

# Copy env file
COPY --from=builder /app/.env ./.env

# Expose backend port
EXPOSE 8080

# Start application
CMD ["node", "dist/server.js"]
