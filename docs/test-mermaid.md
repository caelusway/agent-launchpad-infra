---
layout: default
title: "Mermaid Test Page"
---

# 🧪 Mermaid Test Page

This page is for testing Mermaid diagram rendering.

## Test 1: Simple Flowchart

```mermaid
flowchart TD
    A[Start] --> B{Is it working?}
    B -->|Yes| C[Great!]
    B -->|No| D[Check console]
    C --> E[End]
    D --> E[End]
    
    style A fill:#e8f5e8
    style C fill:#4caf50
    style D fill:#f44336
```

## Test 2: Sequence Diagram

```mermaid
sequenceDiagram
    participant A as User
    participant B as System
    
    A->>B: Request
    B->>B: Process
    B->>A: Response
    
    Note over A,B: This is a test diagram
```

## Test 3: Simple Graph

```mermaid
graph LR
    A[Input] --> B[Process]
    B --> C[Output]
    
    style A fill:#e1f5fe
    style B fill:#fff3e0
    style C fill:#e8f5e8
```

## Manual Mermaid Test

<div class="mermaid">
graph TD
    Start([Manual Test]) --> Process[Processing]
    Process --> End([Complete])
    
    style Start fill:#e8f5e8
    style End fill:#4caf50
</div>

## Debug Information

Open your browser's developer console (F12) to see debug information about Mermaid diagram detection and rendering.

If you see diagrams above, Mermaid is working correctly! If not, check the console for error messages. 