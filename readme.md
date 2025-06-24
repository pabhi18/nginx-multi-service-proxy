# Nginx Multi-Service Proxy

A multi-service architecture using Docker Compose, Nginx as a reverse proxy, and two backend services (one in Go, one in Python).
---

## Setup Instructions

1. **Build and start all services with Docker Compose:**
   ```bash
   docker-compose up --build
   ```
   This will build and start:
   - `service_1` (Go, port 8001)
   - `service_2` (Python, port 8002)
   - `nginx` (reverse proxy, exposed on port 8080)

3. **Access the services via Nginx:**
   - [http://localhost:8080/service1/ping](http://localhost:8080/service1/ping)
   - [http://localhost:8080/service2/ping](http://localhost:8080/service2/ping)

---

## How Routing Works (Nginx Configuration)

- Nginx listens on port **80** (mapped to host **8080**).
- Requests to `/service1/` are proxied to `service_1` (Go) on port 8001.
- Requests to `/service2/` are proxied to `service_2` (Python) on port 8002.
- The routing is defined in [`nginx/nginx.conf`](nginx/nginx.conf):

  ```nginx
  location /service1/ {
      proxy_pass http://service1/;
  }
  location /service2/ {
      proxy_pass http://service2/;
  }
  ```

---

## Bonus Features

- **Health Checks:**
  - Both backend services expose a `/ping` endpoint for health checks.
  - Docker Compose waits for both services to be healthy before starting Nginx.

- **Custom Logging:**
  - A custom log format to log the time and request URI:
    ```nginx
    log_format custom '$time_local | $request_uri';
    access_log /var/log/nginx/access.log custom;
    ```