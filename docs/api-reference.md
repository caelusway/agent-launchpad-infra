---
layout: default
title: "API Reference"
description: "Complete REST API documentation with examples and authentication"
---

# 📡 API Reference

## 🔐 Authentication

All API endpoints require authentication using API keys in the request header:

```bash
curl -H "X-API-Key: your-api-key" https://api.yourdomain.com/api/endpoint
```

### Generate API Key

```bash
POST /api/auth/generate-key
```

**Request Body:**
```json
{
  "username": "admin",
  "email": "admin@yourdomain.com",
  "role": "ADMIN"
}
```

**Response:**
```json
{
  "apiKey": "alk_a1b2c3d4e5f6...",
  "keyId": "key_123456789",
  "expiresAt": "2024-12-31T23:59:59Z"
}
```

## 🤖 Agent Management

### Create Agent

```bash
POST /api/agents
```

**Request Body:**
```json
{
  "agentName": "my-trading-bot",
  "description": "AI trading assistant",
  "personality": {
    "system": "You are a professional cryptocurrency trading assistant.",
    "bio": [
      "I'm an AI trading assistant specialized in cryptocurrency markets",
      "I provide technical analysis and market insights"
    ],
    "adjectives": ["analytical", "precise", "risk-aware"],
    "topics": ["cryptocurrency", "trading", "technical analysis"]
  },
  "selectedPlugins": [
    "@elizaos/plugin-discord",
    "@elizaos/plugin-web3"
  ],
  "resources": {
    "memory": "1Gi",
    "cpu": "500m",
    "replicas": 1
  },
  "platforms": {
    "discord": {
      "token": "your-discord-bot-token",
      "guildId": "your-server-id"
    }
  }
}
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "status": "QUEUED",
  "createdAt": "2024-01-15T10:30:00Z",
  "estimatedDeployTime": "5-10 minutes"
}
```

### Get Agent Status

```bash
GET /api/agents/{agentId}/status
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "status": "DEPLOYED",
  "url": "https://agent-123abc.agents.yourdomain.com",
  "health": {
    "overall": "HEALTHY",
    "kubernetes": {
      "status": "HEALTHY",
      "replicas": 1,
      "readyReplicas": 1
    },
    "application": {
      "status": "HEALTHY",
      "uptime": "2h 15m",
      "responseTime": "45ms"
    }
  },
  "deployedAt": "2024-01-15T10:35:00Z",
  "lastUpdated": "2024-01-15T12:45:00Z"
}
```

### List Agents

```bash
GET /api/agents
```

**Query Parameters:**
- `limit` (optional): Number of results (default: 20)
- `offset` (optional): Pagination offset (default: 0)
- `status` (optional): Filter by status
- `owner` (optional): Filter by owner (admin only)

**Response:**
```json
{
  "agents": [
    {
      "agentId": "agent-123abc",
      "name": "my-trading-bot",
      "status": "DEPLOYED",
      "owner": "user-456def",
      "createdAt": "2024-01-15T10:30:00Z",
      "url": "https://agent-123abc.agents.yourdomain.com"
    }
  ],
  "total": 1,
  "limit": 20,
  "offset": 0
}
```

### Scale Agent

```bash
POST /api/agents/{agentId}/scale
```

**Request Body:**
```json
{
  "replicas": 3
}
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "currentReplicas": 3,
  "targetReplicas": 3,
  "status": "SCALING",
  "updatedAt": "2024-01-15T13:00:00Z"
}
```

### Update Agent

```bash
PUT /api/agents/{agentId}
```

**Request Body:**
```json
{
  "description": "Updated description",
  "resources": {
    "memory": "2Gi",
    "cpu": "1000m"
  }
}
```

### Restart Agent

```bash
POST /api/agents/{agentId}/restart
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "status": "RESTARTING",
  "message": "Agent restart initiated",
  "timestamp": "2024-01-15T13:15:00Z"
}
```

### Delete Agent

```bash
DELETE /api/agents/{agentId}
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "status": "DELETING",
  "message": "Agent deletion initiated",
  "timestamp": "2024-01-15T13:20:00Z"
}
```

## 📊 Monitoring & Metrics

### Get Agent Metrics

```bash
GET /api/agents/{agentId}/metrics
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "metrics": {
    "cpu": {
      "usage": 0.45,
      "limit": 1.0,
      "percentage": 45
    },
    "memory": {
      "usage": 536870912,
      "limit": 1073741824,
      "percentage": 50
    },
    "network": {
      "bytesIn": 1048576,
      "bytesOut": 524288
    },
    "requests": {
      "total": 1250,
      "success": 1200,
      "errors": 50,
      "rate": 10.5
    }
  },
  "timestamp": "2024-01-15T13:25:00Z"
}
```

### Get Agent Logs

```bash
GET /api/agents/{agentId}/logs
```

**Query Parameters:**
- `lines` (optional): Number of log lines (default: 100)
- `since` (optional): Time since logs (e.g., "1h", "30m")
- `follow` (optional): Stream logs (default: false)

**Response:**
```json
{
  "agentId": "agent-123abc",
  "logs": [
    {
      "timestamp": "2024-01-15T13:20:00Z",
      "level": "INFO",
      "message": "Agent started successfully",
      "pod": "agent-123abc-7d4f8c9b-xz9k2"
    },
    {
      "timestamp": "2024-01-15T13:21:00Z",
      "level": "INFO",
      "message": "Connected to Discord",
      "pod": "agent-123abc-7d4f8c9b-xz9k2"
    }
  ],
  "totalLines": 2,
  "truncated": false
}
```

## 🔒 Security

### Container Security Scan

```bash
POST /api/security/scan/{agentId}
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "scanId": "scan-789xyz",
  "status": "COMPLETED",
  "results": {
    "passed": true,
    "vulnerabilities": {
      "critical": 0,
      "high": 1,
      "medium": 5,
      "low": 12
    },
    "recommendations": [
      "Update base image to latest version",
      "Remove unnecessary packages"
    ]
  },
  "scannedAt": "2024-01-15T13:30:00Z"
}
```

### Security Events

```bash
GET /api/security/events
```

**Query Parameters:**
- `limit` (optional): Number of events (default: 50)
- `severity` (optional): Filter by severity (LOW, MEDIUM, HIGH, CRITICAL)
- `since` (optional): Events since timestamp

**Response:**
```json
{
  "events": [
    {
      "id": "event-456def",
      "type": "SECURITY_SCAN_FAILED",
      "severity": "HIGH",
      "agentId": "agent-123abc",
      "message": "Container security scan failed",
      "timestamp": "2024-01-15T13:25:00Z",
      "details": {
        "scanId": "scan-789xyz",
        "reason": "Critical vulnerability detected"
      }
    }
  ],
  "total": 1
}
```

## 🏥 Health & Status

### System Health

```bash
GET /api/health
```

**Response:**
```json
{
  "status": "HEALTHY",
  "timestamp": "2024-01-15T13:35:00Z",
  "version": "1.0.0",
  "components": {
    "database": {
      "status": "HEALTHY",
      "responseTime": "2ms",
      "connections": 5
    },
    "kubernetes": {
      "status": "HEALTHY",
      "nodes": 3,
      "pods": 15
    },
    "redis": {
      "status": "HEALTHY",
      "memory": "45MB",
      "connections": 12
    },
    "registry": {
      "status": "HEALTHY",
      "available": true
    }
  }
}
```

### System Metrics

```bash
GET /api/system/metrics
```

**Response:**
```json
{
  "system": {
    "totalAgents": 5,
    "runningAgents": 4,
    "failedAgents": 1,
    "deployments": {
      "successful": 95,
      "failed": 3,
      "successRate": 96.9
    }
  },
  "resources": {
    "cpu": {
      "total": 16.0,
      "used": 8.5,
      "percentage": 53.1
    },
    "memory": {
      "total": 67108864000,
      "used": 33554432000,
      "percentage": 50.0
    },
    "storage": {
      "total": 1099511627776,
      "used": 549755813888,
      "percentage": 50.0
    }
  },
  "timestamp": "2024-01-15T13:40:00Z"
}
```

## 👥 User Management

### Get User Profile

```bash
GET /api/users/profile
```

**Response:**
```json
{
  "userId": "user-456def",
  "username": "admin",
  "email": "admin@yourdomain.com",
  "role": "ADMIN",
  "createdAt": "2024-01-01T00:00:00Z",
  "lastLogin": "2024-01-15T10:00:00Z",
  "permissions": [
    "agents:*",
    "system:*",
    "users:*"
  ]
}
```

### Update User Profile

```bash
PUT /api/users/profile
```

**Request Body:**
```json
{
  "email": "newemail@yourdomain.com",
  "notifications": {
    "email": true,
    "slack": false
  }
}
```

## 🚨 Error Handling

### Error Response Format

```json
{
  "error": {
    "code": "AGENT_NOT_FOUND",
    "message": "Agent with ID 'agent-123abc' not found",
    "timestamp": "2024-01-15T13:45:00Z",
    "requestId": "req-789xyz"
  }
}
```

### Common Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `INVALID_API_KEY` | 401 | API key is invalid or expired |
| `INSUFFICIENT_PERMISSIONS` | 403 | User lacks required permissions |
| `AGENT_NOT_FOUND` | 404 | Agent does not exist |
| `VALIDATION_ERROR` | 400 | Request validation failed |
| `DEPLOYMENT_FAILED` | 500 | Agent deployment failed |
| `RESOURCE_LIMIT_EXCEEDED` | 429 | Resource quota exceeded |
| `SECURITY_SCAN_FAILED` | 422 | Security scan blocked deployment |

## 📋 Request/Response Examples

### Complete Agent Creation Flow

```bash
# 1. Create agent
curl -X POST https://api.yourdomain.com/api/agents \
  -H "X-API-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{
    "agentName": "discord-bot",
    "description": "Community Discord bot",
    "personality": {
      "system": "You are a helpful community moderator.",
      "bio": ["I help manage Discord communities"],
      "adjectives": ["helpful", "friendly", "professional"]
    },
    "selectedPlugins": ["@elizaos/plugin-discord"],
    "resources": {
      "memory": "512Mi",
      "cpu": "250m"
    },
    "platforms": {
      "discord": {
        "token": "your-discord-token"
      }
    }
  }'

# Response
{
  "agentId": "agent-discord-001",
  "status": "QUEUED",
  "createdAt": "2024-01-15T14:00:00Z"
}

# 2. Check status
curl -H "X-API-Key: your-api-key" \
  https://api.yourdomain.com/api/agents/agent-discord-001/status

# Response
{
  "agentId": "agent-discord-001",
  "status": "DEPLOYED",
  "url": "https://agent-discord-001.agents.yourdomain.com",
  "health": {
    "overall": "HEALTHY"
  }
}

# 3. Scale up
curl -X POST https://api.yourdomain.com/api/agents/agent-discord-001/scale \
  -H "X-API-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"replicas": 2}'

# Response
{
  "agentId": "agent-discord-001",
  "currentReplicas": 2,
  "status": "SCALING"
}
```

## 🔧 Rate Limiting

API endpoints are rate-limited to prevent abuse:

- **Default Limit**: 100 requests per 15 minutes per API key
- **Burst Limit**: 20 requests per minute
- **Headers**: Rate limit information is returned in response headers

```bash
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1642252800
```

## 📖 SDKs and Libraries

### Node.js SDK

```javascript
const { AgentLaunchpadClient } = require('@agent-launchpad/sdk');

const client = new AgentLaunchpadClient({
  apiKey: 'your-api-key',
  baseUrl: 'https://api.yourdomain.com'
});

// Create agent
const agent = await client.agents.create({
  agentName: 'my-bot',
  description: 'My AI bot',
  // ... other options
});

// Get status
const status = await client.agents.getStatus(agent.agentId);
```

### Python SDK

```python
from agent_launchpad import AgentLaunchpadClient

client = AgentLaunchpadClient(
    api_key='your-api-key',
    base_url='https://api.yourdomain.com'
)

# Create agent
agent = client.agents.create(
    agent_name='my-bot',
    description='My AI bot',
    # ... other options
)

# Get status
status = client.agents.get_status(agent['agentId'])
```

## 🚀 Next Steps

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 15px; margin: 20px 0;">

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #4CAF50;">
<h4><a href="/getting-started" style="text-decoration: none; color: #4CAF50;">🚀 Getting Started</a></h4>
<p>Set up your first agent deployment</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #F44336;">
<h4><a href="/security" style="text-decoration: none; color: #F44336;">🔐 Security Guide</a></h4>
<p>Learn about API security and best practices</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #795548;">
<h4><a href="/use-cases" style="text-decoration: none; color: #795548;">💡 Use Cases</a></h4>
<p>Explore real-world API usage examples</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #E91E63;">
<h4><a href="/support" style="text-decoration: none; color: #E91E63;">📞 Support</a></h4>
<p>Get help with API integration</p>
</div>

</div> 