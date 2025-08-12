# System Architecture Diagram

## Overview

This document provides a comprehensive view of the KeenOn Card Orchestration system architecture, including service interactions, data flow, and deployment infrastructure.

## System Components

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        KeenOn Card Orchestration                        │
└─────────────────────────────────────────────────────────────────────────┘
                                     │
                                     ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                              Load Balancer                              │
│                             (Nginx Proxy)                               │
└─────────────────────────────────────────────────────────────────────────┘
                                     │
                 ┌───────────────────┼───────────────────┐
                 │                   │                   │
                 ▼                   ▼                   ▼
┌───────────────────────┐ ┌───────────────────┐ ┌───────────────────────┐
│  Frontend Application │ │    Central API    │ │      API Gateway      │
│      (Next.js)        │ │     (Node.js)     │ │       (Nginx)         │
└───────────────────────┘ └───────────────────┘ └───────────────────────┘
                                     │
                 ┌───────────────────┼───────────────────┐
                 │                   │                   │
                 ▼                   ▼                   ▼
┌───────────────────────┐ ┌───────────────────┐ ┌───────────────────────┐
│  Translation Service  │ │ Image Composition │ │   Card Generation     │
│      (Python)         │ │  Service (Rust)   │ │   Service (Node.js)   │
└───────────────────────┘ └───────────────────┘ └───────────────────────┘
                 │                   │                   │
                 └───────────────────┼───────────────────┘
                                     │
                 ┌───────────────────┼───────────────────┐
                 │                   │                   │
                 ▼                   ▼                   ▼
┌───────────────────────┐ ┌───────────────────┐ ┌───────────────────────┐
│    PostgreSQL DB      │ │    Redis Cache    │ │   Object Storage      │
│                       │ │                   │ │      (Images)         │
└───────────────────────┘ └───────────────────┘ └───────────────────────┘
```

## Service Interactions

### Frontend to Backend Communication

The Next.js frontend application communicates with the backend services through the API Gateway:

```
┌───────────────────┐     HTTP/REST     ┌───────────────────┐
│ Frontend (Next.js)│ ─────────────────►│   API Gateway     │
└───────────────────┘                   └───────────────────┘
                                                │
                                                ▼
                                        ┌───────────────────┐
                                        │   Central API     │
                                        └───────────────────┘
```

### Microservice Communication

The Central API orchestrates the microservices using gRPC for efficient communication:

```
┌───────────────────┐     gRPC      ┌───────────────────┐
│   Central API     │ ◄───────────► │ Translation Service│
└───────────────────┘               └───────────────────┘

┌───────────────────┐     gRPC      ┌───────────────────┐
│   Central API     │ ◄───────────► │ Image Composition  │
└───────────────────┘               └───────────────────┘

┌───────────────────┐     gRPC      ┌───────────────────┐
│   Central API     │ ◄───────────► │ Card Generation    │
└───────────────────┘               └───────────────────┘
```

## Data Flow

### Card Creation Process

1. User submits a request to create a new card through the frontend
2. Request is routed through the API Gateway to the Central API
3. Central API validates the request and initiates the card creation process
4. Translation Service translates the provided text
5. Image Composition Service combines the translated text with images
6. Card Generation Service creates the final card and stores it
7. Response is returned to the user through the API Gateway

```
User → Frontend → API Gateway → Central API → Translation Service
                                      │
                                      ▼
User ← Frontend ← API Gateway ← Central API ← Card Generation Service ← Image Composition Service
```

## Deployment Infrastructure

The system is deployed using Docker containers orchestrated with Docker Compose:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         Docker Compose Network                          │
│                                                                         │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐  ┌───────────┐ │
│  │ Frontend      │  │ Central API   │  │ Translation   │  │ Image     │ │
│  │ Container     │  │ Container     │  │ Container     │  │ Container │ │
│  └───────────────┘  └───────────────┘  └───────────────┘  └───────────┘ │
│                                                                         │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐  ┌───────────┐ │
│  │ Nginx         │  │ PostgreSQL    │  │ Redis         │  │ Monitoring│ │
│  │ Container     │  │ Container     │  │ Container     │  │ Container │ │
│  └───────────────┘  └───────────────┘  └───────────────┘  └───────────┘ │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## Monitoring and Observability

The system includes comprehensive monitoring using Prometheus and Grafana:

```
┌───────────────────┐     metrics     ┌───────────────────┐
│ Service Containers│ ─────────────────►│   Prometheus     │
└───────────────────┘                   └───────────────────┘
                                                │
                                                ▼
                                        ┌───────────────────┐
                                        │     Grafana       │
                                        └───────────────────┘
```

## Security Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           Security Layers                               │
│                                                                         │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐  ┌───────────┐ │
│  │ TLS/SSL       │  │ API Gateway   │  │ Authentication│  │ Rate      │ │
│  │ Encryption    │  │ Security      │  │ & Authorization│ │ Limiting  │ │
│  └───────────────┘  └───────────────┘  └───────────────┘  └───────────┘ │
│                                                                         │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐  ┌───────────┐ │
│  │ Input         │  │ Secret        │  │ Network       │  │ Audit     │ │
│  │ Validation    │  │ Management    │  │ Segmentation  │  │ Logging   │ │
│  └───────────────┘  └───────────────┘  └───────────────┘  └───────────┘ │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

## Conclusion

This architecture diagram provides a high-level overview of the KeenOn Card Orchestration system. The microservice-based architecture allows for independent scaling and development of each component while maintaining clear boundaries and responsibilities.
