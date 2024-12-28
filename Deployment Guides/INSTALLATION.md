# Comprehensive Najahi Installation Guide

## Initial Setup

### 1. System Requirements Verification

1. Check Node.js version:
   ```bash
   node --version  # Must be v20 or later
   ```
2. Verify package manager installation:
   ```bash
   npm --version  # or
   pnpm --version
   ```

### 2. Email Server Configuration

1. **Gmail Setup** (Recommended for starting):
   1. Enable 2-Factor Authentication
   2. Generate App Password
   3. Use following settings:
      ```
      EMAIL_USER=
      EMAIL_PASS=
      EMAIL_TO=
      ```

## Deployment Methods

### Method 1: Vercel Deployment

#### Pre-deployment Setup

1. Install Vercel CLI:

   ```bash
   npm i -g vercel
   ```

2. Create `.env` file with all required variables
3. Verify project structure follows Vercel requirements

#### Deployment Steps

1. Login to Vercel:

   ```bash
   vercel login
   ```

2. Initial deployment:

   ```bash
   vercel
   ```

3. Configure environment variables:

   1. Open Vercel dashboard
   2. Go to Project Settings
   3. Navigate to Environment Variables
   4. Import `.env` file or add manually
   5. Verify all required variables are set

4. Production deployment:
   ```bash
   vercel --prod
   ```

### Method 2: Docker Deployment

#### Pre-deployment Setup

1. Install Docker:

   ```bash
   # Ubuntu
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh

   # Verify installation
   docker --version
   ```

#### Configuration Files

1. Create `docker-compose.yml`:

```yaml
services:
  app: # Main application service
    image: ghcr.io/kraito/najahi:latest # Change this to your image repository
    build:
      context: . # Build context is current directory
      dockerfile: Dockerfile # Points to Dockerfile in root
    ports:
      - "3000:3000" # Map host port 3000 to container port 3000 (adjust if needed)
    environment:
      # Email Configuration
      EMAIL_USER: ""
      EMAIL_PASS: ""
      EMAIL_TO: ""
      NEXT_PUBLIC_CONTACT_API_ROUTE: "/api/contact"

      # Cloudflare Turnstile
      NEXT_PUBLIC_CLOUDFLARE_TURNSTILE_SITE_KEY: ""
      CLOUDFLARE_TURNSTILE_SECRET_KEY: ""

      # Analytics
      NEXT_PUBLIC_GA_ID: "your-google-analytics-id"
      NEXT_PUBLIC_BING_ID: "your-bing-id"
      NEXT_PUBLIC_YANDEX_ID: "your-yandex-id"

      # Application
      NODE_ENV: "production"
      PORT: "3000"

      # Security
      CORS_ORIGINS: "https://yourdomain.com,https://api.yourdomain.com"
    restart: unless-stopped # Restart policy (options: no, always, on-failure, unless-stopped)
    networks:
      - najahi-network # Connect to custom network (defined below)
    volumes:
      - najahi-uploads:/app/public/uploads
      - najahi-data:/app/data
      - najahi-cache:/app/.next/cache

networks:
  najahi-network: # Custom network definition
    driver: bridge # Network driver type (bridge is default)

volumes:
  najahi-uploads:
  najahi-data:
  najahi-cache:
```

2. Create `Dockerfile`:

```dockerfile
# Stage 1: Dependencies
FROM node:20-alpine AS deps
LABEL org.opencontainers.image.source="https://github.com/kraito/najahi"
LABEL org.opencontainers.image.description="Najahi - Personal Portfolio Resume Template Developed by Kraito"
LABEL org.opencontainers.image.licenses="Commercial License"

RUN apk add --no-cache libc6-compat
WORKDIR /app

# Copy package files
COPY package.json package-lock.json* ./
COPY .env .env.local* ./

# Install dependencies
RUN npm install

# Stage 2: Builder
FROM node:20-alpine AS builder
WORKDIR /app

ENV NEXT_TELEMETRY_DISABLED=1
ENV NODE_ENV=production
ENV SKIP_ENV_VALIDATION=1
ENV SKIP_SMTP_VERIFY=true

COPY --from=deps /app/node_modules ./node_modules
COPY --from=deps /app/.env* ./
COPY . .

# Build the application with increased memory limit
RUN node --max_old_space_size=4096 node_modules/.bin/next build

# Stage 3: Runner
FROM node:20-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

# Copy necessary files
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static
COPY --from=builder /app/.env* ./

# Create cache directory and set permissions
RUN mkdir -p .next/cache
RUN chown -R nextjs:nodejs .next
RUN chmod -R 755 .next

USER nextjs

EXPOSE 3000

ENV PORT=3000
ENV HOSTNAME="0.0.0.0"

CMD ["node", "server.js"]
```

#### Deployment Steps

1. Start the containers:

   ```bash
   docker-compose up -d
   ```

2. Verify deployment:
   ```bash
   docker-compose ps
   docker-compose logs
   ```

### Method 3: PM2 Deployment

#### Installation & Setup

1. Install PM2:

   ```bash
   npm install -g pm2
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Build application:
   ```bash
   npm run build
   ```

#### Configuration

Create `ecosystem.config.js`:

```javascript
module.exports = {
  apps: [
    {
      name: "najahi",
      script: "./node_modules/next/dist/bin/next",
      args: "start",
      //instances: 'max',
      exec_mode: "cluster",
      autorestart: true,
      watch: false,
      max_memory_restart: "1G",
      env_production: {
        NODE_ENV: "production",
        PORT: 3000,
      },
      env_development: {
        NODE_ENV: "development",
        PORT: 3000,
      },
    },
  ],
};
```

#### Deployment Steps

1. Start application:

   ```bash
   pm2 start ecosystem.config.js --env production
   ```

2. Setup auto-restart:

   ```bash
   pm2 startup
   pm2 save
   ```

3. Monitor application:
   ```bash
   pm2 monit
   pm2 logs najahi
   ```

## Troubleshooting Guide

### Build Failures

1. Clear build cache:

   ```bash
   rm -rf .next
   npm run build
   ```

2. Check dependency conflicts:
   ```bash
   npm ls
   ```

### Runtime Errors

1. Check logs based on deployment method:

   ```bash
   # Vercel
   vercel logs

   # Docker
   docker logs najahi

   # PM2
   pm2 logs najahi
   ```

2. Common fixes:
   - Restart application
   - Verify environment variables
   - Check system resources
   - Monitor memory usage

### Performance Issues

1. Monitor metrics:

   ```bash
   # PM2
   pm2 monit

   # Docker
   docker stats najahi
   ```

2. Optimization steps:
   - Scale instances
   - Adjust memory limits
   - Enable caching

## Security Checklist

- [ ] Environment variables properly set
- [ ] CORS origins configured
- [ ] SMTP credentials protected
- [ ] SSL/TLS enabled
- [ ] Regular security updates
- [ ] Proper error handling
