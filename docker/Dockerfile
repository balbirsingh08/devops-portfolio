# Multi-stage Docker build for Node.js application
# Stage 1: Builder - Install dependencies and build application
# Stage 2: Runtime - Minimal image with only necessary artifacts
# Result: ~100MB final image instead of 500MB+

# ============================================================
# STAGE 1: BUILDER
# ============================================================
# Use official Node.js LTS image as base for building
FROM node:20-alpine AS builder

# Set working directory inside container
WORKDIR /app

# Copy package files for dependency installation
# COPY package*.json ensures we copy both package.json and package-lock.json if it exists
COPY package*.json ./

# Install production dependencies
# --only=production installs only dependencies needed for runtime
# npm ci uses package-lock.json for reproducible installs
RUN npm ci --only=production && \
    npm cache clean --force

# Copy application source code
# This is done after npm install to leverage Docker layer caching
# If source changes but dependencies don't, Docker uses cached layer
COPY . .

# Build application (TypeScript compilation, etc.)
RUN npm run build

# ============================================================
# STAGE 2: RUNTIME
# ============================================================
# Use minimal Alpine Linux image for final runtime
FROM alpine:3.19

# Install runtime dependencies only (no build tools)
# dumb-init handles process management and signal forwarding
# curl for health checks
RUN apk add --no-cache \
    nodejs=20.12.0-r0 \
    dumb-init=1.2.5-r2 \
    curl=8.5.0-r0

# Create non-root user for security
# Running as root in containers is a security anti-pattern
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Set working directory
WORKDIR /app

# Copy built application from builder stage
# COPY --from=builder pulls artifacts from previous stage
# COPY --chown ensures proper file permissions
COPY --from=builder --chown=nodejs:nodejs /app/dist ./dist
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
COPY --from=builder --chown=nodejs:nodejs /app/package*.json ./

# Switch to non-root user
USER nodejs

# Expose application port
# Documentation only; doesn't actually publish the port
EXPOSE 3000

# Set NODE_ENV to production for optimizations
ENV NODE_ENV=production

# Health check to verify container is running and responding
# Docker will mark container unhealthy if check fails 3 times
HEALTHCHECK --interval=30s --timeout=10s --start-period=40s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

# Use dumb-init to properly handle process signals
# This ensures graceful shutdown and prevents zombie processes
ENTRYPOINT ["/usr/sbin/dumb-init", "--"]

# Start the application
# CMD defines default command; can be overridden when running container
CMD ["node", "dist/index.js"]