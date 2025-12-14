# 🌐 Infra Gateway (Master Nginx)

This repository contains the **Central Gateway (Reverse Proxy)** configuration responsible for terminating HTTPS traffic and routing requests to Bruna Development's internal services.

## 🏗 Architecture

Nginx acts as the single entry point (Ports 80/443), managing:

1.  **SSL/TLS Termination** (Let's Encrypt)
2.  **Domain Routing**:
    *   `brunadev.com` ➝ Container `brunadev_web` (Static Portfolio)
    *   `petcare.brunadev.com` ➝ Container `petcare_nginx` (Django SaaS)
3.  **Security**: Proxy headers (`X-Forwarded-Proto`) ensuring internal apps recognize secure connections.

## 🚀 How to Run

### Prerequisites
*   Docker & Docker Compose
*   Shared Docker Network: `docker network create gateway_net`
*   SSL Certificates generated (see below)

### Deploy
```bash
docker compose up --build -d
```

## 🔒 SSL Certificates (First Run)

The container expects certificates to already exist. For the first run on a new server:

1.  Comment out the `server 443` blocks in `nginx.conf`.
2.  Start the container exposing only port 80.
3.  Run Certbot to generate the certificates:
    ```bash
    certbot certonly --webroot -w /var/www/certbot -d brunadev.com -d www.brunadev.com
    certbot certonly --webroot -w /var/www/certbot -d petcare.brunadev.com
    ```
4.  Uncomment the SSL blocks and restart the container.
