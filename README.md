# KeenOn Card Orchestration

The **KeenOn Card Orchestration** repository manages the overall infrastructure and deployment of the **KeenOn Card Generate** project. It contains configurations for **Docker Compose**, **Nginx**, and service components to orchestrate the interaction between the **Central Hub API**, **Translation Service**, and **Image Composition Service**.

## Overview

This repository is the **orchestration layer** for deploying the entire project. It includes:
- **Docker Compose** configuration for multi-service deployment.
- **Nginx** configuration to handle reverse proxy and routing.
- **Services** for the individual components:
    - `keenOn-card-generate`: The main API in Node.js.
    - `keenOn-card-translate`: Python-based translation service.
    - `keenOn-card-compose`: Rust-based image processing service.
- **Frontend** application built with Next.js.
- **Monitoring** tools for observability.

All backend services communicate via **gRPC**, with **Nginx** providing load balancing and routing between them. The frontend communicates with the backend via REST APIs.

## Project Structure

```
orchestrator-repo/
├── frontend/                # Frontend application
│   └── keen-on-app/         # Next.js application
├── services/                # Backend services
│   ├── keenOn-card-generate/  # Main API (Node.js)
│   ├── keenOn-card-translate/ # Translation service (Python)
│   ├── keenOn-card-compose/   # Image composition service (Rust)
│   └── backup-cron-jobs     # Backup automation scripts
├── nginx/                   # Nginx configuration
├── monitoring/              # Monitoring configuration
├── docker-compose.yml       # Base Docker Compose configuration
├── docker-compose.development.yml  # Development-specific configuration
└── docker-compose.production.yml   # Production-specific configuration
```

## Setup Instructions

### Prerequisites

- Docker and Docker Compose
- Git
- Node.js (for local development)
- Python (for local development)
- Rust (for local development)

### Installation

1. Clone the repository with submodules:
   ```bash
   git clone https://github.com/your-username/orchestrator-repo.git
   cd orchestrator-repo
   ```

2. Set up environment variables:
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. Start the entire stack:
   ```bash
   # For development
   docker-compose -f docker-compose.yml -f docker-compose.development.yml up -d

   # For production
   docker-compose -f docker-compose.yml -f docker-compose.production.yml up -d
   ```

4. Access the services:
   - Frontend: http://localhost:3000
   - API: http://localhost:4000/api
   - API Documentation: http://localhost:4000/api/docs

## Development Workflow

### Working on Individual Services

Each service has its own README with specific development instructions:

- [keenOn-card-generate](services/keenOn-card-generate/README.md)
- [keenOn-card-translate](services/keenOn-card-translate/README.md)
- [keenOn-card-compose](services/keenOn-card-compose/README.md)
- [Frontend Application](frontend/keen-on-app/README.md)

### Running the Entire Stack

For development with all services:

```bash
docker-compose -f docker-compose.yml -f docker-compose.development.yml up -d
```

### Stopping the Stack

```bash
docker-compose down
```

## Deployment

### Production Deployment

1. Set up production environment variables:
   ```bash
   cp .env.example .env.production
   # Edit .env.production with your configuration
   ```

2. Deploy the stack:
   ```bash
   docker-compose -f docker-compose.yml -f docker-compose.production.yml up -d
   ```

### Backup and Recovery

Automated backups are configured using cron jobs. See [backup-cron-jobs](services/backup-cron-jobs) for details.

## Monitoring

The project includes monitoring tools for observability. See the [monitoring](monitoring) directory for configuration details.

## Security Considerations

- All services run with non-root users in Docker containers
- Nginx is configured with security best practices
- API endpoints are protected with authentication and rate limiting
- Sensitive data is stored securely using environment variables

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -am 'Add my feature'`
4. Push to the branch: `git push origin feature/my-feature`
5. Submit a pull request

---

> **Note:** This project is not production-ready but is intended to demonstrate learning progress in backend development.
