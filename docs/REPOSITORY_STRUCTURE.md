# REPOSITORY_STRUCTURE.md

## Project Repository Structure

This document defines the folder structure and purpose of each directory for the AI-Native backend project.

```
backend/
├── ai
│ ├── agents            # AI agents definitions (managed via OpenCode /init)
│ ├── skills            # Reusable AI skills or helper modules
│ └── tasks             # Specific tasks executed by agents
├── docs
│ ├── ARCHITECTURE.md   # System architecture overview
│ └── REPOSITORY_STRUCTURE.md # This document
├── src
│ ├── models            # Database models and validation schemas
│ ├── routes            # Express route definitions (public & private endpoints)
│ ├── controllers       # Request handlers for each route
│ ├── services          # Business logic, integration with Supabase, Resend
│ ├── middlewares       # Express middlewares (auth, validation, error handling)
│ └── utils             # Helper functions and utilities
├── node_modules        # Node.js dependencies
├── package.json        # Project manifest
└── package-lock.json   # Dependency lockfile
```

## Folder Purpose

- **ai/** → Contains AI-related modules: agents, tasks, and skills. Prepares backend for AI-Native extensions.
- **docs/** → Stores system documentation.
- **src/** → All application source code lives here for better modularity.
  - **models/** → Database models and validation schemas for Supabase/PostgreSQL.
  - **routes/** → Maps HTTP endpoints for public and private APIs.
  - **controllers/** → Handles request logic, calling services and returning responses.
  - **services/** → Implements business logic, email notifications, and communication with Supabase.
  - **middlewares/** → Express middlewares for authentication, authorization, and input validation.
  - **utils/** → General utility functions.
- **node_modules/** → Installed Node.js dependencies.
- **package.json / package-lock.json** → Node.js project manifest and dependency lockfile.

This structure keeps the backend modular, clean, and ready for AI-Native development while separating documentation, AI logic, and source code.
