# n8n with Docker Compose and Caddy

This repository provides a configuration to run [n8n](https://n8n.io/), a workflow automation tool, using Docker Compose. It includes Caddy as a reverse proxy for automatic HTTPS.

## Prerequisites

*   [Docker](https://docs.docker.com/get-docker/)
*   [Docker Compose](https://docs.docker.com/compose/install/) (usually included with Docker Desktop)
*   A domain name pointed to the server where you will run this setup.

## Setup

1.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd <repository-directory>
    ```

2.  **Configure Environment Variables:**
    *   Copy the example `.env` file or ensure your `.env` file exists.
    *   Review and update the variables in the `.env` file:
        *   `DOMAIN_NAME`: Your top-level domain (e.g., `example.com`).
        *   `SUBDOMAIN`: The subdomain for n8n (e.g., `n8n`). The final URL will be `n8n.example.com`.
        *   `GENERIC_TIMEZONE`: Your preferred timezone (e.g., `Europe/Amsterdam`).
        *   `SSL_EMAIL`: The email address for Let's Encrypt SSL certificate registration.
        *   `DB_POSTGRESDB_HOST`, `DB_POSTGRESDB_PORT`, `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB`: Credentials for your PostgreSQL database. This setup uses [Neon.tech](https://neon.tech/) serverless PostgreSQL. You will need to create a database instance on Neon and obtain the connection details. **Ensure you change the default password!**

3.  **Configure Caddy:**
    *   Edit `caddy_config/Caddyfile`.
    *   Replace `n8n.example.com` with your actual n8n domain (`${SUBDOMAIN}.${DOMAIN_NAME}`).

4.  **Start the services:**
    ```bash
    docker-compose up -d
    ```
    This command will build (if necessary) and start the n8n and Caddy containers in detached mode. Caddy will automatically obtain an SSL certificate for your domain.

## Accessing n8n

Once the containers are running, you can access your n8n instance at `https://${SUBDOMAIN}.${DOMAIN_NAME}`.

## Persistent Data

*   **Caddy:** Configuration and SSL certificates are stored in the `caddy_data` named volume and the `caddy_config` directory bind-mount.
*   **n8n:** n8n data (workflows, credentials, etc.) is stored in the configured PostgreSQL database. Ensure your database has appropriate backup procedures.

## Stopping the services

To stop the containers:
```bash
docker-compose down
```

To stop and remove the volumes (use with caution, as this deletes Caddy's data):
```bash
docker-compose down -v
```
