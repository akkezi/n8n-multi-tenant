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

## 🛠️ Configuration
### Environment Variables
Edit the **.env** file to configure your deployment
```
# Postgres
POSTGRES_USER=changeUser
POSTGRES_PASSWORD=changePassword

# Traefik
DOMAIN_NAME=docker-standalone.lab.local
LETSENCRYPT_EMAIL=contact@lab.local
TRAEFIK_AUTH_CREDENTIALS="admin:$$apr1$$r3vKEUbE$$3LEU7AIoxxRXUV623KnlO1" # Password: admin

# General
GENERIC_TIMEZONE=Europe/Paris

# Tenant 1
TENANT1_SUBFOLDER=tenant1
TENANT1_DB=n8n_tenant1
TENANT1_USER=n8n_tenant1_user
TENANT1_PASSWORD=n8n_tenant1_pass

# Tenant 2
TENANT2_SUBFOLDER=tenant2
TENANT2_DB=n8n_tenant2
TENANT2_USER=n8n_tenant2_user
TENANT2_PASSWORD=n8n_tenant2_pass
```

### Important Notes
1. Replace all default credentials in the **.env** file
2. Set your actual domain name in **DOMAIN_NAME**
3. For production use, consider using Let's Encrypt certificates instead of self-signed ones

## 🏢 Usage
### Accessing Services

>Traefik Dashboard: https://yourdomain.com/dashboard/
>    - Username: admin
>    - Password: admin (unless changed in .env)


>n8n Instances:
>    - Tenant 1: https://yourdomain.com/tenant1
>    - Tenant 2: https://yourdomain.com/tenant2

### Managing Tenants
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
   
## Troubleshooting
### Certificate errors

### Database connection issues

### Traefik routing problems

### Tenant creation fails
