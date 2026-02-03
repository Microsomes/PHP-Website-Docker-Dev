# PHP Docker Dev

Nginx + PHP-FPM setup for local development and deployment testing.

## Quick Start

```bash
# Start locally
docker-compose up

# Visit http://localhost:8023
```

## Structure

```
.docker/dev/
├── frontend/    # Nginx (reverse proxy)
└── backend/     # PHP-FPM
src/             # Your PHP files
```

## Build & Push (for x86 servers)

```bash
# Login to registry
doctl registry login

# Build for x86 and push
docker buildx build --platform linux/amd64 --push \
  -t registry.digitalocean.com/myregistrytayyab/frontend:1.0.0 \
  -f ./.docker/dev/frontend/Dockerfile .

docker buildx build --platform linux/amd64 --push \
  -t registry.digitalocean.com/myregistrytayyab/backend:1.0.0 \
  -f ./.docker/dev/backend/Dockerfile .
```

## How It Works

- **Frontend**: Nginx listens on port 80, proxies `.php` requests to backend
- **Backend**: PHP-FPM listens on port 9000
- **Volumes**: `./src` mounts to `/var/www` for live reload during development
