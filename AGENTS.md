# AGENTS.md

Guidelines for AI agents working on the ReyPaletas backend.

## Project Overview

- **Type:** Node.js/Express 5.x REST API Backend
- **Module System:** CommonJS (`"type": "commonjs"`)
- **External Services:** Supabase (auth + PostgreSQL), Resend (email)

## Directory Structure

```
src/
├── config/           # Configuration files
├── models/           # Database schemas & validation (Joi)
├── routes/           # Express route definitions
├── controllers/      # Request handlers
├── services/         # Business logic
├── middlewares/      # Auth, validation, error handling
└── utils/            # Helper functions
docs/                 # ARCHITECTURE.md, BUSINESS_LOGIC.md, DATA.md
```

## Available Commands

```bash
# Development
npm run dev          # Start server with nodemon (create nodemon.json first)
node index.js        # Run directly

# Testing (requires setup - see below)
npm test             # Run all tests
npx jest test/path   # Run single test file
npx jest --watch    # Watch mode

# Linting (requires setup - see below)
npm run lint         # Run ESLint
npm run lint -- --fix # Fix auto-fixable issues
```

## Required Setup

Install dev dependencies:
```bash
npm install --save-dev jest supertest eslint prettier eslint-config-prettier
npm install joi dotenv
```

## Code Style

### Naming Conventions
- **Files:** kebab-case (`product-routes.js`, `auth-middleware.js`)
- **Functions:** camelCase (`getProducts`, `validateCategory`)
- **Classes:** PascalCase (`ProductService`, `AuthMiddleware`)
- **Constants:** UPPER_SNAKE_CASE (`MAX_PRODUCTS`)
- **DB Tables:** snake_case (`product_variants`)

### Imports/Exports (CommonJS)
```javascript
const express = require('express');
const { supabase } = require('./config/supabase');
const productService = require('./services/product-service');

module.exports = router;
```

### Error Handling
- Use try/catch for all async operations
- Return proper HTTP status codes: 200, 201, 400, 401, 403, 404, 500

```javascript
// Controller pattern
async function getProducts(req, res) {
  try {
    const products = await productService.getProducts(req.query);
    res.status(200).json({ data: products });
  } catch (error) {
    console.error('Error:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
}
```

### Response Format
```javascript
res.status(200).json({ data: result });
res.status(201).json({ data: createdItem });
res.status(400).json({ error: 'Descriptive message' });
```

### Architecture
- Controllers: request/response only, call services
- Services: business logic, external integrations
- Models: validation schemas (Joi), database types
- Never expose internal DB IDs in public API responses

## API Design

- Public endpoints: `/public` prefix, no auth (except login)
- Private endpoints: Supabase Bearer token, validate roles for writes
- RESTful patterns: GET/POST/PUT/DELETE for collections and resources

## Security

- Use environment variables for secrets (`.env` - never commit)
- Validate authentication tokens on private routes
- Sanitize user input before database queries

## Documentation

Update `docs/` files when making architectural changes.

## Git Workflow

- Feature branches, meaningful commit messages
- Never commit `.env`, node_modules, or secrets

## Skills

- **express-production:** Production-ready Express patterns
- **supabase-postgres-best-practices:** PostgreSQL/Supabase optimization
- **javascript-typescript-jest:** Testing patterns
