<p align="center"><a href="https://froxlor.org" target="_blank"><img src="https://raw.githubusercontent.com/froxlor/framework/refs/heads/main/packages/ui/resources/img/icon.png" width="80"></a></p>

<p align="center">
<a href="https://github.com/froxlor/framework/actions"><img src="https://github.com/froxlor/framework/actions/workflows/run-unit-tests.yml/badge.svg" alt="Build Status"></a>
<a href="https://github.com/froxlor/container"><img src="https://img.shields.io/badge/status-in%20development-orange" alt="Status"></a>
<a href="https://github.com/froxlor/container"><img src="https://img.shields.io/github/license/froxlor/container" alt="License"></a>
</p>

# froxlor Container
This is the froxlor container image for Docker, Kubernetes, and other container platforms.

## Quick Start

A minimal setup to run **froxlor** using Docker Compose.

### Setup Docker Compose File

Create a `docker-compose.yml` file:

```yaml
services:
  froxlor:
    image: hub.froxlor.io/froxlor/froxlor:latest
    restart: unless-stopped
    privileged: true
    pid: "host"
    ports:
      - "8000:8000"
      - "8443:8443"
    env_file:
      - ../froxlor/.env
    volumes:
      - ../froxlor:/var/www/html/froxlor
```

Or use the extended configuration example, which includes environment variables and volumes:

```yaml
services:
  froxlor:
    image: hub.froxlor.io/froxlor/froxlor:latest
    restart: unless-stopped
    privileged: true
    pid: "host"
    environment:
      DB_CONNECTION: sqlite
    ports:
      - "8443:8443"
    volumes:
      - froxlor:/var/www/html/froxlor

volumes:
  froxlor:
```

### Start the Container

```bash
docker compose up -d
```

### Access the Panel

Open froxlor in your browser under the URL`http://localhost:8000` or `http://localhost:8443` when SSL termination is enabled.

### Stop the Container

```bash
docker compose down
```

## Remove Privileged Host Access

Starting the froxlor service without the `privileged` and `pid` options may cause certain features that require elevated permissions to malfunction.

### Steps to Remove

If you want to run the container without privileged access, remove the following lines from your `docker-compose.yml`:

```bash
privileged: true
pid: "host"
```

### Consequences

* Local node management will be `disabled`.
* To continue using froxlor, you will need to install the `froxlor Remote Adapter` package.

## Production Environment Recommendations

For production deployments, consider applying the following configurations:

* **Persistent Volumes**: Ensure data is stored outside the container to prevent data loss.
* **Reverse Proxy**: Use a reverse proxy such as `Caddy`, `Nginx` or `Traefik` to handle routing and security.
* **HTTPS**: Enable HTTPS to secure communications.
* **Image Versioning**: Pin a specific image version instead of relying on the `latest` tag to maintain stability.
