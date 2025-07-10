---
layout: default
title: "Multi-Agent Infrastructure at Scale - Home"
description: "Infrastructure backend for automated AI agent deployment and management"
---

<div align="center">

# 🚀 Multi-Agent Infrastructure at Scale

**Infrastructure Backend for Automated AI Agent Deployment**

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Documentation](https://img.shields.io/badge/docs-latest-brightgreen.svg)](https://yourusername.github.io/agent-launchpad-infra)

</div>

---

## 🌟 What is Multi-Agent Infrastructure at Scale?

The **Multi-Agent Infrastructure at Scale** is a robust infrastructure backend that automates the deployment and management of [Eliza-based AI agents](https://github.com/elizaos/eliza) through a streamlined interface with Docker and Kubernetes orchestration.

### ✨ Key Features

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; margin: 20px 0;">

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px;">
<h4>🤖 Automated Deployment</h4>
<p>Deploy Eliza agents with simple configuration - from code to production in minutes</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px;">
<h4>🔒 Supabase Security</h4>
<p>Enterprise-grade authentication with OAuth providers, RLS, and real-time updates</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px;">
<h4>📊 Real-time Monitoring</h4>
<p>Health checks, metrics, and alerting with Prometheus and Grafana</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px;">
<h4>⚡ Auto-scaling</h4>
<p>Dynamic resource management based on load and performance</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px;">
<h4>🛡️ Container Security</h4>
<p>Vulnerability scanning with Trivy blocks insecure deployments</p>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px;">
<h4>🔧 Simple Management</h4>
<p>Easy agent lifecycle operations through RESTful APIs</p>
</div>

</div>

---

## 🏗️ Architecture Overview

```mermaid
graph TB
    subgraph "Frontend"
        UI[Web Interface]
        RealTime[Real-time Subscriptions]
    end
    
    subgraph "Supabase Backend"
        SupaAuth[Supabase Auth]
        SupaDB[Supabase Database]
        SupaStorage[Supabase Storage]
        SupaRT[Supabase Real-time]
    end
    
    subgraph "Agent Infrastructure"
        Agent[Agent Service]
        Deploy[Deploy Service]
        Monitor[Monitor Service]
    end
    
    subgraph "Container Platform"
        K8s[Kubernetes]
        Registry[Container Registry]
        Metrics[Prometheus]
    end
    
    UI --> SupaAuth
    UI --> SupaRT
    RealTime --> SupaRT
    SupaAuth --> Agent
    Agent --> SupaDB
    Agent --> Deploy
    Deploy --> Monitor
    Monitor --> K8s
    Monitor --> SupaDB
    Deploy --> Registry
    SupaRT --> SupaDB
    Agent --> SupaStorage
    K8s --> Metrics
    
    style UI fill:#e1f5fe
    style SupaAuth fill:#4CAF50
    style SupaDB fill:#4CAF50
    style SupaRT fill:#4CAF50
    style Agent fill:#f3e5f5
    style K8s fill:#e8f5e8
```

<div align="center">
<a href="architecture" style="display: inline-block; padding: 10px 20px; background-color: #007bff; color: white; text-decoration: none; border-radius: 5px; margin: 10px;">
📋 Detailed Architecture →
</a>
</div>

---

## 🚀 Create Your First AI Agent

Ready to deploy intelligent agents at scale? Create and deploy AI agents in minutes with secure JWT authentication and simple configuration:

### 🔐 Supabase Authentication

The platform uses **Supabase Auth** for enterprise-grade authentication with the following benefits:

- **🛡️ Secure**: Industry-standard JWT tokens with automatic key rotation
- **🔑 Multi-provider**: Email/password, OAuth (Google, GitHub, Discord), and magic links
- **⏰ Auto-expiring**: Configurable token expiration with automatic refresh
- **🚀 Real-time**: Database-backed user management with real-time updates
- **🏢 Enterprise**: Row-level security (RLS) and advanced user management

```bash
# Email/Password Authentication
curl -X POST https://your-supabase-url.supabase.co/auth/v1/token?grant_type=password \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "your-password"
  }'

# OAuth Authentication (Google, GitHub, etc.)
curl -X POST https://your-supabase-url.supabase.co/auth/v1/authorize \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "redirect_to": "https://your-domain.com/auth/callback"
  }'

# Magic Link Authentication
curl -X POST https://your-supabase-url.supabase.co/auth/v1/magiclink \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com"
  }'

# Response includes Supabase JWT token
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

### 🤖 Discord Trading Bot

```bash
# Authenticate with Supabase
curl -X POST https://your-supabase-url.supabase.co/auth/v1/token?grant_type=password \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "trader@example.com",
    "password": "your-password"
  }'

# Create agent using Supabase JWT token
curl -X POST https://your-domain.com/api/agents \
  -H "Authorization: Bearer supabase-jwt-token" \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "agentName": "crypto-trading-bot",
    "description": "AI-powered cryptocurrency trading assistant",
    "character": {
      "name": "TradingBot",
      "bio": "Expert cryptocurrency trader with real-time market analysis",
      "personality": "Professional, analytical, and helpful"
    },
    "plugins": [
      "@elizaos/plugin-discord",
      "@elizaos/plugin-web3",
      "@elizaos/plugin-coingecko"
    ],
    "platforms": ["discord"],
    "resources": {
      "memory": "1Gi",
      "cpu": "500m"
    },
    "environment": {
      "DISCORD_TOKEN": "your-discord-token",
      "COINGECKO_API_KEY": "your-api-key"
    },
    "supabase_config": {
      "project_ref": "your-project-ref",
      "database_url": "postgresql://postgres:[password]@db.your-project.supabase.co:5432/postgres"
    }
  }'
```

### 💬 Telegram Support Agent

```bash
curl -X POST https://your-domain.com/api/agents \
  -H "Authorization: Bearer supabase-jwt-token" \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "agentName": "support-assistant",
    "description": "24/7 customer support agent",
    "character": {
      "name": "SupportBot",
      "bio": "Friendly customer support specialist",
      "personality": "Helpful, patient, and knowledgeable"
    },
    "plugins": [
      "@elizaos/plugin-telegram",
      "@elizaos/plugin-knowledge-base",
      "@elizaos/plugin-supabase"
    ],
    "platforms": ["telegram"],
    "resources": {
      "memory": "512Mi",
      "cpu": "250m"
    },
    "environment": {
      "TELEGRAM_BOT_TOKEN": "your-telegram-token"
    },
    "supabase_config": {
      "project_ref": "your-project-ref",
      "tables": {
        "conversations": "support_conversations",
        "knowledge_base": "support_articles",
        "analytics": "support_analytics"
      }
    }
  }'
```

### 🌐 Multi-Platform Agent

```bash
curl -X POST https://your-domain.com/api/agents \
  -H "Authorization: Bearer supabase-jwt-token" \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "agentName": "omni-agent",
    "description": "Multi-platform AI agent with Web3 capabilities",
    "character": {
      "name": "OmniBot",
      "bio": "Versatile AI assistant across multiple platforms",
      "personality": "Adaptable, intelligent, and engaging"
    },
    "plugins": [
      "@elizaos/plugin-discord",
      "@elizaos/plugin-telegram",
      "@elizaos/plugin-twitter",
      "@elizaos/plugin-web3",
      "@elizaos/plugin-supabase"
    ],
    "platforms": ["discord", "telegram", "twitter"],
    "resources": {
      "memory": "2Gi",
      "cpu": "1000m"
    },
    "scaling": {
      "minReplicas": 1,
      "maxReplicas": 5,
      "targetCPU": 70
    },
    "supabase_config": {
      "project_ref": "your-project-ref",
      "real_time": {
        "enabled": true,
        "channels": ["agent_status", "user_interactions", "performance_metrics"]
      },
      "storage": {
        "bucket": "agent-assets",
        "public_access": false
      }
    }
  }'
```

### 📊 Agent Status & Management

#### REST API - Standard Operations

```bash
# Check agent status (using Supabase Auth)
curl -X GET https://your-domain.com/api/agents/crypto-trading-bot \
  -H "Authorization: Bearer supabase-jwt-token" \
  -H "apikey: your-supabase-anon-key"

# Scale agent
curl -X PATCH https://your-domain.com/api/agents/crypto-trading-bot \
  -H "Authorization: Bearer supabase-jwt-token" \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "resources": {
      "memory": "2Gi",
      "cpu": "1000m"
    },
    "scaling": {
      "minReplicas": 2,
      "maxReplicas": 10
    }
  }'

# View agent logs
curl -X GET https://your-domain.com/api/agents/crypto-trading-bot/logs \
  -H "Authorization: Bearer supabase-jwt-token" \
  -H "apikey: your-supabase-anon-key"

# Refresh Supabase JWT token
curl -X POST https://your-supabase-url.supabase.co/auth/v1/token?grant_type=refresh_token \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "refresh_token": "your-refresh-token"
  }'

# Query agent data directly from Supabase
curl -X GET https://your-supabase-url.supabase.co/rest/v1/agents?name=eq.crypto-trading-bot \
  -H "Authorization: Bearer supabase-jwt-token" \
  -H "apikey: your-supabase-anon-key"

# Update agent metadata in Supabase
curl -X PATCH https://your-supabase-url.supabase.co/rest/v1/agents?id=eq.agent-uuid \
  -H "Authorization: Bearer supabase-jwt-token" \
  -H "apikey: your-supabase-anon-key" \
  -H "Content-Type: application/json" \
  -d '{
    "status": "running",
    "last_activity": "2024-01-15T10:30:00Z",
    "performance_metrics": {
      "cpu_usage": 45.2,
      "memory_usage": 1024,
      "requests_per_minute": 150
    }
  }'
```

#### Real-time Updates - Supabase Integration

```javascript
import { createClient } from '@supabase/supabase-js'

// Initialize Supabase client
const supabaseUrl = 'https://your-project.supabase.co'
const supabaseKey = 'your-supabase-anon-key'
const supabase = createClient(supabaseUrl, supabaseKey)

// Supabase Authentication Helper
class SupabaseAuthManager {
  constructor() {
    this.supabase = supabase;
    this.session = null;
    this.setupAuthListener();
  }

  setupAuthListener() {
    this.supabase.auth.onAuthStateChange((event, session) => {
      this.session = session;
      
      switch (event) {
        case 'SIGNED_IN':
          console.log('User signed in:', session.user);
          this.setupRealTimeSubscriptions();
          break;
        case 'SIGNED_OUT':
          console.log('User signed out');
          this.cleanupSubscriptions();
          break;
        case 'TOKEN_REFRESHED':
          console.log('Token refreshed');
          break;
      }
    });
  }

  async signIn(email, password) {
    const { data, error } = await this.supabase.auth.signInWithPassword({
      email,
      password
    });
    return { data, error };
  }

  async signInWithOAuth(provider) {
    const { data, error } = await this.supabase.auth.signInWithOAuth({
      provider,
      options: {
        redirectTo: 'https://your-domain.com/auth/callback'
      }
    });
    return { data, error };
  }

  getAccessToken() {
    return this.session?.access_token;
  }

  setupRealTimeSubscriptions() {
    // Real-time agent status updates
    this.agentStatusSubscription = this.supabase
      .channel('agent-status')
      .on('postgres_changes', {
        event: '*',
        schema: 'public',
        table: 'agents',
        filter: `user_id=eq.${this.session.user.id}`
      }, (payload) => {
        console.log('Agent status update:', payload);
        this.handleAgentStatusUpdate(payload);
      })
      .subscribe();

    // Real-time performance metrics
    this.metricsSubscription = this.supabase
      .channel('performance-metrics')
      .on('postgres_changes', {
        event: 'INSERT',
        schema: 'public',
        table: 'agent_metrics'
      }, (payload) => {
        console.log('Performance metrics:', payload);
        this.handleMetricsUpdate(payload);
      })
      .subscribe();

    // Real-time log streaming
    this.logsSubscription = this.supabase
      .channel('agent-logs')
      .on('postgres_changes', {
        event: 'INSERT',
        schema: 'public',
        table: 'agent_logs',
        filter: `user_id=eq.${this.session.user.id}`
      }, (payload) => {
        console.log('New log entry:', payload);
        this.handleLogUpdate(payload);
      })
      .subscribe();

    // Real-time system alerts
    this.alertsSubscription = this.supabase
      .channel('system-alerts')
      .on('postgres_changes', {
        event: 'INSERT',
        schema: 'public',
        table: 'system_alerts'
      }, (payload) => {
        console.log('System alert:', payload);
        this.handleSystemAlert(payload);
      })
      .subscribe();
  }

  handleAgentStatusUpdate(payload) {
    const { new: newData, old: oldData, eventType } = payload;
    
    switch (eventType) {
      case 'INSERT':
        console.log('New agent created:', newData);
        addAgentToUI(newData);
        break;
      case 'UPDATE':
        console.log('Agent updated:', newData);
        updateAgentInUI(newData);
        break;
      case 'DELETE':
        console.log('Agent deleted:', oldData);
        removeAgentFromUI(oldData);
        break;
    }
  }

  handleMetricsUpdate(payload) {
    const metricsData = payload.new;
    updateMetricsChart({
      agentId: metricsData.agent_id,
      timestamp: metricsData.timestamp,
      cpu: metricsData.cpu_usage,
      memory: metricsData.memory_usage,
      requests: metricsData.requests_per_minute
    });
  }

  handleLogUpdate(payload) {
    const logData = payload.new;
    appendLogToUI({
      timestamp: logData.timestamp,
      level: logData.level,
      message: logData.message,
      source: logData.source,
      agentId: logData.agent_id
    });
  }

  handleSystemAlert(payload) {
    const alertData = payload.new;
    showRealTimeAlert({
      id: alertData.id,
      type: alertData.alert_type,
      severity: alertData.severity,
      message: alertData.message,
      timestamp: alertData.created_at
    });
  }

  cleanupSubscriptions() {
    this.supabase.removeAllChannels();
  }
}

// Initialize authentication manager
const authManager = new SupabaseAuthManager();

// Example usage with agent operations
async function createAgentWithSupabase(agentConfig) {
  const token = authManager.getAccessToken();
  
  if (!token) {
    throw new Error('User not authenticated');
  }

  // Create agent via API
  const response = await fetch('/api/agents', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${token}`,
      'apikey': supabaseKey,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(agentConfig)
  });

  // Agent status will be automatically updated via real-time subscription
  return response.json();
}

// Real-time presence for collaborative features
const presenceChannel = supabase.channel('online-users')
  .on('presence', { event: 'sync' }, () => {
    const newState = presenceChannel.presenceState();
    console.log('Online users:', newState);
  })
  .on('presence', { event: 'join' }, ({ key, newPresences }) => {
    console.log('User joined:', key, newPresences);
  })
  .on('presence', { event: 'leave' }, ({ key, leftPresences }) => {
    console.log('User left:', key, leftPresences);
  })
  .subscribe(async (status) => {
    if (status === 'SUBSCRIBED') {
      await presenceChannel.track({
        user_id: authManager.session?.user.id,
        online_at: new Date().toISOString(),
      });
    }
  });
```

#### React Component with Supabase

```jsx
import React, { useState, useEffect, useCallback } from 'react';
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(
  process.env.REACT_APP_SUPABASE_URL,
  process.env.REACT_APP_SUPABASE_ANON_KEY
);

const AgentMonitoringDashboard = ({ agentName }) => {
  const [agentStatus, setAgentStatus] = useState({});
  const [metrics, setMetrics] = useState([]);
  const [logs, setLogs] = useState([]);
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [subscriptions, setSubscriptions] = useState([]);

  // Authentication management
  useEffect(() => {
    // Get initial session
    supabase.auth.getSession().then(({ data: { session } }) => {
      setUser(session?.user ?? null);
      setLoading(false);
    });

    // Listen for auth changes
    const { data: { subscription } } = supabase.auth.onAuthStateChange(
      (event, session) => {
        setUser(session?.user ?? null);
        setLoading(false);
      }
    );

    return () => subscription.unsubscribe();
  }, []);

  // Real-time subscriptions
  useEffect(() => {
    if (!user || !agentName) return;

    const setupSubscriptions = async () => {
      // Subscribe to agent status changes
      const agentStatusSub = supabase
        .channel(`agent-status-${agentName}`)
        .on('postgres_changes', {
          event: '*',
          schema: 'public',
          table: 'agents',
          filter: `name=eq.${agentName} AND user_id=eq.${user.id}`
        }, (payload) => {
          console.log('Agent status update:', payload);
          if (payload.new) {
            setAgentStatus(payload.new);
          }
        })
        .subscribe();

      // Subscribe to performance metrics
      const metricsSub = supabase
        .channel(`metrics-${agentName}`)
        .on('postgres_changes', {
          event: 'INSERT',
          schema: 'public',
          table: 'agent_metrics',
          filter: `agent_name=eq.${agentName}`
        }, (payload) => {
          console.log('New metrics:', payload);
          setMetrics(prev => [...prev.slice(-49), payload.new]);
        })
        .subscribe();

      // Subscribe to logs
      const logsSub = supabase
        .channel(`logs-${agentName}`)
        .on('postgres_changes', {
          event: 'INSERT',
          schema: 'public',
          table: 'agent_logs',
          filter: `agent_name=eq.${agentName}`
        }, (payload) => {
          console.log('New log:', payload);
          setLogs(prev => [...prev.slice(-99), payload.new]);
        })
        .subscribe();

      setSubscriptions([agentStatusSub, metricsSub, logsSub]);
    };

    setupSubscriptions();

    // Cleanup subscriptions on unmount
    return () => {
      subscriptions.forEach(sub => {
        supabase.removeChannel(sub);
      });
    };
  }, [user, agentName]);

  // Initial data fetch
  useEffect(() => {
    if (!user || !agentName) return;

    const fetchInitialData = async () => {
      try {
        // Fetch current agent status
        const { data: agentData, error: agentError } = await supabase
          .from('agents')
          .select('*')
          .eq('name', agentName)
          .eq('user_id', user.id)
          .single();

        if (agentError) throw agentError;
        setAgentStatus(agentData);

        // Fetch recent metrics
        const { data: metricsData, error: metricsError } = await supabase
          .from('agent_metrics')
          .select('*')
          .eq('agent_name', agentName)
          .order('timestamp', { ascending: false })
          .limit(50);

        if (metricsError) throw metricsError;
        setMetrics(metricsData.reverse());

        // Fetch recent logs
        const { data: logsData, error: logsError } = await supabase
          .from('agent_logs')
          .select('*')
          .eq('agent_name', agentName)
          .order('timestamp', { ascending: false })
          .limit(100);

        if (logsError) throw logsError;
        setLogs(logsData.reverse());

      } catch (error) {
        console.error('Error fetching initial data:', error);
      }
    };

    fetchInitialData();
  }, [user, agentName]);

  // Agent operations
  const scaleAgent = async (replicas) => {
    try {
      const { data, error } = await supabase
        .from('agents')
        .update({ 
          replicas: replicas,
          updated_at: new Date().toISOString()
        })
        .eq('name', agentName)
        .eq('user_id', user.id);

      if (error) throw error;
      console.log('Agent scaled successfully');
    } catch (error) {
      console.error('Error scaling agent:', error);
    }
  };

  const pauseAgent = async () => {
    try {
      const { data, error } = await supabase
        .from('agents')
        .update({ 
          status: 'paused',
          updated_at: new Date().toISOString()
        })
        .eq('name', agentName)
        .eq('user_id', user.id);

      if (error) throw error;
      console.log('Agent paused successfully');
    } catch (error) {
      console.error('Error pausing agent:', error);
    }
  };

  const getLatestMetrics = () => {
    return metrics.length > 0 ? metrics[metrics.length - 1] : {};
  };

  if (loading) {
    return <div className="loading">Loading...</div>;
  }

  if (!user) {
    return (
      <div className="auth-required">
        <h3>Authentication Required</h3>
        <button onClick={() => supabase.auth.signInWithOAuth({ provider: 'google' })}>
          Sign in with Google
        </button>
      </div>
    );
  }

  const latestMetrics = getLatestMetrics();

  return (
    <div className="agent-dashboard">
      <div className="status-indicator">
        <span className={`status-badge ${agentStatus.status}`}>
          🟢 {agentStatus.status} - Supabase Connected
        </span>
        <span className="user-info">
          User: {user.email}
        </span>
      </div>
      
      <div className="agent-info">
        <h3>{agentName}</h3>
        <p>Status: {agentStatus.status}</p>
        <p>CPU: {latestMetrics.cpu_usage}%</p>
        <p>Memory: {latestMetrics.memory_usage}MB</p>
        <p>Replicas: {agentStatus.replicas}</p>
        <p>Last Updated: {new Date(agentStatus.updated_at).toLocaleString()}</p>
      </div>

      <div className="agent-controls">
        <button onClick={() => scaleAgent(agentStatus.replicas + 1)}>
          Scale Up
        </button>
        <button onClick={() => scaleAgent(Math.max(1, agentStatus.replicas - 1))}>
          Scale Down
        </button>
        <button onClick={pauseAgent}>
          Pause Agent
        </button>
      </div>
      
      <div className="metrics-chart">
        <h4>Performance Metrics (Real-time)</h4>
        <div className="metrics-grid">
          {metrics.slice(-10).map((metric, index) => (
            <div key={index} className="metric-point">
              <span className="time">{new Date(metric.timestamp).toLocaleTimeString()}</span>
              <span className="value">CPU: {metric.cpu_usage}%</span>
              <span className="value">Memory: {metric.memory_usage}MB</span>
            </div>
          ))}
        </div>
      </div>
      
      <div className="log-stream">
        <h4>Live Logs (Supabase Real-time)</h4>
        <div className="log-container">
          {logs.slice(-20).map((log, index) => (
            <div key={index} className={`log-entry ${log.level}`}>
              <span className="timestamp">{new Date(log.timestamp).toLocaleString()}</span>
              <span className="level">[{log.level}]</span>
              <span className="message">{log.message}</span>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};

export default AgentMonitoringDashboard;
```

#### Supabase Database Schema & Events

```sql
-- Core database tables for Supabase integration

-- Users table (managed by Supabase Auth)
CREATE TABLE auth.users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR NOT NULL UNIQUE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Agents table
CREATE TABLE public.agents (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  name VARCHAR NOT NULL,
  description TEXT,
  status VARCHAR DEFAULT 'pending',
  replicas INTEGER DEFAULT 1,
  character JSONB,
  plugins TEXT[],
  platforms TEXT[],
  resources JSONB,
  environment JSONB,
  supabase_config JSONB,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  
  UNIQUE(user_id, name)
);

-- Agent metrics table for performance tracking
CREATE TABLE public.agent_metrics (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  agent_id UUID REFERENCES public.agents(id) ON DELETE CASCADE,
  agent_name VARCHAR NOT NULL,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  timestamp TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  cpu_usage DECIMAL(5,2),
  memory_usage INTEGER,
  requests_per_minute INTEGER,
  response_time_ms DECIMAL(8,2),
  error_count INTEGER DEFAULT 0
);

-- Agent logs table for real-time log streaming
CREATE TABLE public.agent_logs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  agent_id UUID REFERENCES public.agents(id) ON DELETE CASCADE,
  agent_name VARCHAR NOT NULL,
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  timestamp TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  level VARCHAR NOT NULL,
  message TEXT NOT NULL,
  source VARCHAR,
  metadata JSONB
);

-- System alerts table
CREATE TABLE public.system_alerts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  alert_type VARCHAR NOT NULL,
  severity VARCHAR NOT NULL,
  message TEXT NOT NULL,
  affected_agent_id UUID REFERENCES public.agents(id),
  resolved BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  resolved_at TIMESTAMP WITH TIME ZONE
);

-- Row Level Security (RLS) policies
ALTER TABLE public.agents ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.agent_metrics ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.agent_logs ENABLE ROW LEVEL SECURITY;

-- RLS policies for user data isolation
CREATE POLICY "Users can only see their own agents" ON public.agents
  FOR ALL USING (auth.uid() = user_id);

CREATE POLICY "Users can only see their own metrics" ON public.agent_metrics
  FOR ALL USING (auth.uid() = user_id);

CREATE POLICY "Users can only see their own logs" ON public.agent_logs
  FOR ALL USING (auth.uid() = user_id);

-- Real-time subscriptions setup
ALTER PUBLICATION supabase_realtime ADD TABLE public.agents;
ALTER PUBLICATION supabase_realtime ADD TABLE public.agent_metrics;
ALTER PUBLICATION supabase_realtime ADD TABLE public.agent_logs;
ALTER PUBLICATION supabase_realtime ADD TABLE public.system_alerts;
```

```javascript
// Supabase Real-time Events
const supabaseEvents = {
  // Postgres changes events
  'postgres_changes': {
    'INSERT': 'New record inserted',
    'UPDATE': 'Record updated',
    'DELETE': 'Record deleted'
  },
  
  // Presence events for collaborative features
  'presence': {
    'sync': 'Presence state synchronized',
    'join': 'User joined',
    'leave': 'User left'
  },
  
  // Custom broadcast events
  'broadcast': {
    'agent_command': 'Real-time agent commands',
    'deployment_update': 'Deployment status updates',
    'system_notification': 'System-wide notifications'
  }
};

// Supabase JWT Token Structure
const supabaseJWTPayload = {
  'aud': 'authenticated',               // Audience
  'exp': 1234571490,                   // Expiration timestamp
  'iat': 1234567890,                   // Issued at timestamp
  'iss': 'https://your-project.supabase.co/auth/v1', // Issuer
  'sub': 'user-uuid',                  // Subject (user ID)
  'email': 'user@example.com',         // User email
  'phone': '',                         // User phone
  'app_metadata': {                    // Application metadata
    'provider': 'email',
    'providers': ['email']
  },
  'user_metadata': {                   // User metadata
    'permissions': [
      'agents:read',
      'agents:write',
      'monitoring:read',
      'logs:read'
    ]
  },
  'role': 'authenticated',             // User role
  'aal': 'aal1',                       // Authentication Assurance Level
  'amr': [{ 'method': 'password', 'timestamp': 1234567890 }], // Auth methods
  'session_id': 'session-uuid'         // Session identifier
};

// Environment variables for Supabase
const supabaseConfig = {
  'SUPABASE_URL': 'https://your-project.supabase.co',
  'SUPABASE_ANON_KEY': 'your-anon-key',
  'SUPABASE_SERVICE_ROLE_KEY': 'your-service-role-key',
  'DATABASE_URL': 'postgresql://postgres:[password]@db.your-project.supabase.co:5432/postgres'
};
```

<div align="center">
<a href="getting-started" style="display: inline-block; padding: 10px 20px; background-color: #28a745; color: white; text-decoration: none; border-radius: 5px; margin: 10px;">
🚀 Complete Setup Guide →
</a>
</div>

---

## 📚 Documentation Hub

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 20px; margin: 30px 0;">

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px; border-left: 4px solid #2196F3;">
<h4><a href="architecture" style="text-decoration: none; color: #2196F3;">🏗️ System Architecture</a></h4>
<p>Detailed system architecture with flow charts and component documentation</p>
<small>Components • Flow Charts • Infrastructure</small>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px; border-left: 4px solid #4CAF50;">
<h4><a href="getting-started" style="text-decoration: none; color: #4CAF50;">🚀 Getting Started</a></h4>
<p>Step-by-step guide to set up and deploy your first agent</p>
<small>Installation • Configuration • First Deployment</small>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px; border-left: 4px solid #FF9800;">
<h4><a href="api-reference" style="text-decoration: none; color: #FF9800;">📡 API Reference</a></h4>
<p>Complete REST API documentation with examples</p>
<small>Endpoints • Authentication • Examples</small>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px; border-left: 4px solid #F44336;">
<h4><a href="security" style="text-decoration: none; color: #F44336;">🔐 Security Guide</a></h4>
<p>Security architecture, best practices, and compliance</p>
<small>Authentication • Encryption • Monitoring</small>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px; border-left: 4px solid #9C27B0;">
<h4><a href="deployment" style="text-decoration: none; color: #9C27B0;">⚙️ Deployment Guide</a></h4>
<p>Production deployment strategies and operations</p>
<small>Kubernetes • Docker • CI/CD</small>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px; border-left: 4px solid #607D8B;">
<h4><a href="monitoring" style="text-decoration: none; color: #607D8B;">📊 Monitoring</a></h4>
<p>Observability, metrics, and alerting configuration</p>
<small>Prometheus • Grafana • Alerts</small>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px; border-left: 4px solid #795548;">
<h4><a href="use-cases" style="text-decoration: none; color: #795548;">💡 Use Cases</a></h4>
<p>Real-world examples and implementation patterns</p>
<small>Examples • Templates • Best Practices</small>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px; border-left: 4px solid #009688;">
<h4><a href="contributing" style="text-decoration: none; color: #009688;">🤝 Contributing</a></h4>
<p>How to contribute to the project</p>
<small>Guidelines • Development • Pull Requests</small>
</div>

<div style="border: 1px solid #e0e0e0; border-radius: 8px; padding: 20px; border-left: 4px solid #E91E63;">
<h4><a href="support" style="text-decoration: none; color: #E91E63;">📞 Support</a></h4>
<p>Get help and troubleshooting</p>
<small>Community • Issues • Documentation</small>
</div>

</div>

---

## 🎯 Popular Use Cases

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; margin: 20px 0;">

<div style="text-align: center; padding: 20px; border: 1px solid #e0e0e0; border-radius: 8px;">
<h3>🤖 Discord Trading Bots</h3>
<p>Deploy AI agents that provide real-time market analysis and trading signals to Discord communities.</p>
<a href="use-cases#discord-trading" style="color: #007bff; text-decoration: none;">Learn more →</a>
</div>

<div style="text-align: center; padding: 20px; border: 1px solid #e0e0e0; border-radius: 8px;">
<h3>💬 Telegram Support</h3>
<p>Create customer support bots that can handle inquiries and provide automated assistance.</p>
<a href="use-cases#telegram-support" style="color: #007bff; text-decoration: none;">Learn more →</a>
</div>

<div style="text-align: center; padding: 20px; border: 1px solid #e0e0e0; border-radius: 8px;">
<h3>🌐 Web3 Integration</h3>
<p>Deploy agents that can interact with blockchain networks and provide DeFi services.</p>
<a href="use-cases#web3-integration" style="color: #007bff; text-decoration: none;">Learn more →</a>
</div>

<div style="text-align: center; padding: 20px; border: 1px solid #e0e0e0; border-radius: 8px;">
<h3>📊 Analytics & Reporting</h3>
<p>Set up agents that generate automated reports and data analysis for your organization.</p>
<a href="use-cases#analytics" style="color: #007bff; text-decoration: none;">Learn more →</a>
</div>

</div>

---

## 🛣️ Roadmap

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">

<div style="padding: 15px; border: 1px solid #27ae60; border-radius: 8px; background-color: #d4edda;">
<h4 style="color: #155724; margin-top: 0;">✅ Completed</h4>
<ul style="margin-bottom: 0;">
<li>Basic Agent Deployment</li>
<li>Container Security</li>
<li>API Authentication</li>
<li>Health Monitoring</li>
</ul>
</div>

<div style="padding: 15px; border: 1px solid #ffc107; border-radius: 8px; background-color: #fff3cd;">
<h4 style="color: #856404; margin-top: 0;">🔄 In Progress</h4>
<ul style="margin-bottom: 0;">
<li>Multi-cloud Support</li>
<li>Advanced Scaling</li>
<li>Plugin Marketplace</li>
</ul>
</div>

<div style="padding: 15px; border: 1px solid #007bff; border-radius: 8px; background-color: #e7f3ff;">
<h4 style="color: #004085; margin-top: 0;">🔮 Planned</h4>
<ul style="margin-bottom: 0;">
<li>Visual Builder</li>
<li>Advanced Analytics</li>
<li>Enterprise Features</li>
</ul>
</div>

</div>

---

## 🚀 Ready to Get Started?

<div style="text-align: center; padding: 30px; background-color: #f8f9fa; border-radius: 10px; margin: 20px 0;">

<h3>Deploy Your First AI Agent Today!</h3>

<div style="display: flex; justify-content: center; gap: 15px; margin-top: 20px; flex-wrap: wrap;">

<a href="getting-started" style="display: inline-block; padding: 12px 24px; background-color: #28a745; color: white; text-decoration: none; border-radius: 6px; font-weight: bold;">
🚀 Get Started
</a>

<a href="architecture" style="display: inline-block; padding: 12px 24px; background-color: #007bff; color: white; text-decoration: none; border-radius: 6px; font-weight: bold;">
🏗️ View Architecture
</a>

<a href="api-reference" style="display: inline-block; padding: 12px 24px; background-color: #6c757d; color: white; text-decoration: none; border-radius: 6px; font-weight: bold;">
📡 API Docs
</a>

<a href="support" style="display: inline-block; padding: 12px 24px; background-color: #ffc107; color: #212529; text-decoration: none; border-radius: 6px; font-weight: bold;">
💬 Get Support
</a>

</div>

</div>

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Made with ❤️ by the Multi-Agent Infrastructure at Scale Team 