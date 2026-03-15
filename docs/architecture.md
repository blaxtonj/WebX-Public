# Architecture Overview

This document details the architecture of **webx**, highlighting
design principles, folder structure, and key patterns used
to ensure maintainability, performance, and scalability.

## High-Level Structure

```
webx/
├─ app/ # Main pages and routes
├─ data/ # Static configuration and JSON data files
├─ img/ # Static images used throughout the site
├─ public/ # Public assets (favicon, robots.txt, etc.)
├─ UI/ # Reusable UI components
├─ utils/ # Non-UI utility functions and helpers
```

```mermaid
graph TD
  A[webx/] --> B[app/]  
  A --> C[data/]
  A --> D[img/]
  A --> E[public/]
  A --> F[UI/]
  A --> G[utils/]
