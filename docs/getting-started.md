---
layout: default
title: "Getting Started"
description: "Step-by-step guide to set up and deploy your first agent"
---

# 🚀 Getting Started

## 🎯 Overview

This guide will walk you through setting up the Multi-Agent Infrastructure at Scale infrastructure and deploying your first Eliza-based AI agent. Follow these steps to get up and running in minutes.

## 📋 Prerequisites

Before you begin, ensure you have:

### Required Software
- **Node.js** (version 23 or higher) - Required for Eliza
- **Bun** package manager - Recommended for Eliza projects
- **Docker** (version 20.10 or higher)
- **Kubernetes** cluster (local or cloud)
- **kubectl** configured and connected to your cluster
- **Eliza CLI** - Install with `bun install -g @elizaos/cli`

### Required Services
- **PostgreSQL** database (version 12 or higher)
- **Container Registry** (Docker Hub, AWS ECR, GCP Registry, etc.)
- **Domain** with SSL certificate (for production)

### Access Requirements
- **API Keys** for platforms you want to integrate (Discord, Telegram, etc.)
- **Cloud credentials** if using cloud services
- **Admin access** to your Kubernetes cluster

## 🏗️ Installation

### Step 1: Install Eliza CLI

```bash
# Install the ElizaOS CLI globally
bun install -g @elizaos/cli

# Verify installation
elizaos --version

# Get help with available commands
elizaos --help
```

### Step 2: Clone the Repository

```bash
# Clone the repository
git clone https://github.com/yourusername/agent-launchpad-infra.git
cd agent-launchpad-infra
```

### Step 3: Install Dependencies

```bash
# Install dependencies using Bun (recommended for Eliza projects)
bun install

# Or if using npm
npm install
```

### Step 4: Configure Environment

```bash
# Copy environment template
cp .env.example .env

# Edit configuration using Eliza CLI
elizaos env edit-local

# Or manually edit the .env file
nano .env
```

#### Required Environment Variables

```bash
# Database Configuration
DATABASE_URL=postgresql://user:password@localhost:5432/agent_launchpad
DATABASE_SSL=true

# Container Registry
CONTAINER_REGISTRY_URL=your-registry.com/agent-launchpad
CONTAINER_REGISTRY_USERNAME=your-username
CONTAINER_REGISTRY_PASSWORD=your-password

# Kubernetes Configuration
KUBECONFIG_PATH=/path/to/your/kubeconfig
KUBERNETES_NAMESPACE=agents

# API Configuration
API_PORT=3000
API_HOST=0.0.0.0
JWT_SECRET=your-super-secret-jwt-key

# Security
ENCRYPTION_KEY=your-32-character-encryption-key
API_RATE_LIMIT=100
API_RATE_WINDOW=900000

# Monitoring
PROMETHEUS_URL=http://prometheus:9090
GRAFANA_URL=http://grafana:3000
ALERT_WEBHOOK_URL=https://your-webhook-url.com
```

### Step 4: Database Setup

```bash
# Create database (if not exists)
createdb agent_launchpad

# Run database migrations
npm run db:migrate

# Seed initial data
npm run db:seed
```

### Step 5: Deploy Core Infrastructure

```bash
# Deploy PostgreSQL (if not external)
kubectl apply -f k8s/postgres/

# Deploy Redis
kubectl apply -f k8s/redis/

# Deploy monitoring stack
kubectl apply -f k8s/monitoring/

# Deploy the main application
kubectl apply -f k8s/app/
```

### Step 6: Verify Installation

```bash
# Check all pods are running
kubectl get pods -n agents

# Check services
kubectl get services -n agents

# Check ingress
kubectl get ingress -n agents
```

## 🔧 Configuration

### API Key Setup

Generate an API key for authentication:

```bash
# Generate API key
npm run generate-api-key

# Or using curl after deployment
curl -X POST http://localhost:3000/api/auth/generate-key \
  -H "Content-Type: application/json" \
  -d '{
    "username": "admin",
    "email": "admin@yourdomain.com",
    "role": "ADMIN"
  }'
```

### Container Registry Configuration

Configure your container registry:

```bash
# For Docker Hub
docker login

# For AWS ECR
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com

# For GCP Container Registry
gcloud auth configure-docker
```

### SSL Certificate Setup

For production deployments, configure SSL certificates:

```bash
# Using cert-manager with Let's Encrypt
kubectl apply -f k8s/cert-manager/

# Apply certificate issuer
kubectl apply -f k8s/certificates/
```

## 🤖 Deploy Your First Agent

### Step 1: Prepare Agent Configuration

Create your first agent configuration:

```json
{
  "agentName": "my-first-bot",
  "description": "My first AI agent using Eliza",
  "personality": {
    "system": "You are a helpful AI assistant that can answer questions and have conversations.",
    "bio": [
      "I'm an AI assistant created with Eliza",
      "I can help with various tasks and questions",
      "I'm friendly and always ready to help"
    ],
    "adjectives": ["helpful", "friendly", "knowledgeable", "patient"],
    "topics": ["general", "technology", "help", "conversation"]
  },
  "selectedPlugins": [
    "@elizaos/plugin-discord"
  ],
  "resources": {
    "memory": "512Mi",
    "cpu": "250m"
  },
  "platforms": {
    "discord": {
      "token": "your-discord-bot-token"
    }
  }
}
```

### Step 2: Deploy Using API

```bash
# Deploy the agent
curl -X POST http://localhost:3000/api/agents \
  -H "X-API-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d @agent-config.json
```

### Step 3: Monitor Deployment

```bash
# Check deployment status
curl -H "X-API-Key: your-api-key" \
  http://localhost:3000/api/agents/my-first-bot/status

# Watch pod creation
kubectl get pods -n agents -w

# Check logs
kubectl logs -n agents -l app=eliza-agent,agent-id=my-first-bot
```

## 🔍 Development Setup

### Local Development

For local development and testing:

```bash
# Start development server
npm run dev

# Run tests
npm test

# Run linting
npm run lint

# Build production
npm run build
```

### Development Flow

```mermaid
flowchart TD
    Start([🚀 Start Development]) --> Clone[📥 Clone Repository]
    Clone --> Install[📦 Install Dependencies]
    Install --> Configure[⚙️ Configure Environment]
    Configure --> Database[🗄️ Setup Database]
    Database --> LocalK8s[☸️ Setup Local Kubernetes]
    LocalK8s --> Deploy[🚀 Deploy Services]
    Deploy --> Test[✅ Test Deployment]
    Test --> Ready[✅ Ready for Development]
    
    Ready --> Code[💻 Code Changes]
    Code --> TestLocal[🧪 Local Testing]
    TestLocal --> BuildImage[🔨 Build Container]
    BuildImage --> Deploy
    
    style Start fill:#e8f5e8
    style Ready fill:#4caf50
    style Code fill:#e1f5fe
    style TestLocal fill:#fff3e0
```

### Testing Your Setup

```bash
# Test database connection
npm run test:db

# Test Kubernetes connection
npm run test:k8s

# Test container registry
npm run test:registry

# Run integration tests
npm run test:integration

# Health check
curl http://localhost:3000/health
```

## 📊 Monitoring Your Deployment

### Access Monitoring Dashboards

1. **Grafana Dashboard**: `http://grafana.yourdomain.com`
   - Username: `admin`
   - Password: Check `kubectl get secret grafana-admin-secret -o jsonpath="{.data.password}" | base64 -d`

2. **Prometheus**: `http://prometheus.yourdomain.com`

3. **Agent Metrics**: `http://localhost:3000/api/agents/{agent-id}/metrics`

### Key Metrics to Watch

| Metric | Description | Healthy Range |
|--------|-------------|---------------|
| **CPU Usage** | Container CPU utilization | < 70% |
| **Memory Usage** | Container memory usage | < 80% |
| **Response Time** | API response latency | < 100ms |
| **Error Rate** | Request error percentage | < 1% |
| **Deployment Success** | Successful deployments | > 95% |

## 🚨 Troubleshooting

### Common Issues

#### 1. Database Connection Failed
```bash
# Check database status
kubectl get pods -n postgres

# Check connection
psql -h localhost -U postgres -d agent_launchpad

# Check logs
kubectl logs -n postgres postgres-0
```

#### 2. Container Build Failed
```bash
# Check build service logs
kubectl logs -n agents -l app=build-service

# Check registry access
docker login your-registry.com

# Manual build test
docker build -t test-image .
```

#### 3. Agent Not Starting
```bash
# Check agent pod status
kubectl describe pod -n agents agent-{id}

# Check agent logs
kubectl logs -n agents agent-{id}

# Check resource limits
kubectl top pods -n agents
```

#### 4. Network Issues
```bash
# Check ingress
kubectl get ingress -n agents

# Check service endpoints
kubectl get endpoints -n agents

# Test internal connectivity
kubectl exec -it test-pod -- curl http://api-service:3000/health
```

### Getting Help

If you encounter issues:

1. **Check the logs** - Always start with application and system logs
2. **Review configuration** - Ensure all environment variables are set correctly
3. **Verify resources** - Check CPU, memory, and storage availability
4. **Test connectivity** - Verify network access between components

<div class="alert alert-info">
<strong>💡 Pro Tip:</strong> Enable debug logging by setting <code>LOG_LEVEL=debug</code> in your environment variables for detailed troubleshooting information.
</div>

## 🎉 What's Next?

Congratulations! You've successfully set up the Multi-Agent Infrastructure at Scale. Here's what you can do next:

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 15px; margin: 20px 0;">

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #FF9800;">
<h4><a href="api-reference" style="text-decoration: none; color: #FF9800;">📡 Explore APIs</a></h4>
<p>Learn about all available endpoints and features</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #F44336;">
<h4><a href="security" style="text-decoration: none; color: #F44336;">🔐 Security Setup</a></h4>
<p>Configure security best practices</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #795548;">
<h4><a href="use-cases" style="text-decoration: none; color: #795548;">💡 Use Cases</a></h4>
<p>Explore real-world examples and templates</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #607D8B;">
<h4><a href="monitoring" style="text-decoration: none; color: #607D8B;">📊 Advanced Monitoring</a></h4>
<p>Set up comprehensive monitoring and alerting</p>
</div>

</div>

---

## 🏁 Quick Reference

### Essential Commands

```bash
# Check system status
kubectl get pods -n agents
curl -H "X-API-Key: $API_KEY" http://localhost:3000/api/system/health

# Deploy new agent
curl -X POST -H "X-API-Key: $API_KEY" \
  http://localhost:3000/api/agents \
  -d @agent-config.json

# Scale agent
curl -X POST -H "X-API-Key: $API_KEY" \
  http://localhost:3000/api/agents/{id}/scale \
  -d '{"replicas": 3}'

# Check agent status
curl -H "X-API-Key: $API_KEY" \
  http://localhost:3000/api/agents/{id}/status
```

### Important Files

- `k8s/` - Kubernetes manifests
- `.env` - Environment configuration
- `config/` - Application configuration
- `docs/` - Documentation files

Ready to build amazing AI agents? Let's get started! 🚀 