---
layout: default
title: "API Reference"
description: "Complete REST API documentation with Supabase authentication and real-time capabilities"
---

# 📡 API Reference

## 🔐 Supabase Authentication

All API endpoints use **Supabase Auth** for enterprise-grade authentication. The system supports multiple authentication methods with automatic JWT token management.

### Authentication Methods

#### Email/Password Authentication

```bash
curl -X POST https://your-supabase-url.supabase.co/auth/v1/token?grant_type=password \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "your-password"
  }'
```

#### OAuth Authentication

```bash
curl -X POST https://your-supabase-url.supabase.co/auth/v1/authorize \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "redirect_to": "https://your-domain.com/auth/callback"
  }'
```

#### Magic Link Authentication

```bash
curl -X POST https://your-supabase-url.supabase.co/auth/v1/magiclink \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com"
  }'
```

### Authentication Response

**Successful Authentication Response:**
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
      "permissions": ["agents:read", "agents:write", "monitoring:read"]
    }
  }
}
```

### Using Supabase JWT Tokens

All API requests require both the JWT token and Supabase API key:

```bash
curl -H "Authorization: Bearer supabase-jwt-token" \
     -H "apikey: your-supabase-anon-key" \
     https://your-domain.com/api/endpoint
```

## 🤖 Agent Management

### Create Agent

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
  "description": "AI trading assistant",
  "character": {
    "name": "TradingBot",
    "bio": "Expert cryptocurrency trader with real-time market analysis",
    "personality": "Professional, analytical, and helpful",
    "system": "You are a professional cryptocurrency trading assistant.",
    "adjectives": ["analytical", "precise", "risk-aware"],
    "topics": ["cryptocurrency", "trading", "technical analysis"]
  },
  "plugins": [
    "@elizaos/plugin-discord",
    "@elizaos/plugin-web3",
    "@elizaos/plugin-supabase"
  ],
  "platforms": ["discord"],
  "resources": {
    "memory": "1Gi",
    "cpu": "500m",
    "replicas": 1
  },
  "environment": {
    "DISCORD_TOKEN": "your-discord-bot-token",
    "COINGECKO_API_KEY": "your-api-key"
  },
  "supabase_config": {
    "project_ref": "your-project-ref",
    "database_url": "postgresql://postgres:[password]@db.your-project.supabase.co:5432/postgres",
    "real_time": {
      "enabled": true,
      "channels": ["agent_status", "user_interactions", "performance_metrics"]
    }
  }
}
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "status": "QUEUED",
  "user_id": "user-uuid",
  "createdAt": "2024-01-15T10:30:00Z",
  "estimatedDeployTime": "5-10 minutes",
  "supabase_integration": {
    "tables_created": true,
    "rls_policies_applied": true,
    "real_time_enabled": true
  }
}
```

### Get Agent Status

```bash
GET /api/agents/{agentId}/status
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "name": "my-trading-bot",
  "status": "DEPLOYED",
  "user_id": "user-uuid",
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
    },
    "supabase": {
      "connection": "HEALTHY",
      "real_time_active": true,
      "rls_policies": "ACTIVE"
    }
  },
  "supabase_config": {
    "project_ref": "your-project-ref",
    "tables": {
      "conversations": "agent_conversations",
      "metrics": "agent_metrics",
      "logs": "agent_logs"
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

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Query Parameters:**
- `limit` (optional): Number of results (default: 20)
- `offset` (optional): Pagination offset (default: 0)
- `status` (optional): Filter by status
- `platform` (optional): Filter by platform

**Note:** Due to Row-Level Security (RLS), users can only see their own agents.

**Response:**
```json
{
  "agents": [
    {
      "agentId": "agent-123abc",
      "name": "my-trading-bot",
      "status": "DEPLOYED",
      "user_id": "user-uuid",
      "platforms": ["discord"],
      "supabase_integration": true,
      "createdAt": "2024-01-15T10:30:00Z",
      "url": "https://agent-123abc.agents.yourdomain.com"
    }
  ],
  "total": 1,
  "limit": 20,
  "offset": 0,
  "user_id": "user-uuid"
}
```

### Direct Supabase Query

You can also query agents directly using Supabase REST API:

```bash
GET https://your-supabase-url.supabase.co/rest/v1/agents
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Response:** (RLS automatically filters to user's agents)
```json
[
  {
    "id": "agent-123abc",
    "name": "my-trading-bot",
    "status": "deployed",
    "user_id": "user-uuid",
    "character": {...},
    "plugins": ["@elizaos/plugin-discord"],
    "created_at": "2024-01-15T10:30:00Z",
    "updated_at": "2024-01-15T12:45:00Z"
  }
]
```

### Scale Agent

```bash
POST /api/agents/{agentId}/scale
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
  "replicas": 3,
  "scaling_policy": {
    "min_replicas": 1,
    "max_replicas": 5,
    "target_cpu": 70
  }
}
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "user_id": "user-uuid",
  "currentReplicas": 3,
  "targetReplicas": 3,
  "status": "SCALING",
  "scaling_event_logged": true,
  "supabase_event_id": "event-456def",
  "updatedAt": "2024-01-15T13:00:00Z"
}
```

**Supabase Event Log:**
```bash
# Scaling events are automatically logged to Supabase
GET https://your-supabase-url.supabase.co/rest/v1/agent_events?agent_id=eq.agent-123abc&event_type=eq.SCALING
```

### Update Agent

```bash
PUT /api/agents/{agentId}
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
  "description": "Updated description",
  "resources": {
    "memory": "2Gi",
    "cpu": "1000m"
  },
  "supabase_config": {
    "real_time": {
      "enabled": true,
      "channels": ["agent_status", "user_interactions", "performance_metrics", "error_logs"]
    },
    "storage": {
      "bucket": "agent-assets",
      "public_access": false
    }
  }
}
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "user_id": "user-uuid",
  "status": "UPDATING",
  "changes_applied": {
    "resources": true,
    "supabase_config": true
  },
  "supabase_event_id": "event-789xyz",
  "updatedAt": "2024-01-15T13:10:00Z"
}
```

### Update Agent via Supabase

You can also update agents directly using Supabase REST API:

```bash
PATCH https://your-supabase-url.supabase.co/rest/v1/agents?id=eq.agent-123abc
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
  "status": "updated",
  "resources": {"memory": "2Gi", "cpu": "1000m"},
  "updated_at": "2024-01-15T13:10:00Z"
}
```

### Restart Agent

```bash
POST /api/agents/{agentId}/restart
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "user_id": "user-uuid",
  "status": "RESTARTING",
  "message": "Agent restart initiated",
  "supabase_event_logged": true,
  "real_time_notification_sent": true,
  "timestamp": "2024-01-15T13:15:00Z"
}
```

### Delete Agent

```bash
DELETE /api/agents/{agentId}
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "user_id": "user-uuid",
  "status": "DELETING",
  "message": "Agent deletion initiated",
  "supabase_cleanup": {
    "data_retention_policy": "30_days",
    "personal_data_removal": "immediate",
    "audit_log_preserved": true
  },
  "timestamp": "2024-01-15T13:20:00Z"
}
```

**Note:** Deletion triggers automatic cleanup of user data in compliance with GDPR while preserving audit logs for security purposes.

## 📊 Monitoring & Real-time Metrics

### Get Agent Metrics

```bash
GET /api/agents/{agentId}/metrics
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "user_id": "user-uuid",
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
    },
    "supabase": {
      "database_connections": 5,
      "real_time_subscriptions": 3,
      "rls_queries_per_minute": 120,
      "auth_requests_per_hour": 45
    }
  },
  "timestamp": "2024-01-15T13:25:00Z"
}
```

### Real-time Metrics with Supabase

Subscribe to real-time metrics updates:

```bash
# Establish real-time connection
POST https://your-supabase-url.supabase.co/realtime/v1/channels
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
  "topic": "realtime:public:agent_metrics:agent_id=eq.agent-123abc"
}
```

**JavaScript Example:**
```javascript
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(supabaseUrl, supabaseKey)

// Subscribe to real-time metrics
const subscription = supabase
  .channel('agent-metrics')
  .on('postgres_changes', {
    event: 'INSERT',
    schema: 'public',
    table: 'agent_metrics',
    filter: 'agent_id=eq.agent-123abc'
  }, (payload) => {
    console.log('New metrics:', payload.new)
  })
  .subscribe()
```

### Query Metrics from Supabase

```bash
GET https://your-supabase-url.supabase.co/rest/v1/agent_metrics?agent_id=eq.agent-123abc&order=timestamp.desc&limit=50
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

### Get Agent Logs

```bash
GET /api/agents/{agentId}/logs
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Query Parameters:**
- `lines` (optional): Number of log lines (default: 100)
- `since` (optional): Time since logs (e.g., "1h", "30m")
- `level` (optional): Filter by log level (INFO, WARN, ERROR)

**Response:**
```json
{
  "agentId": "agent-123abc",
  "user_id": "user-uuid",
  "logs": [
    {
      "id": "log-123",
      "timestamp": "2024-01-15T13:20:00Z",
      "level": "INFO",
      "message": "Agent started successfully",
      "source": "eliza-core",
      "metadata": {
        "pod": "agent-123abc-7d4f8c9b-xz9k2",
        "supabase_connection": "established"
      }
    },
    {
      "id": "log-124",
      "timestamp": "2024-01-15T13:21:00Z",
      "level": "INFO",
      "message": "Connected to Discord and Supabase",
      "source": "platform-integration",
      "metadata": {
        "discord_guild": "your-server-id",
        "supabase_project": "your-project-ref"
      }
    }
  ],
  "totalLines": 2,
  "truncated": false,
  "real_time_available": true
}
```

### Real-time Log Streaming with Supabase

Subscribe to real-time log updates:

```javascript
// Real-time log streaming
const logSubscription = supabase
  .channel('agent-logs')
  .on('postgres_changes', {
    event: 'INSERT',
    schema: 'public',
    table: 'agent_logs',
    filter: 'agent_id=eq.agent-123abc'
  }, (payload) => {
    console.log('New log:', payload.new)
  })
  .subscribe()
```

### Query Logs from Supabase

```bash
GET https://your-supabase-url.supabase.co/rest/v1/agent_logs?agent_id=eq.agent-123abc&order=timestamp.desc&limit=100
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

## 🔒 Security with Supabase

### Container Security Scan

```bash
POST /api/security/scan/{agentId}
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Response:**
```json
{
  "agentId": "agent-123abc",
  "user_id": "user-uuid",
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
    ],
    "supabase_compliance": {
      "rls_policies_verified": true,
      "data_encryption_enabled": true,
      "audit_logging_active": true
    }
  },
  "security_event_logged": true,
  "scannedAt": "2024-01-15T13:30:00Z"
}
```

### Supabase Security Features

#### Row-Level Security (RLS) Status

```bash
GET https://your-supabase-url.supabase.co/rest/v1/rpc/check_rls_policies
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Response:**
```json
{
  "tables": {
    "agents": {"rls_enabled": true, "policies_count": 3},
    "agent_metrics": {"rls_enabled": true, "policies_count": 2},
    "agent_logs": {"rls_enabled": true, "policies_count": 2},
    "agent_secrets": {"rls_enabled": true, "policies_count": 4}
  },
  "user_isolation": "ACTIVE",
  "compliance_status": "GDPR_COMPLIANT"
}
```

### Security Events

```bash
GET /api/security/events
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Query Parameters:**
- `limit` (optional): Number of events (default: 50)
- `severity` (optional): Filter by severity (LOW, MEDIUM, HIGH, CRITICAL)
- `event_type` (optional): Filter by event type
- `since` (optional): Events since timestamp

**Response:**
```json
{
  "events": [
    {
      "id": "event-456def",
      "event_type": "AUTH_FAILURE",
      "severity": "HIGH",
      "user_id": "user-uuid",
      "agent_id": "agent-123abc",
      "message": "Multiple authentication failures detected",
      "timestamp": "2024-01-15T13:25:00Z",
      "ip_address": "192.168.1.100",
      "user_agent": "Mozilla/5.0...",
      "details": {
        "failure_count": 5,
        "time_window": "5 minutes",
        "supabase_session_id": "session-789xyz"
      },
      "automated_response": {
        "action_taken": "TEMPORARY_LOCKOUT",
        "duration": "15 minutes"
      }
    }
  ],
  "total": 1,
  "user_id": "user-uuid"
}
```

### Real-time Security Monitoring

Subscribe to security events with Supabase real-time:

```javascript
// Real-time security event monitoring
const securitySubscription = supabase
  .channel('security-events')
  .on('postgres_changes', {
    event: 'INSERT',
    schema: 'public',
    table: 'security_events',
    filter: 'severity=eq.CRITICAL'
  }, (payload) => {
    console.log('Critical security event:', payload.new)
    // Handle critical security event
  })
  .subscribe()
```

### Query Security Events from Supabase

```bash
GET https://your-supabase-url.supabase.co/rest/v1/security_events?order=timestamp.desc&limit=50
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Note:** RLS automatically filters events to show only user-related security events.

## 🏥 Health & Status

### System Health

```bash
GET /api/health
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Response:**
```json
{
  "status": "HEALTHY",
  "timestamp": "2024-01-15T13:35:00Z",
  "version": "1.0.0",
  "user_id": "user-uuid",
  "components": {
    "supabase": {
      "status": "HEALTHY",
      "database": {
        "status": "HEALTHY",
        "responseTime": "2ms",
        "connections": 5,
        "rls_active": true
      },
      "auth": {
        "status": "HEALTHY",
        "active_sessions": 150,
        "jwt_valid": true
      },
      "realtime": {
        "status": "HEALTHY",
        "active_connections": 45,
        "subscriptions": 120
      },
      "storage": {
        "status": "HEALTHY",
        "buckets": 3,
        "total_size": "2.5GB"
      }
    },
    "kubernetes": {
      "status": "HEALTHY",
      "nodes": 3,
      "pods": 15,
      "user_agents": 1
    },
    "container_registry": {
      "status": "HEALTHY",
      "available": true,
      "last_scan": "2024-01-15T12:00:00Z"
    }
  },
  "user_permissions": [
    "agents:read",
    "agents:write",
    "monitoring:read"
  ]
}
```

### Supabase-specific Health Check

```bash
GET https://your-supabase-url.supabase.co/rest/v1/rpc/health_check
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Response:**
```json
{
  "supabase_health": {
    "database": "HEALTHY",
    "auth": "HEALTHY",
    "realtime": "HEALTHY",
    "storage": "HEALTHY",
    "edge_functions": "HEALTHY"
  },
  "user_specific": {
    "rls_policies": "ACTIVE",
    "user_data_accessible": true,
    "real_time_subscriptions": 3
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

## 👥 User Management with Supabase Auth

### Get User Profile

```bash
GET /api/users/profile
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

**Response:**
```json
{
  "user": {
    "id": "user-456def",
    "email": "admin@yourdomain.com",
    "created_at": "2024-01-01T00:00:00Z",
    "last_sign_in_at": "2024-01-15T10:00:00Z",
    "app_metadata": {
      "provider": "email",
      "providers": ["email", "google"]
    },
    "user_metadata": {
      "full_name": "Admin User",
      "avatar_url": "https://...",
      "permissions": [
        "agents:read",
        "agents:write",
        "monitoring:read"
      ]
    },
    "role": "authenticated"
  },
  "profile": {
    "username": "admin",
    "notification_preferences": {
      "email": true,
      "real_time": true,
      "security_alerts": true
    },
    "agent_count": 3,
    "storage_used": "150MB"
  }
}
```

### Update User Profile

```bash
PUT /api/users/profile
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
  "user_metadata": {
    "full_name": "Updated Name",
    "avatar_url": "https://new-avatar-url.com"
  },
  "notification_preferences": {
    "email": true,
    "real_time": false,
    "security_alerts": true
  }
}
```

### Supabase User Management

#### Get Current User

```bash
GET https://your-supabase-url.supabase.co/auth/v1/user
```

**Headers:**
```
Authorization: Bearer supabase-jwt-token
apikey: your-supabase-anon-key
```

#### Update User Metadata

```bash
PUT https://your-supabase-url.supabase.co/auth/v1/user
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
  "data": {
    "permissions": ["agents:read", "agents:write", "monitoring:read"],
    "notification_preferences": {
      "email": true,
      "real_time": true
    }
  }
}
```

### Change Password

```bash
PUT https://your-supabase-url.supabase.co/auth/v1/user
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
  "password": "new-secure-password"
}
```

## 🚨 Error Handling with Supabase

### Error Response Format

```json
{
  "error": {
    "code": "AGENT_NOT_FOUND",
    "message": "Agent with ID 'agent-123abc' not found or access denied",
    "timestamp": "2024-01-15T13:45:00Z",
    "requestId": "req-789xyz",
    "user_id": "user-uuid",
    "supabase_details": {
      "rls_applied": true,
      "user_isolation": "active"
    }
  }
}
```

### Common Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `INVALID_JWT_TOKEN` | 401 | Supabase JWT token is invalid or expired |
| `INSUFFICIENT_PERMISSIONS` | 403 | User lacks required permissions in Supabase |
| `AGENT_NOT_FOUND` | 404 | Agent does not exist or RLS blocks access |
| `RLS_ACCESS_DENIED` | 403 | Row-Level Security policy blocks access |
| `SUPABASE_CONNECTION_ERROR` | 503 | Cannot connect to Supabase backend |
| `VALIDATION_ERROR` | 400 | Request validation failed |
| `DEPLOYMENT_FAILED` | 500 | Agent deployment failed |
| `RESOURCE_LIMIT_EXCEEDED` | 429 | Resource quota exceeded |
| `SECURITY_SCAN_FAILED` | 422 | Security scan blocked deployment |
| `REAL_TIME_SUBSCRIPTION_FAILED` | 422 | Cannot establish real-time connection |

## 📋 Request/Response Examples

### Complete Agent Creation Flow with Supabase

```bash
# 1. Authenticate with Supabase
curl -X POST https://your-supabase-url.supabase.co/auth/v1/token?grant_type=password \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "your-password"
  }'

# Response
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "refresh_token_here",
  "user": {
    "id": "user-uuid",
    "email": "user@example.com"
  }
}

# 2. Create agent using Supabase JWT
curl -X POST https://api.yourdomain.com/api/agents \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "agentName": "discord-bot",
    "description": "Community Discord bot",
    "character": {
      "name": "DiscordBot",
      "bio": "Helpful community moderator",
      "personality": "helpful, friendly, professional"
    },
    "plugins": ["@elizaos/plugin-discord", "@elizaos/plugin-supabase"],
    "platforms": ["discord"],
    "resources": {
      "memory": "512Mi",
      "cpu": "250m"
    },
    "environment": {
      "DISCORD_TOKEN": "your-discord-token"
    },
    "supabase_config": {
      "project_ref": "your-project-ref",
      "real_time": {
        "enabled": true,
        "channels": ["agent_status", "discord_events"]
      }
    }
  }'

# Response
{
  "agentId": "agent-discord-001",
  "user_id": "user-uuid",
  "status": "QUEUED",
  "createdAt": "2024-01-15T14:00:00Z",
  "supabase_integration": {
    "tables_created": true,
    "rls_policies_applied": true,
    "real_time_enabled": true
  }
}

# 3. Check status (with Supabase auth)
curl -X GET https://api.yourdomain.com/api/agents/agent-discord-001/status \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "apikey: your-supabase-anon-key"

# Response
{
  "agentId": "agent-discord-001",
  "user_id": "user-uuid",
  "status": "DEPLOYED",
  "url": "https://agent-discord-001.agents.yourdomain.com",
  "health": {
    "overall": "HEALTHY",
    "supabase": {
      "connection": "HEALTHY",
      "real_time_active": true
    }
  }
}

# 4. Scale up with Supabase event logging
curl -X POST https://api.yourdomain.com/api/agents/agent-discord-001/scale \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "replicas": 2,
    "scaling_policy": {
      "min_replicas": 1,
      "max_replicas": 5
    }
  }'

# Response
{
  "agentId": "agent-discord-001",
  "user_id": "user-uuid",
  "currentReplicas": 2,
  "status": "SCALING",
  "scaling_event_logged": true,
  "supabase_event_id": "event-123abc"
}

# 5. Query agent directly from Supabase
curl -X GET https://your-supabase-url.supabase.co/rest/v1/agents?name=eq.discord-bot \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "apikey: your-supabase-anon-key"

# Response (RLS automatically filters to user's data)
[
  {
    "id": "agent-discord-001",
    "name": "discord-bot",
    "status": "deployed",
    "user_id": "user-uuid",
    "replicas": 2,
    "created_at": "2024-01-15T14:00:00Z",
    "updated_at": "2024-01-15T14:05:00Z"
  }
]
```

## 🔧 Rate Limiting with Supabase

API endpoints are rate-limited using both application-level and Supabase-level controls:

### Application Rate Limits
- **Default Limit**: 100 requests per 15 minutes per authenticated user
- **Burst Limit**: 20 requests per minute
- **Supabase Queries**: 1000 queries per minute per user

### Supabase Rate Limiting
- **Auth Requests**: 30 requests per hour per email
- **Real-time Connections**: 100 concurrent connections per user
- **Database Queries**: Rate limited by Supabase tier

### Rate Limit Headers

```bash
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1642252800
Supabase-RateLimit-Remaining: 950
Supabase-Auth-Rate-Limit: 25
```

### Supabase Rate Limit Response

```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Rate limit exceeded",
    "details": {
      "limit_type": "supabase_queries",
      "reset_time": "2024-01-15T14:00:00Z",
      "user_id": "user-uuid"
    }
  }
}
```

## 📖 SDKs and Libraries

### Node.js SDK with Supabase

```javascript
const { createClient } = require('@supabase/supabase-js');
const { AgentLaunchpadClient } = require('@agent-launchpad/sdk');

// Initialize Supabase client
const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_ANON_KEY
);

// Initialize Agent Launchpad client with Supabase auth
const client = new AgentLaunchpadClient({
  supabaseClient: supabase,
  baseUrl: 'https://api.yourdomain.com'
});

// Authenticate user
const { data, error } = await supabase.auth.signInWithPassword({
  email: 'user@example.com',
  password: 'password'
});

// Create agent (automatically uses Supabase JWT)
const agent = await client.agents.create({
  agentName: 'my-bot',
  description: 'My AI bot',
  supabase_config: {
    project_ref: 'your-project-ref',
    real_time: { enabled: true }
  }
});

// Get status with real-time updates
const status = await client.agents.getStatus(agent.agentId);

// Subscribe to real-time updates
client.subscribeToAgent(agent.agentId, (update) => {
  console.log('Agent update:', update);
});
```

### Python SDK with Supabase

```python
from supabase import create_client, Client
from agent_launchpad import AgentLaunchpadClient

# Initialize Supabase client
supabase: Client = create_client(
    supabase_url=os.environ.get("SUPABASE_URL"),
    supabase_key=os.environ.get("SUPABASE_ANON_KEY")
)

# Initialize Agent Launchpad client
client = AgentLaunchpadClient(
    supabase_client=supabase,
    base_url='https://api.yourdomain.com'
)

# Authenticate user
auth_response = supabase.auth.sign_in_with_password({
    "email": "user@example.com",
    "password": "password"
})

# Create agent
agent = client.agents.create(
    agent_name='my-bot',
    description='My AI bot',
    supabase_config={
        'project_ref': 'your-project-ref',
        'real_time': {'enabled': True}
    }
)

# Get status
status = client.agents.get_status(agent['agentId'])

# Subscribe to real-time updates
def handle_update(payload):
    print(f"Agent update: {payload}")

client.subscribe_to_agent(agent['agentId'], handle_update)
```

## 🚀 Next Steps

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 15px; margin: 20px 0;">

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #4CAF50;">
<h4><a href="getting-started" style="text-decoration: none; color: #4CAF50;">🚀 Getting Started</a></h4>
<p>Set up your first agent deployment with Supabase authentication</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #F44336;">
<h4><a href="security" style="text-decoration: none; color: #F44336;">🔐 Security Guide</a></h4>
<p>Learn about Supabase security, RLS, and authentication best practices</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #795548;">
<h4><a href="use-cases" style="text-decoration: none; color: #795548;">💡 Use Cases</a></h4>
<p>Explore real-world API usage examples with Supabase integration</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 15px; border-left: 4px solid #E91E63;">
<h4><a href="support" style="text-decoration: none; color: #E91E63;">📞 Support</a></h4>
<p>Get help with API integration and Supabase setup</p>
</div>

</div>

---

## 📋 Supabase Integration Summary

The Multi-Agent Infrastructure API now provides:

### 🔑 **Enterprise Authentication**
- **Multi-provider OAuth**: Google, GitHub, Discord, and more
- **Email/Password**: Traditional authentication with secure password policies
- **Magic Links**: Passwordless authentication for better UX
- **JWT Management**: Automatic token refresh and secure claims

### 🛡️ **Database Security**
- **Row-Level Security (RLS)**: Automatic user data isolation
- **Encrypted Storage**: AES-256 encryption for all data at rest
- **Audit Logging**: Comprehensive security event tracking
- **GDPR Compliance**: Automatic data retention and deletion policies

### ⚡ **Real-time Capabilities**
- **Live Updates**: Real-time agent status, metrics, and logs
- **WebSocket Events**: Bi-directional communication for instant updates
- **Presence Tracking**: See who's online and collaborating
- **Broadcast Messages**: System-wide notifications and alerts

### 📊 **Enhanced Monitoring**
- **Real-time Metrics**: CPU, memory, network, and Supabase-specific metrics
- **Live Log Streaming**: Stream logs directly from Supabase with filtering
- **Security Monitoring**: Real-time security event detection and response
- **Performance Analytics**: Database query performance and optimization insights

### 🚀 **Developer Experience**
- **Simplified SDKs**: Native Supabase integration in Node.js and Python
- **GraphQL Support**: Query your agent data with GraphQL through Supabase
- **Edge Functions**: Deploy serverless functions alongside your agents
- **Storage Integration**: Secure file storage for agent assets and backups

All API endpoints maintain backward compatibility while adding powerful new Supabase-powered features for enterprise-grade agent deployment and management. 