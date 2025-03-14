# Lightweight AI Automation Stack for Cloud Hosting

An all-in-one Docker Compose setup that provides a complete stack for AI workflow automation, integration, and monitoring.

> 🔗 **Detailed Setup Guide**: [Complete Installation Guide on Notion](https://liberating-galley-48d.notion.site/Installing-N8N-Flowise-w-Qdrant-monitoring-with-Prometheus-Grafana-on-Hetzner-Cloud-1b3cf2b3a53980d39ae8f38a121a33fd?pvs=4)
> 
> 📺 **Video Tutorial**: [How to Set Up on Hetzner Cloud (YouTube)](link-to-be-determined)
> 
> 🧪 **Need Help?**: Visit [Alchemyst Digital](https://alchemyst.digital) to get expert assistance building AI and automation solutions for your business.

## What's Included

This stack combines the following tools:

- **n8n**: Powerful workflow automation tool to connect applications and automate tasks
- **Flowise**: Low-code UI builder for creating LLM flows
- **Qdrant**: Vector database for AI applications
- **Monitoring stack**:
  - Prometheus: Metrics collection system
  - Grafana: Analytics and monitoring dashboard
  - Postgres Exporter: Postgres metrics collector
- **Infrastructure**:
  - PostgreSQL: Database storage for n8n and other services
  - Caddy: Reverse proxy with automatic HTTPS

## Prerequisites

- Docker and Docker Compose V2
- A server with ports 80 and 443 accessible
- A domain name pointed to your server

## Getting Started

For a complete step-by-step guide with screenshots, follow our [Notion Setup Guide](https://liberating-galley-48d.notion.site/Installing-N8N-Flowise-w-Qdrant-monitoring-with-Prometheus-Grafana-on-Hetzner-Cloud-1b3cf2b3a53980d39ae8f38a121a33fd?pvs=4).

You can also watch our [YouTube tutorial](link-to-be-determined) that demonstrates the entire setup process on Hetzner Cloud.

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/ai-automation-stack.git
cd ai-automation-stack
```

### 2. Configure environment variables

Edit the `.env` file to customize your setup:

```bash
# Set your domain name
DOMAIN_NAME=yourdomain.com
SUBDOMAIN=n8n

# Set secure passwords for services
POSTGRES_PASSWORD=your-secure-password
PGRST_JWT_SECRET=your-secret-key
GRAFANA_ADMIN_PASS=your-secure-password
FLOWISE_PASSWORD=your-secure-password
```

### 3. Configure Caddyfile

The default Caddyfile is set up for subdomains. Edit `caddy/Caddyfile` to match your domain:

Replace all instances of `example.com` with your domain name from the `.env` file.

### 4. Start the stack

```bash
docker compose up -d
```

### 5. Access your services

After deployment, you can access your services at:

- n8n: https://n8n.yourdomain.com
- Flowise: https://flowise.yourdomain.com
- Grafana: https://grafana.yourdomain.com
- Prometheus: https://prometheus.yourdomain.com
- Qdrant: https://qdrant.yourdomain.com/dashboard

## Architecture

This stack is designed to provide a complete environment for building AI-powered automation workflows:

- **n8n** serves as the central automation platform
- **Flowise** allows you to build LLM-powered flows
- **Qdrant** provides vector database capabilities for AI applications
- **PostgreSQL** stores workflow data and application state
- **Prometheus & Grafana** monitor the health and performance of all components
- **Caddy** handles routing and TLS certificate management

## Configuration Details

### n8n Configuration

n8n is configured with PostgreSQL as the database backend and has metrics and runners enabled. The container exposes port 5678 internally.

### Flowise Configuration

Flowise is configured with metrics enabled and uses the internal directory for storage. It exposes port 3000 internally.

### Monitoring

Prometheus is configured to scrape metrics from:
- n8n
- PostgreSQL (via postgres-exporter)
- Qdrant
- Flowise
- Host machine (requires node-exporter)

Grafana connects to Prometheus as a data source and provides dashboards for monitoring.

### Security

- All services are only accessible through the Caddy reverse proxy
- Caddy automatically obtains and renews HTTPS certificates
- Each service has its own credentials defined in the `.env` file

## Persistence

All data is stored in Docker volumes:

- `n8n_data`: n8n workflows and credentials
- `postgres_data`: Database storage
- `flowise_data`: Flowise configurations
- `qdrant_data`: Vector database storage
- `prometheus_data`: Metrics history
- `grafana_data`: Dashboards and configurations
- `caddy_data`: TLS certificates

## Maintenance

### Updating

To update the stack to the latest versions:

```bash
docker compose pull
docker compose up -d
```

### Backup

Backup the Docker volumes for complete data protection:

```bash
# Example backup command
docker run --rm -v ai-automation-stack_n8n_data:/source -v $(pwd)/backups:/dest alpine tar czf /dest/n8n_backup.tar.gz -C /source .
```

### Logs

View logs for any service:

```bash
docker compose logs -f n8n
```

## Troubleshooting

### Service not starting

Check service logs:
```bash
docker compose logs service_name
```

### Cannot access services

- Verify Caddy is running: `docker compose ps caddy`
- Check Caddy logs: `docker compose logs caddy`
- Ensure your domain DNS is correctly configured

## License

[MIT License](LICENSE)

---

## About

This stack is maintained by [Alchemyst Digital](https://alchemyst.digital), experts in AI implementation and business automation solutions. Visit our website to learn how we can help you leverage AI for your business needs.
