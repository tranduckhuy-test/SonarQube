# SonarQube Server with Let's Encrypt SSL

This repository contains a Docker Compose setup for running SonarQube with PostgreSQL and Nginx with Let's Encrypt SSL certificates.

## Prerequisites

- A VPS or server with Docker and Docker Compose installed
- A domain name pointing to your server's IP address
- Ports 80 and 443 open on your firewall

## Setup Instructions

### 1. Prepare Your Domain

Make sure your domain (e.g., sonarqube.yourdomain.com) is pointing to your server's IP address. DNS propagation might take some time.

### 2. Customize Configuration

1. Edit the `nginx/conf.d/sonarqube.conf` file:
   - Replace `your-domain.com` with your actual domain name in both server blocks

2. Edit the `init-letsencrypt.sh` script:
   - Replace `your-domain.com` with your actual domain
   - Replace `your-email@example.com` with your actual email address

3. Make the script executable:
   ```
   chmod +x init-letsencrypt.sh
   ```

### 3. Initialize Let's Encrypt Certificates

Run the initialization script to obtain SSL certificates:

```
./init-letsencrypt.sh
```

This script will:
- Create a dummy SSL certificate
- Start Nginx
- Request a real certificate from Let's Encrypt
- Replace the dummy certificate with the real one
- Reload Nginx configuration

### 4. Start the SonarQube Server

Start all services:

```
docker compose up -d
```

### 5. Access SonarQube

Open your browser and navigate to https://your-domain.com

Log in with the default credentials:
- Username: admin
- Password: admin

You'll be prompted to change the password on first login.

## Maintenance

### Certificate Renewal

Let's Encrypt certificates are valid for 90 days. The Certbot container will automatically try to renew them when they're 30 days from expiration.

### Updating SonarQube

To update to a newer SonarQube version:

1. Edit the `docker-compose.yaml` file and update the SonarQube image tag
2. Run:
   ```
   docker compose down
   docker compose up -d
   ```

## Troubleshooting

### Certificate Issues

If you encounter problems with the certificates, you can check the Certbot logs:

```
docker compose logs certbot
```

### SonarQube Won't Start

Check SonarQube logs for errors:

```
docker compose logs sonarqube
```

Common issues include:
- Elasticsearch memory settings (may require adjusting system settings)
- Database connection problems
