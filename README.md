# 🚀 DevOps Assessment – Yii2 + Docker Swarm + CI/CD + Ansible

## 📘 Overview

This project demonstrates deploying a sample **Yii2 PHP application** using:

- **Docker Swarm** for orchestration  
- **NGINX** running on the host as a reverse proxy  
- **GitHub Actions** for CI/CD automation  
- **Ansible** for infrastructure provisioning and application deployment
- **Grafana and Prometheus** for infrastructure monitoring 

---

## 🧭 Architecture

[User] --> [NGINX (host)] --> [Docker Swarm Service (Yii2 App Container)]


**CI/CD Flow:**

GitHub Push --> GitHub Actions --> Build Docker Image --> Push to Registry --> SSH to EC2 --> Update Swarm Service


---

## 🔧 Prerequisites

- AWS EC2 instance (Ubuntu recommended)
- SSH access to EC2 with a private key
- Docker Hub or GitHub Container Registry account
- Basic domain (optional for host-based routing)
- GitHub repository with this project

---

## 📁 Folder Structure

```
.
├── deliverables
│   ├── ansible
│   │   └── ansible_playbook.yml
│   ├── docker
│   │   ├── Dockerfile
│   │   └── docker-compose.yml
│   ├── github_workflow
│   │   └── first-actions.yml
│   ├── monitoring
│   │   ├── monitoring.yml
│   │   └── prometheus.yml
│   └── nginx
│       └── nginx-config.txt
├── monitoring
│   ├── monitoring.yml
│   └── prometheus.yml
└── my-yii2-app
    ├── Dockerfile
    └── docker-compose.yml
```


---

## 🚀 Deployment Steps

### 1️⃣ Provision with Ansible

Run the playbook to configure your EC2 instance:

2️⃣ Docker Swarm Deployment
Deploy your Yii2 app using:
docker stack deploy -c docker-compose.yml yii2app

### 3️⃣ NGINX Configuration (Host)

1. Edit the NGINX configuration file at `/etc/nginx/sites-available/default` or create a new file (e.g., `yii2.conf`) for your Yii2 app:

    ```bash
    sudo nano /etc/nginx/sites-available/yii2.conf
    ```

2. Link the configuration file to `/etc/nginx/sites-enabled/`:

    ```bash
    sudo ln -s /etc/nginx/sites-available/yii2.conf /etc/nginx/sites-enabled/
    ```

3. Test the NGINX configuration for syntax errors:

    ```bash
    sudo nginx -t
    ```

4. Restart the NGINX service and PHP-FPM (if you're using PHP 8.3):

    ```bash
    sudo systemctl restart nginx
    sudo systemctl restart php8.3-fpm
    ```

This will apply the changes and restart both NGINX and PHP-FPM to reflect the new reverse proxy setup for your Yii2 application.


4️⃣ GitHub Actions CI/CD


## 🔐 Environment Variables & Secrets

Add the following secrets to your GitHub repository under **Settings > Secrets**:

- **GHCR**: Your github conatiner registry access token.
- **PUBLICIP**: The IP address or hostname of your EC2 instance.
- **SSH_PRIVATE_KEY**: Your private SSH key to access the EC2 instance.

---

## 🧪 Testing the Deployment

1. **Push Code to the Main Branch**:
   - When code is pushed to the `main` branch, GitHub Actions will trigger the CI/CD pipeline.

2. **Watch GitHub Actions Run the Pipeline**:
   - GitHub Actions will run the pipeline, build the Docker container, and deploy the application to your EC2 instance.

3. **Verify the Deployment**:
   - After the pipeline completes, check your app at `http://yourdomain.com` or ip address.

4. **Check NGINX Reverse Proxy**:
   - Ensure that NGINX is correctly configured as a reverse proxy for your Docker container.

5. **Verify the Container Health**:
   - The Docker container should be healthy, and the NGINX reverse proxy should route traffic to the correct service.

---

## 💡 Assumptions

- The **Yii2 app** is minimal and public for demo purposes.
- **Docker Swarm** is running on a single-node EC2 instance.
- **GitHub Actions** can SSH into the EC2 instance using the provided private key.
- **NGINX** is running directly on the host machine and is configured as a reverse proxy for the Docker container.

---

## 🏁 Conclusion

This project demonstrates a complete DevOps workflow, including:

- **Docker containerization**: Creating a Docker container for your Yii2 app.
- **Infrastructure automation with Ansible**: Deploying the application using Ansible scripts (if applicable).
- **Continuous deployment using GitHub Actions**: Automatically deploying new changes from GitHub to the EC2 instance.
- **NGINX reverse proxy**: Managing the traffic and routing it to your Docker container.
- **Monitoring using Grafana**: Visualizing app metrics and system performance via Grafana.


Feel free to **fork**, **modify**, and **extend** this project for production use.

---
