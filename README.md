# 🚀 WordPress Dockerized CI/CD on Azure

This repository contains a professional-grade, containerized WordPress setup automated with GitHub Actions. It features an immutable deployment strategy, SSL encryption, and persistent storage on an Azure Ubuntu VM.

---

## 🏗️ Architecture

* **CI/CD**: GitHub Actions (Build -> Push -> Deploy).

* **Container Registry**: GitHub Packages (GHCR).

* **Web Server**: Nginx (Running inside Docker via Supervisor).

* **Process Manager**: Supervisor (Manages Nginx & PHP-FPM).

* **Database**: MariaDB/MySQL (Installed on Azure Host).

* **SSL**: Let's Encrypt (Certbot).

---

## 🛠️ Tech Stack

* **Docker**: Containerization of the application environment.

* **PHP 8.2-FPM**: Core engine for WordPress.

* **Nginx**: High-performance web server and reverse proxy.

* **GitHub Actions**: Automation of the build and deployment pipeline.

* **Azure VM**: Cloud infrastructure (Ubuntu 22.04 LTS).
    
---

## 📂 Project Structure

```text

├── .github/workflows/
│   └── deploy.yml       # The CI/CD Pipeline
├── wp-content/          # Themes, Plugins, and Languages
├── Dockerfile           # Environment definition
├── nginx.conf           # Nginx server block configuration
├── supervisord.conf     # Process manager configuration
├── wp-config.php        # WordPress Database configuration
└── README.md            # You are here!

```

---

## 🚀 Getting Started
* **Prerequisites**

    * An Azure VM with Docker installed.

    * Port 80 and 443 open in Azure Network Security Group (NSG).

    * A domain name pointing to the VM's Public IP.

* **GitHub Secrets**

Add the following secrets to your repository (Settings > Secrets and variables > Actions):

* **VM_IPADDRESS**: Public IP of your Azure VM.

* **VM_USERNAME**: The SSH username (e.g., azureuser).

* **SSH_PRIVATE_KEY**: Your private SSH key for VM access.

---

## ⚙️ Deployment Strategy

This project uses Immutable Deployments:

* **Build**: A new Docker image is created for every push, tagged with the unique GITHUB_SHA.

* **Push**: The image is pushed to GitHub Container Registry.

* **Atomic Swap**: The VM pulls the new image, stops the old container, and starts the new one instantly.

* **Persistence**: The /var/www/html/wp-content/uploads directory is mounted as a volume on the host to prevent data loss during deployments.

---
   
## 🛡️ Security & Maintenance

* **SSL**: Handled via Certbot on the host or inside a proxy container.

* **Pruning**: The deployment script automatically runs docker image prune to save disk space.

* **Database**: Ensure DB_HOST in wp-config.php points to the Docker bridge IP (172.17.0.1)

