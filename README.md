# OpenClaw Bridge

Bridge for inter-OpenClaw communication via Docker Socket Proxy

## Overview

This bridge enables communication between two OpenClaw instances running on the same VPS. It uses a Docker Socket Proxy for secure container access and provides REST API endpoints for inter-instance communication.

## Architecture

```
OpenClaw-1 (47597)
      ↓
   Bridge (5000)
      ↓
OpenClaw-2 (47697)

+ Docker Socket Proxy (2375)
```

## Features

- ✅ Docker Socket Proxy integration for secure Docker access
- ✅ REST API for inter-OpenClaw communication
- ✅ Health checks and status monitoring
- ✅ Container management via Docker API
- ✅ Automatic GitHub Actions CI/CD to ghcr.io
- ✅ Docker Compose deployment

## Quick Start

### Prerequisites

- Docker & Docker Compose
- Two OpenClaw instances running on ports 47597 and 47697
- GitHub Container Registry (ghcr.io) access

### Deployment

1. Clone the repository:
```bash
git clone https://github.com/BenbalBerlinAuf/openclaw-bridge.git
cd openclaw-bridge
```

2. Set environment variables:
```bash
export TOKEN=your_github_token
```

3. Deploy with Docker Compose:
```bash
docker-compose up -d
```

4. Verify the bridge is running:
```bash
curl http://localhost:5000/health
```

## API Endpoints

### Health Check
```
GET /health
```
Returns bridge status and configuration.

### Status
```
GET /api/status
```
Get bridge operational status and connected systems.

### Docker Containers
```
GET /api/docker/containers
```
List all Docker containers via Socket Proxy.

### Docker Info
```
GET /api/docker/info
```
Get Docker daemon information.

### Proxy Requests
```
GET|POST|PUT|DELETE /api/proxy/<endpoint>?source_port=47597
```
Proxy requests between OpenClaw instances.

**Example:**
```bash
# Route from OpenClaw-1 to OpenClaw-2
curl -X GET "http://localhost:5000/api/proxy/jobs?source_port=47597"
```

## Configuration

Environment variables in `docker-compose.yml`:

- `DOCKER_HOST`: Docker Socket Proxy address (default: tcp://dockerproxy:2375)
- `TARGET_CONTAINER`: Target container name for routing
- `TOKEN`: GitHub token for authentication
- `BRIDGE_PORT`: Bridge service port (default: 5000)

## Monitoring

Check bridge logs:
```bash
docker-compose logs -f bridge
```

Check health status:
```bash
docker-compose exec bridge curl http://localhost:5000/health
```

## Development

### Local Testing

1. Install dependencies:
```bash
pip install -r requirements.txt
```

2. Run locally:
```bash
python bridge.py
```

3. Test endpoints:
```bash
curl http://localhost:5000/health
```

## GitHub Actions

Automatic build and push to ghcr.io on every push to `main` branch.

Image: `ghcr.io/benbalberlin/openclaw-bridge:latest`

## Troubleshooting

### Bridge container won't start
- Check Docker Socket Proxy is running
- Verify network `openclaw-shared` exists
- Check TOKEN environment variable is set

### Can't reach OpenClaw instances
- Verify OpenClaw containers are running on correct ports
- Check bridge is on same Docker network
- Verify firewall rules

### Image not found in ghcr.io
- Check GitHub Actions workflow ran successfully
- Verify GHCR_TOKEN secret is set correctly
- Check token has `write:packages` permission

## License

MIT

## Support

For issues or questions, open a GitHub issue in this repository.