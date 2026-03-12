# AGENTS.md

This file provides guidelines for AI agents working on the ReyPaletas backend codebase.

## Project Overview

- **Project Type:** Node.js/Express.js REST API Backend
- **Package Manager:** npm
- **Module System:** CommonJS (`"type": "commonjs"` in package.json)
- **Runtime:** Node.js
- **Key Dependencies:** Express 5.x
- **External Services:** Supabase (auth + PostgreSQL), Resend (email)

## Directory Structure

```
backend/
├── ai/                   # AI agents, skills, and tasks (future use)
│   ├── agents/
│   ├── skills/
│   └── tasks/
├── docs/                 # Documentation
│   ├── ARCHITECTURE.md
│   ├── BUSINESS_LOGIC.md
│   ├── DATA.md
│   └── REPOSITORY_STRUCTURE.md
├── src/                  # Source code (create as needed)
│   ├── models/           # Database models & validation schemas
│   ├── routes/           # Express route definitions
│   ├── controllers/      # Request handlers
│   ├── services/         # Business logic & external integrations
│   ├── middlewares/      # Express middlewares (auth, validation)
│   └── utils/            # Helper functions
├── node_modules/
├── package.json
└── package-lock.json
```

## Available Commands

### Running the Server

```bash
npm start           # Start the server (requires index.js entry point)
node index.js       # Run directly
```

### Installing Dependencies

```bash
npm install         # Install all dependencies
```

### Testing

Currently no test framework is configured. Recommended setup:

```bash
# Install testing dependencies
npm install --save-dev jest supertest

# Run all tests
npm test

# Run a single test file
npx jest tests/filename.test.js

# Run tests in watch mode
npm test -- --watch
```

### Linting

No linter is currently configured. Recommended setup:

```bash
# Install ESLint
npm install --save-dev eslint

# Initialize ESLint
npx eslint --init

# Run linting
npm run lint

# Fix auto-fixable issues
npm run lint -- --fix
```

## Environment Variables

Create a `.env` file in the root directory (copy from `.env.example`):

```bash
# Supabase Configuration
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_KEY=your-service-key

# Resend (Email Service)
RESEND_API_KEY=re_123456789

# Server
PORT=3000
NODE_ENV=development

# CORS
CORS_ORIGIN=http://localhost:5173
```

**Important:** Never commit `.env` files to version control. Only `.env.example` should be tracked.

## Code Style Guidelines

### General Principles

- Keep functions small and focused (single responsibility)
- Business logic belongs in services, not controllers
- Controllers should only handle request/response and call service functions
- All responses to public endpoints should omit internal IDs

### Naming Conventions

- **Files:** kebab-case (e.g., `product-routes.js`, `auth-middleware.js`)
- **Functions:** camelCase (e.g., `getProducts`, `validateCategory`)
- **Classes:** PascalCase (e.g., `ProductService`, `AuthMiddleware`)
- **Constants:** UPPER_SNAKE_CASE (e.g., `MAX_PRODUCTS`, `DEFAULT_PAGE_SIZE`)
- **Database Tables:** snake_case (e.g., `product_variants`, `announcements`)

### Imports and Exports

```javascript
// Use CommonJS require/exports
const express = require('express');
const { supabase } = require('./config/supabase');
const productService = require('../services/product-service');

// Export at end of file
module.exports = router;
```

### Error Handling

- Use try/catch for async operations
- Return proper HTTP status codes:
  - `200` - Success
  - `201` - Created
  - `400` - Bad Request
  - `401` - Unauthorized
  - `403` - Forbidden
  - `404` - Not Found
  - `500` - Internal Server Error

```javascript
// Example error handling pattern
app.get('/products', async (req, res) => {
  try {
    const products = await productService.getProducts(req.query);
    res.status(200).json(products);
  } catch (error) {
    console.error('Error fetching products:', error);
    res.status(500).json({ error: 'Internal server error' });
  }
});
```

### Async/Await

- Always use async/await over callbacks or .then() chains
- Always wrap in try/catch for error handling

### Request Validation

- Validate all input in middleware or at the start of controller functions
- Use consistent validation error responses

### Response Format

Follow these patterns for consistency:

```javascript
// Success response
res.status(200).json({ data: /* response data */ });

// Creation response
res.status(201).json({ data: /* created item */ });

// Error response
res.status(400).json({ error: 'Descriptive error message' });
```

### Security

- Never expose database IDs in public API responses
- Use environment variables for secrets (create `.env` file, add to `.gitignore`)
- Validate authentication tokens on private routes
- Sanitize user input before database queries

### Configuration

- Store configuration in a `config/` directory
- Use environment variables for environment-specific values
- Never commit secrets to version control

## API Design Guidelines

### Public Endpoints

- Located under `/public` prefix
- No authentication required (except login)
- Never expose internal IDs in responses

### Private Endpoints

- Require Supabase authentication via Bearer token
- Validate user role before allowing write operations

### RESTful Patterns

```javascript
// GET collection
router.get('/products', productController.getProducts);

// GET single
router.get('/products/:id', productController.getProduct);

// POST create
router.post('/products', productController.createProduct);

// PUT update
router.put('/products/:id', productController.updateProduct);

// DELETE
router.delete('/products/:id', productController.deleteProduct);
```

## Documentation

- Update `docs/` files when making architectural changes
- Document new endpoints in BUSINESS_LOGIC.md
- Add new data models to DATA.md

## Git Workflow

- Create feature branches for new functionality
- Write meaningful commit messages
- Do not commit node_modules or sensitive files

## Dependencies to Add

For a production-ready backend, consider adding:

```bash
# Testing
npm install --save-dev jest supertest

# Linting & Formatting
npm install --save-dev eslint prettier eslint-config-prettier

# Validation
npm install joi

# Environment variables
npm install dotenv
```
