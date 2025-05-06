# 🚀 n8n Multi-Tenant Deployment

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Docker Ready](https://img.shields.io/badge/Docker-Ready-blue)](https://hub.docker.com/)
[![n8n Version](https://img.shields.io/badge/n8n-latest-green)](https://n8n.io)

This project provides a solution to deploy multiple instances of the n8n workflow automation platform using a single PostgreSQL container, with each n8n instance having its own separate database. The deployment is managed via Docker Compose and Traefik as a reverse proxy.

A scalable multi-tenant deployment solution for n8n workflow automation, featuring:
- Single PostgreSQL container with isolated databases
- Traefik reverse proxy with automatic HTTPS
- Easy tenant management via scripts


## 🌟 Features

- 🏢 **Multi-tenant architecture** - Separate n8n instances with isolated databases
- 🔐 **Auto HTTPS** - Via Let's Encrypt or self-signed certificates
- 🛠️ **Easy management** - Add/remove tenants with simple scripts
- 📊 **Traefik dashboard** - Monitor and manage routing
- 🐳 **Docker-based** - Easy deployment and scaling


## 🚀 Quick Start

### Prerequisites
- Docker & Docker Compose
- Bash shell
- OpenSSL (for certificate generation)
- **yq** and **jq** (for tenant deletion script)

```bash
# Clone the repository
git clone https://github.com/akkezi/n8n-multi-tenant.git
cd n8n-multi-tenant

# Edit the .env file to configure your deployment

# Generate certificates
./generate-certs.sh

# Start the services
docker compose up -d
```

## 📂 Project Structure
```
n8n-multi-tenant/
├── .env                          # Environment variables configuration
├── generate-certs.sh             # SSL certificate generation script
├── docker-compose.yml            # Main Docker Compose file
├── init-db-multi-tenant.sh       # Database initialization script
├── traefik-dynamic.yml           # Traefik dynamic configuration
└── templates/                    # Tenant management templates
    ├── add-tenant.sh             # Script to add new tenants
    ├── delete-all-tenants.sh     # Script to remove all tenants
    ├── tenant-template.yml       # Template for new tenant services
    └── traefik-middleware.yml    # Template for Traefik middlewares
```


## 🔧 Configuration

### Environment Variables
Edit the **.env** file to configure your deployment
```
# Postgres Configuration
POSTGRES_USER=your_user
POSTGRES_PASSWORD=your_secure_password

# Traefik Configuration
DOMAIN_NAME=yourdomain.com
LETSENCRYPT_EMAIL=your@email.com

# Tenant Configuration
TENANT1_SUBFOLDER=tenant1
TENANT1_DB=n8n_tenant1
...
```

### Important Notes
1. Replace all default credentials in the **.env** file
2. Set your actual domain name in **DOMAIN_NAME**
3. For production use, consider using Let's Encrypt certificates instead of self-signed ones


## 🏢 Usage

### 🔗 Accessing Services
>Traefik Dashboard: https://yourdomain.com/dashboard/
>    - Username: admin
>    - Password: admin (unless changed in .env)

>n8n Instances:
>    - Tenant 1: https://yourdomain.com/tenant1
>    - Tenant 2: https://yourdomain.com/tenant2


## 🏗️ Managing Tenants

#### Adding a New Tenant
1. Run the add tenant script
```
./templates/add-tenant.sh
```
2. Follow the prompts to provide:
    - Tenant ID (numeric, e.g., 3)
    - Tenant subfolder (e.g., tenant3)
    - Database name (e.g., n8n_tenant3)
    - Database user and password
3. The script will
    - Create a new database and user
    - Update configuration files
    - Deploy the new tenant

#### Deleting All Tenants
1. Run the delete script
```
./templates/delete-all-tenants.sh
```
2. Confirm the deletion when prompted
3. The script will
    - Stop and remove all services
    - Clean up configuration files
    - Restore the original **.env** and **traefik-dynamic.yml** from backups

### Certificate Management
To generate new self-signed certificates
```
./generate-certs.sh
```

>For production use, it's recommended to use Let's Encrypt certificates by
>1. Setting a valid domain name in **.env**
>2. Ensuring your server is accessible from the internet on ports 80 and 443
>3. Providing a valid email in **LETSENCRYPT_EMAIL**


## 🛠 Troubleshooting

### Certificate errors
- For development, you'll need to trust the self-signed certificate
- For production, use Let's Encrypt by setting a valid domain
### Database connection issues
- Verify the PostgreSQL container is running: **docker compose ps**
- Check logs: **docker compose logs postgres**
### Traefik routing problems
- Check Traefik logs: **docker compose logs traefik**
- Verify the Traefik dashboard for routing configuration
### Tenant creation fails
- Check the log file: **./templates/tenant_deployment.log**
- Verify available disk space and Docker resources


## 📜 License
MIT License - See LICENSE for details.


## 🤝 Contributing
Pull requests are welcome! Please open an issue first to discuss changes.
