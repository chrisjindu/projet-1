# Stage 1: Build (Builder stage)
# Named stage ("builder")
FROM node:20-alpine AS builder 

# Sets /app as the working directory (all subsequent commands run here).
# Avoids file conflicts with system directories.
WORKDIR /app

# Copy package files and Install dependencies (clean install)
COPY package*.json ./
RUN npm ci

# Build the app (creates `/app/dist`)
COPY . .

# Build the application
RUN npm run build

# Stage 2: Serve (Production stage)

# Fresh minimal image
FROM nginx:alpine

# Copy the build output from build stage (# Copy ONLY built files)
COPY --from=builder /app/dist /usr/share/nginx/html

# Expose port 80 (documents that the container listens on port 80 "HTTP")
EXPOSE 80

# Start nginx i the foreground (Docker containers need a foreground process to stay alive)
CMD ["nginx", "-g", "daemon off;"]
