# Step 1: Use Node.js base image for building
FROM node:20-alpine AS builder

WORKDIR /app

# Copy root files needed for dependency installation and build
COPY package.json pnpm-lock.yaml tsconfig*.json ./

# Install dependencies
RUN npm install -g pnpm && pnpm install

# Copy the entire codebase and build the application
COPY . .
RUN pnpm run build:users

# Step 2: Prepare the final lightweight image for production
FROM node:20-alpine

WORKDIR /app

# Copy only the `dist` folder and `node_modules` from the builder stage
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist /app/dist

# Expose the port for the `users` service
EXPOSE 3001

# Start the users service
CMD ["node", "dist/apps/users/main"]
