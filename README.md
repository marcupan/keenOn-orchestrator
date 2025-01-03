# KeenOn Card Orchestration

The **KeenOn Card Orchestration** repository manages the overall infrastructure and deployment of the **KeenOn Card Generate** project. It contains configurations for **Docker Compose**, **Nginx**, and service submodules to orchestrate the interaction between the **Central Hub API**, **Translation Service**, and **Image Composition Service**.

## Overview

This repository serves as the **orchestration layer** for deploying the entire project. It includes:
- **Docker Compose** configuration for multi-service deployment.
- **Nginx** configuration to handle reverse proxy and routing.
- **Submodules** for the individual services:
    - `central-api`: The main API in Node.js.
    - `translation-service`: Python-based translation service.
    - `image-composition-service`: Rust-based image processing service.

All services communicate via **gRPC**, with **Nginx** providing load balancing and routing between them.

---

> **Note:** This project is not production-ready but is intended as a demonstration of my learning progress in backend development.
