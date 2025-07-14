---
layout: default
title: "API Reference"
description: "Complete REST API documentation with Supabase authentication, Portainer integration, and real-time capabilities"
---

# 📡 API Reference

## 🔐 Unified Authentication System

The API uses **Supabase Auth** for all user authentication and authorization. Container management operations are abstracted through our unified REST API, eliminating the need for direct Portainer authentication.

### Authentication Method

#### Supabase Authentication

```bash
curl -X POST https://your-supabase-url.supabase.co/auth/v1/token?grant_type=password \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "your-password"
  }'
```

### Authentication Response

**Supabase Authentication Response:**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "refresh_token_here",
  "expires_in": 3600,
  "token_type": "bearer",
  "user": {
    "id": "user-uuid",
    "email": "user@example.com",
    "user_metadata": {
      "permissions": ["agents:read", "agents:write", "monitoring:read", "containers:manage"]
    }
  }
}
```

### API Headers

All API requests use Supabase authentication:

```bash
curl -H "Authorization: Bearer supabase-jwt-token" \
     -H "apikey: your-supabase-anon-key" \
     -H "Content-Type: application/json" \
     https://your-domain.com/api/endpoint
```

**Note:** Container management operations (via Portainer) are handled internally by our API layer with proper authentication and authorization checks.

## 🤖 Enhanced Agent Management

### Create Agent with Portainer Integration

```bash
POST /api/agents
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
Content-Type: application/json
```

**Request Body:**
```json
{
  "agentName": "my-trading-bot",
  "description": "AI trading assistant with real-time market analysis",
  "character": {
    "name": "TradingBot",
    "bio": [
      "Expert cryptocurrency trader with real-time market analysis",
      "Provides investment insights and portfolio management",
      "Always stays up-to-date with market trends"
    ],
    "personality": "Professional, analytical, and data-driven",
    "instructions": [
      "Always provide accurate market data",
      "Explain trading strategies clearly",
      "Never give financial advice without disclaimers"
    ],
    "adjectives": ["analytical", "professional", "precise", "helpful"],
    "topics": ["cryptocurrency", "trading", "market analysis", "portfolio management"]
  },
  "plugins": [
    "@elizaos/plugin-discord",
    "@elizaos/plugin-web3",
    "@elizaos/plugin-coingecko"
  ],
  "platforms": ["discord"],
  "resources": {
    "memory": "1Gi",
    "cpu": "500m",
    "replicas": 3
  },
  "scaling": {
    "minReplicas": 1,
    "maxReplicas": 10,
    "targetCPU": 70,
    "targetMemory": 80
  },
  "environment": {
    "DISCORD_TOKEN": "your-discord-token",
    "COINGECKO_API_KEY": "your-api-key",
    "TRADING_API_KEY": "your-trading-key"
  },
  "portainer": {
    "template": "eliza-agent-template",
    "stack_name": "eliza-trading-bot",
    "deployment_strategy": "rolling",
    "health_checks": {
      "enabled": true,
      "interval": "30s",
      "timeout": "10s",
      "retries": 3
    }
  },
  "supabase_config": {
    "project_ref": "your-project-ref",
    "real_time": {
      "enabled": true,
      "channels": ["agent_status", "trading_signals", "performance_metrics"]
    }
  }
}
```

**Response:**
```json
{
  "success": true,
  "agent": {
    "id": "agent-uuid",
    "name": "my-trading-bot",
    "status": "deploying",
    "created_at": "2024-01-15T10:30:00Z",
    "supabase": {
      "database_id": "agent-record-uuid",
      "real_time_channel": "agent_status"
    },
    "portainer": {
      "stack_id": "123",
      "stack_name": "eliza-trading-bot",
      "endpoint_id": 1,
      "deployment_url": "https://portainer.yourdomain.com/#!/1/docker/stacks/123"
    }
  },
  "deployment": {
    "status": "initiating",
    "progress": 0,
    "estimated_completion": "2024-01-15T10:35:00Z",
    "steps": [
      {
        "name": "validate_configuration",
        "status": "completed",
        "completed_at": "2024-01-15T10:30:15Z"
      },
      {
        "name": "create_container_template",
        "status": "in_progress",
        "started_at": "2024-01-15T10:30:20Z"
      },
      {
        "name": "deploy_to_portainer",
        "status": "pending"
      },
      {
        "name": "setup_monitoring",
        "status": "pending"
      }
    ]
  }
}
```

### Get Agent Status with Container Metrics

```bash
GET /api/agents/{agentId}
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
Content-Type: application/json
```

**Response:**
```json
{
  "agent": {
    "id": "agent-uuid",
    "name": "my-trading-bot",
    "status": "running",
    "created_at": "2024-01-15T10:30:00Z",
    "deployed_at": "2024-01-15T10:35:00Z",
    "last_activity": "2024-01-15T15:45:00Z",
    "supabase": {
      "database_record": {
        "id": "agent-record-uuid",
        "updated_at": "2024-01-15T15:45:00Z"
      }
    },
    "portainer": {
      "stack_id": "123",
      "stack_status": "active",
      "containers": [
        {
          "id": "container-uuid",
          "name": "eliza-trading-bot_eliza-agent.1",
          "status": "running",
          "health": "healthy",
          "uptime": "5h 15m",
          "restart_count": 0
        }
      ],
      "services": [
        {
          "id": "service-uuid",
          "name": "eliza-trading-bot_eliza-agent",
          "replicas": {
            "running": 3,
            "desired": 3
          },
          "ports": [
            {
              "published": 3000,
              "target": 3000,
              "protocol": "tcp"
            }
          ]
        }
      ]
    },
    "metrics": {
      "cpu_usage": 45.2,
      "memory_usage": 768,
      "memory_limit": 1024,
      "network_rx": 12345678,
      "network_tx": 9876543,
      "requests_per_minute": 150,
      "error_rate": 0.01,
      "uptime": 18900
    },
    "health": {
      "status": "healthy",
      "last_check": "2024-01-15T15:45:00Z",
      "checks": [
        {
          "name": "http_health",
          "status": "passing",
          "response_time": "25ms"
        },
        {
          "name": "database_connection",
          "status": "passing",
          "response_time": "5ms"
        },
        {
          "name": "discord_connection",
          "status": "passing",
          "response_time": "150ms"
        }
      ]
    }
  }
}
```

### List All Agents with Container Status

```bash
GET /api/agents
```

**Query Parameters:**
- `status` (optional): Filter by agent status (running, stopped, deploying, failed)
- `platform` (optional): Filter by platform (discord, telegram, twitter)
- `limit` (optional): Number of results per page (default: 20, max: 100)
- `offset` (optional): Pagination offset (default: 0)

**Response:**
```json
{
  "agents": [
    {
      "id": "agent-uuid-1",
      "name": "trading-bot-1",
      "status": "running",
      "platform": "discord",
      "created_at": "2024-01-15T10:30:00Z",
      "containers": {
        "count": 3,
        "healthy": 3,
        "unhealthy": 0
      },
      "metrics": {
        "cpu_usage": 45.2,
        "memory_usage": 768,
        "requests_per_minute": 150
      }
    }
  ],
  "pagination": {
    "total": 50,
    "limit": 20,
    "offset": 0,
    "has_more": true
  },
  "summary": {
    "total_agents": 50,
    "running": 45,
    "stopped": 3,
    "deploying": 2,
    "failed": 0
  }
}
```

## 🎛️ Container Management

### Container Operations

**Note:** All container operations are abstracted through our unified API layer. Portainer integration is handled internally with proper authentication and authorization.

#### Scale Agent Containers

```bash
POST /api/agents/{agentId}/scale
```

**Request Body:**
```json
{
  "replicas": 5,
  "strategy": "rolling"
}
```

**Response:**
```json
{
  "success": true,
  "operation_id": "scale-op-uuid",
  "scaling": {
    "from_replicas": 3,
    "to_replicas": 5,
    "strategy": "rolling",
    "estimated_completion": "2024-01-15T16:05:00Z",
    "status": "in_progress"
  },
  "agent": {
    "id": "agent-uuid",
    "name": "my-trading-bot",
    "status": "scaling"
  }
}
```

#### Restart Agent Containers

```bash
POST /api/agents/{agentId}/restart
```

**Request Body:**
```json
{
  "restart_policy": "rolling",
  "max_unavailable": 1
}
```

#### Stop Agent

```bash
POST /api/agents/{agentId}/stop
```

#### Start Agent

```bash
POST /api/agents/{agentId}/start
```

### Container Logs

```bash
GET /api/agents/{agentId}/logs
```

**Query Parameters:**
- `lines` (optional): Number of log lines to retrieve (default: 100, max: 1000)
- `since` (optional): Show logs since timestamp (ISO 8601)
- `follow` (optional): Stream logs in real-time (true/false)
- `container` (optional): Specific container ID to get logs from

**Response:**
```json
{
  "logs": [
    {
      "timestamp": "2024-01-15T15:45:00Z",
      "container_id": "container-uuid",
      "container_name": "eliza-trading-bot_eliza-agent.1",
      "level": "info",
      "message": "Agent started successfully",
      "source": "eliza-core"
    },
    {
      "timestamp": "2024-01-15T15:45:01Z",
      "container_id": "container-uuid",
      "container_name": "eliza-trading-bot_eliza-agent.1",
      "level": "info",
      "message": "Connected to Discord",
      "source": "discord-client"
    }
  ],
  "pagination": {
    "total_lines": 1500,
    "returned_lines": 100,
    "has_more": true
  }
}
```

### Container Metrics

```bash
GET /api/agents/{agentId}/metrics
```

**Query Parameters:**
- `timeframe` (optional): Time range (1h, 6h, 24h, 7d, 30d)
- `interval` (optional): Data point interval (1m, 5m, 15m, 1h)

**Response:**
```json
{
  "metrics": {
    "cpu": {
      "current": 45.2,
      "average": 42.8,
      "max": 67.3,
      "min": 12.1,
      "data_points": [
        {
          "timestamp": "2024-01-15T15:40:00Z",
          "value": 43.5
        },
        {
          "timestamp": "2024-01-15T15:45:00Z",
          "value": 45.2
        }
      ]
    },
    "memory": {
      "current": 768,
      "average": 745,
      "max": 892,
      "min": 512,
      "limit": 1024,
      "data_points": [
        {
          "timestamp": "2024-01-15T15:40:00Z",
          "value": 756
        },
        {
          "timestamp": "2024-01-15T15:45:00Z",
          "value": 768
        }
      ]
    },
    "network": {
      "rx_bytes": 12345678,
      "tx_bytes": 9876543,
      "rx_rate": 1024,
      "tx_rate": 512
    }
  },
  "timeframe": "1h",
  "interval": "5m"
}
```

## 🏗️ Stack Management

### Deploy Stack Template

```bash
POST /api/stacks
```

**Request Body:**
```json
{
  "name": "eliza-agent-production",
  "template": "eliza-agent-template",
  "environment": [
    {
      "name": "AGENT_NAME",
      "value": "production-agent"
    },
    {
      "name": "DISCORD_TOKEN",
      "value": "your-discord-token"
    },
    {
      "name": "REPLICAS",
      "value": "5"
    }
  ],
  "labels": {
    "environment": "production",
    "team": "ai-agents",
    "project": "eliza-deployment"
  }
}
```

### Get Stack Status

```bash
GET /api/stacks/{stackId}
```

**Response:**
```json
{
  "stack": {
    "id": "123",
    "name": "eliza-agent-production",
    "status": "active",
    "created_at": "2024-01-15T10:30:00Z",
    "updated_at": "2024-01-15T15:45:00Z",
    "services": [
      {
        "name": "eliza-agent",
        "replicas": {
          "running": 5,
          "desired": 5
        },
        "image": "your-registry/eliza-agent:latest",
        "ports": [3000],
        "status": "running"
      },
      {
        "name": "agent-monitor",
        "replicas": {
          "running": 1,
          "desired": 1
        },
        "image": "your-registry/agent-monitor:latest",
        "status": "running"
      }
    ],
    "networks": [
      {
        "name": "eliza-agent-production_agent-network",
        "driver": "overlay",
        "encrypted": true
      }
    ],
    "volumes": [
      {
        "name": "eliza-agent-production_agent-data",
        "driver": "local"
      }
    ]
  }
}
```

### Update Stack

```bash
PUT /api/stacks/{stackId}
```

### Delete Stack

```bash
DELETE /api/stacks/{stackId}
```

## 📊 Real-time Monitoring Integration

### WebSocket Connection

```javascript
// Connect to real-time updates
const ws = new WebSocket('wss://your-domain.com/ws');

ws.onopen = function() {
  // Subscribe to agent updates
  ws.send(JSON.stringify({
    type: 'subscribe',
    channel: 'agent_status',
    token: 'your-jwt-token'
  }));
};

ws.onmessage = function(event) {
  const data = JSON.parse(event.data);
  
  switch (data.type) {
    case 'agent_status_update':
      console.log('Agent status changed:', data.payload);
      break;
    case 'container_event':
      console.log('Container event:', data.payload);
      break;
    case 'metric_update':
      console.log('Metrics updated:', data.payload);
      break;
  }
};
```

### Supabase Real-time Integration

```javascript
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(
  'https://your-project.supabase.co',
  'your-anon-key'
);

// Subscribe to agent status changes
const agentSubscription = supabase
  .channel('agent-updates')
  .on('postgres_changes', {
    event: '*',
    schema: 'public',
    table: 'agents'
  }, (payload) => {
    console.log('Agent updated:', payload);
  })
  .subscribe();

// Subscribe to container events
const containerSubscription = supabase
  .channel('container-events')
  .on('postgres_changes', {
    event: 'INSERT',
    schema: 'public',
    table: 'container_events'
  }, (payload) => {
    console.log('Container event:', payload);
  })
  .subscribe();
```

## 🔍 Advanced Queries

### Search Agents

```bash
GET /api/agents/search
```

**Query Parameters:**
- `q`: Search query (searches name, description, platform)
- `status`: Filter by status
- `platform`: Filter by platform
- `created_after`: Filter by creation date
- `created_before`: Filter by creation date
- `sort`: Sort by (name, created_at, last_activity, cpu_usage, memory_usage)
- `order`: Sort order (asc, desc)

**Response:**
```json
{
  "results": [
    {
      "id": "agent-uuid",
      "name": "trading-bot",
      "description": "AI trading assistant",
      "status": "running",
      "platform": "discord",
      "relevance_score": 0.95,
      "highlighted_fields": {
        "name": "<mark>trading</mark>-bot",
        "description": "AI <mark>trading</mark> assistant"
      }
    }
  ],
  "total_results": 15,
  "search_time": "45ms"
}
```

### Batch Operations

```bash
POST /api/agents/batch
```

**Request Body:**
```json
{
  "operation": "restart",
  "agent_ids": ["agent-uuid-1", "agent-uuid-2", "agent-uuid-3"],
  "options": {
    "restart_policy": "rolling",
    "max_concurrent": 2
  }
}
```

## 🛠️ Administrative APIs

### System Health

```bash
GET /api/health
```

**Response:**
```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T15:45:00Z",
  "components": {
    "supabase": {
      "status": "healthy",
      "response_time": "25ms",
      "last_check": "2024-01-15T15:45:00Z"
    },
    "portainer": {
      "status": "healthy",
      "response_time": "15ms",
      "last_check": "2024-01-15T15:45:00Z",
      "endpoints": {
        "kubernetes": "healthy",
        "docker": "healthy"
      }
    },
    "kubernetes": {
      "status": "healthy",
      "nodes": {
        "total": 5,
        "ready": 5,
        "not_ready": 0
      },
      "pods": {
        "total": 45,
        "running": 43,
        "pending": 2,
        "failed": 0
      }
    },
    "database": {
      "status": "healthy",
      "connections": {
        "active": 12,
        "idle": 8,
        "max": 100
      }
    }
  },
  "version": "1.0.0",
  "uptime": "72h 15m"
}
```

### System Metrics

```bash
GET /api/metrics
```

**Response:**
```json
{
  "system": {
    "cpu_usage": 34.5,
    "memory_usage": 67.2,
    "disk_usage": 45.8,
    "network_rx": 1234567890,
    "network_tx": 987654321
  },
  "containers": {
    "total": 45,
    "running": 43,
    "stopped": 2,
    "failed": 0
  },
  "agents": {
    "total": 15,
    "active": 14,
    "deploying": 1,
    "failed": 0
  },
  "performance": {
    "average_response_time": "125ms",
    "requests_per_minute": 1250,
    "error_rate": 0.02
  }
}
```

## 📚 SDK Examples

### Node.js SDK

```javascript
import { AgentLaunchpadClient } from '@your-org/agent-launchpad-sdk';

const client = new AgentLaunchpadClient({
  apiUrl: 'https://your-domain.com/api',
  supabaseUrl: 'https://your-project.supabase.co',
  supabaseKey: 'your-anon-key',
  portainerUrl: 'https://portainer.yourdomain.com'
});

// Authenticate
await client.authenticate({
  email: 'user@example.com',
  password: 'password'
});

// Create agent
const agent = await client.agents.create({
  name: 'my-bot',
  character: { /* character config */ },
  plugins: ['@elizaos/plugin-discord'],
  platforms: ['discord']
});

// Monitor deployment
const deployment = await client.agents.watchDeployment(agent.id);
deployment.on('progress', (progress) => {
  console.log(`Deployment progress: ${progress.percentage}%`);
});

deployment.on('complete', () => {
  console.log('Agent deployed successfully!');
});

// Get real-time metrics
const metrics = await client.agents.getMetrics(agent.id);
const metricsStream = client.agents.streamMetrics(agent.id);

metricsStream.on('data', (data) => {
  console.log('CPU:', data.cpu_usage);
  console.log('Memory:', data.memory_usage);
});
```

### Python SDK

```python
from agent_launchpad import AgentLaunchpadClient

client = AgentLaunchpadClient(
    api_url='https://your-domain.com/api',
    supabase_url='https://your-project.supabase.co',
    supabase_key='your-anon-key',
    portainer_url='https://portainer.yourdomain.com'
)

# Authenticate
client.authenticate(email='user@example.com', password='password')

# Create agent
agent = client.agents.create(
    name='my-bot',
    character={},
    plugins=['@elizaos/plugin-discord'],
    platforms=['discord']
)

# Monitor deployment
def on_progress(progress):
    print(f"Deployment progress: {progress['percentage']}%")

def on_complete():
    print("Agent deployed successfully!")

client.agents.watch_deployment(
    agent['id'],
    on_progress=on_progress,
    on_complete=on_complete
)

# Get metrics
metrics = client.agents.get_metrics(agent['id'])
print(f"CPU: {metrics['cpu_usage']}%")
print(f"Memory: {metrics['memory_usage']}MB")
```

## 🚨 Error Handling

### Standard Error Response

```json
{
  "error": {
    "code": "DEPLOYMENT_FAILED",
    "message": "Agent deployment failed due to insufficient resources",
    "details": {
      "component": "portainer",
      "stack_id": "123",
      "reason": "Resource limit exceeded",
      "required_memory": "2Gi",
      "available_memory": "1.5Gi"
    },
    "timestamp": "2024-01-15T15:45:00Z",
    "correlation_id": "error-uuid"
  }
}
```

### Common Error Codes

| Code | Description | HTTP Status |
|------|-------------|-------------|
| `AUTHENTICATION_FAILED` | Invalid credentials | 401 |
| `AUTHORIZATION_FAILED` | Insufficient permissions | 403 |
| `AGENT_NOT_FOUND` | Agent does not exist | 404 |
| `DEPLOYMENT_FAILED` | Container deployment failed | 500 |
| `RESOURCE_EXHAUSTED` | Insufficient cluster resources | 503 |
| `VALIDATION_ERROR` | Invalid request parameters | 400 |
| `RATE_LIMIT_EXCEEDED` | Too many requests | 429 |
| `CONTAINER_UNHEALTHY` | Container health check failed | 503 |

This unified API reference provides comprehensive documentation for managing agents through both Supabase and Portainer integration, offering real-time monitoring, container management, and scalable deployment capabilities. 
This unified API reference provides comprehensive documentation for managing agents through both Supabase and Portainer integration, offering real-time monitoring, container management, and scalable deployment capabilities. 
This unified API reference provides comprehensive documentation for managing agents through both Supabase and Portainer integration, offering real-time monitoring, container management, and scalable deployment capabilities. 