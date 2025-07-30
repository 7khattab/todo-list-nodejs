# Todo List Node.js - DevOps Internship Task

This repository contains a Todo List application built with Node.js, MongoDB, and Express, along with complete DevOps infrastructure including Docker, CI/CD pipelines, and automated deployment.

## Project Structure

```
Todo-List-nodejs/
├── .github/
│   └── workflows/
│       └── ci-cd.yml                 ← GitHub Actions workflow
├── ansible/
│   ├── playbook.yml                  ← Ansible playbook (RedHat Linux)
│   └── inventory.ini                 ← Ansible inventory (VM IP or hostname)
├── docker-compose.yml                ← Docker Compose file (Part 3)
├── Dockerfile                        ← Docker image definition (Part 1)
├── .dockerignore                     ← Ignore node_modules, .env etc
├── .env                              ← MongoDB connection string
├── README.md                         ← Documentation with all steps
├── package.json
├── index.js
└── config/
    └── mongoose.js
```

## Project Overview

This project demonstrates a complete DevOps workflow including:
- **Part 1**: Dockerization and CI/CD pipeline with GitHub Actions
- **Part 2**: Infrastructure as Code with Ansible (RedHat Linux)
- **Part 3**: Docker Compose deployment

## Prerequisites

- Node.js 18+
- Docker and Docker Compose
- Ansible (for VM configuration)

## Quick Start

### Local Development

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/todo-list-nodejs.git
   cd todo-list-nodejs
   ```

2. Create a `.env` file:
   ```bash
   # MongoDB Connection String
   mongoDbUrl=mongodb://localhost:27017/todo-list

   # Application Configuration
   PORT=4000
   NODE_ENV=development
   ```

3. Install dependencies and start:
   ```bash
   npm install
   npm start
   ```

The application will be available at `http://localhost:4000`

## Part 1: Dockerization and CI/CD Pipeline

### Docker Setup

The application is containerized using a multi-stage Dockerfile with the following features:
- Node.js 18 Alpine base image for smaller size
- Non-root user for security
- Health checks for monitoring
- Production-optimized build

### GitHub Actions CI/CD

The CI/CD pipeline automatically:
- Builds Docker images on push to main/master branch
- Pushes images to GitHub Container Registry (ghcr.io)
- Supports multiple tags (branch, PR, SHA, latest)

**Workflow File**: `.github/workflows/ci-cd.yml`

### Usage

1. Fork this repository
2. Update the image name in `docker-compose.yml`
3. Push to trigger the CI/CD pipeline
4. Images will be available at `ghcr.io/your-username/todo-list-nodejs`

## Part 2: Ansible Infrastructure as Code

### VM Configuration

The Ansible playbook (`ansible/playbook.yml`) configures a RedHat Linux VM with:
- Docker and Docker Compose installation
- Application directory setup
- Yum package management
- Systemd service management

### Usage

1. Update the inventory file (`ansible/inventory.ini`):
   ```ini
   [todo_servers]
   todo-vm ansible_host=YOUR_VM_IP_ADDRESS ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa ansible_python_interpreter=/usr/bin/python3

   [all:vars]
   ansible_ssh_common_args='-o StrictHostKeyChecking=no'
   ```

2. Run the playbook:
   ```bash
   ansible-playbook -i ansible/inventory.ini ansible/playbook.yml
   ```

## Part 3: Docker Compose Deployment

### Features

- **Health Checks**: Both application and MongoDB have comprehensive health checks
- **Persistent Storage**: MongoDB data persistence
- **Network Isolation**: Custom bridge network
- **Automatic Restart**: Containers restart automatically on failure

### Usage

1. Deploy with Docker Compose:
   ```bash
   docker-compose up -d
   ```

2. Monitor logs:
   ```bash
   docker-compose logs -f
   ```

3. Check auto-update service:
   ```bash
   sudo journalctl -u todo-update -f
   ```

This README is simple, clear, and covers all the main steps for your project. If you want to add more details or sections, just let me know!
