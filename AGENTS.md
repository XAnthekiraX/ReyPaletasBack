# AGENTS.md - Agentic Coding Guidelines for ReyPaletasBack

This file provides guidelines for AI agents working on this codebase.

---

## Project Overview

- **Project Type:** Node.js/Express REST API Backend
- **Database:** Supabase (PostgreSQL)
- **Authentication:** Supabase Auth
- **Email Service:** Resend
- **Deployment:** Vercel

---

## Build, Lint, and Test Commands

### Installation

```bash
npm install
```

### Running the Server

```bash
# Development (with auto-reload using nodemon if available)
node src/index.js

# Production
NODE_ENV=production node src/index.js
```

### Testing

No test framework is currently configured. When implementing tests:

```bash
# Run all tests
npm test

# Run a single test file (Jest example)
npx jest test/filename.test.js

# Run tests in watch mode
npx jest --watch
```

### Linting

No linter is currently configured. Recommended setup:

```bash
# ESLint (if installed)
npx eslint src/ --ext .js

# Check a specific file
npx eslint src/routes/example.js
```

### Type Checking

No TypeScript is currently in use. If TypeScript is added:

```bash
npx tsc --noEmit
```

---

## Code Style Guidelines

### General Principles

- Use **CommonJS** modules (`require`/`exports`) - project uses `"type": "commonjs"`
- Use **2 spaces** for indentation
- Use **single quotes** for strings in JavaScript
- Use **semicolons** at the end of statements
- Maximum line length: **100 characters**
- Add a **blank line** between imports and the rest of the code

### File Naming

- **Routes:** `routes/something.js` (e.g., `routes/products.js`)
- **Controllers:** `controllers/somethingController.js`
- **Services:** `services/somethingService.js`
- **Models:** `models/somethingModel.js`
- **Middlewares:** `middlewares/authMiddleware.js`
- **Utils:** `utils/helpers.js`

### Naming Conventions

| Element | Convention | Example |
|---------|------------|---------|
| Variables | camelCase | `userName`, `isActive` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Functions | camelCase | `getUserById()` |
| Classes | PascalCase | `UserService` |
| Files/Directories | kebab-case | `user-routes.js` |
| Database tables | snake_case | `product_variants` |

### Imports Order

1. Node built-in modules (e.g., `path`, `fs`, `crypto`)
2. External packages (e.g., `express`, `supabase`)
3. Internal modules (relative imports)

```javascript
// 1. Built-in
const path = require('path');
const crypto = require('crypto');

// 2. External
const express = require('express');
const { createClient } = require('@supabase/supabase-js');

// 3. Internal
const productService = require('../services/productService');
const { validateProduct } = require('../utils/validators');
```

### Error Handling

- Use **try-catch** blocks for async operations
- Always return proper HTTP status codes:
  - `200` - Success
  - `201` - Created
  - `400` - Bad Request
  - `401` - Unauthorized
  - `403` - Forbidden
  - `404` - Not Found
  - `500` - Internal Server Error
- Log errors appropriately (avoid exposing sensitive data)
- Use custom error classes for domain-specific errors

```javascript
// Good error handling pattern
app.get('/products', async (req, res) => {
  try {
    const products = await productService.getAll();
    res.json(products);
  } catch (error) {
    console.error('Error fetching products:', error.message);
    res.status(500).json({ error: 'Failed to fetch products' });
  }
});
```

### Async/Await

- Always handle promise rejections with try-catch
- Use **async/await** over raw promises for readability

```javascript
// Good
async function getProduct(id) {
  try {
    const product = await db.query('SELECT * FROM products WHERE id = $1', [id]);
    return product;
  } catch (error) {
    throw new Error(`Failed to get product: ${error.message}`);
  }
}
```

### Database Operations (Supabase)

- Use parameterized queries to prevent SQL injection
- Handle null values gracefully
- Use Supabase row-level security (RLS) policies

```javascript
// Parameterized query - GOOD
const { data, error } = await supabase
  .from('products')
  .select('*')
  .eq('category_id', categoryId);

// String concatenation - BAD (SQL injection risk)
// const query = `SELECT * FROM products WHERE category_id = ${categoryId}`;
```

### Response Format

Use consistent JSON response structure:

```javascript
// Success response
res.status(200).json({
  success: true,
  data: { /* ... */ }
});

// Error response
res.status(400).json({
  success: false,
  error: 'User-friendly error message'
});

// Paginated response
res.status(200).json({
  success: true,
  data: [...],
  pagination: {
    page: 1,
    limit: 10,
    total: 100
  }
});
```

### Validation

- Validate all inputs at the route level
- Use middleware for reusable validation
- Return specific validation errors

```javascript
// Middleware example
function validateProductInput(req, res, next) {
  const { name, price } = req.body;
  const errors = [];

  if (!name || typeof name !== 'string') {
    errors.push('Name is required');
  }
  if (typeof price !== 'number' || price < 0) {
    errors.push('Price must be a positive number');
  }

  if (errors.length > 0) {
    return res.status(400).json({ success: false, errors });
  }

  next();
}
```

### Security Best Practices

- **Never commit secrets** - use environment variables for API keys
- Validate and sanitize all user inputs
- Use HTTPS in production
- Implement rate limiting on public endpoints
- Don't expose stack traces in production errors
- Use Helmet.js for HTTP headers security

```javascript
// Environment variables usage
const supabaseUrl = process.env.SUPABASE_URL;
const supabaseKey = process.env.SUPABASE_ANON_KEY;

// Never log sensitive data
console.log('User login:', user.email); // OK
console.log('User password:', user.password); // BAD
```

### Comments

- Use **JSDoc** style comments for functions
- Comment **why**, not **what**
- Keep comments up to date with code changes
- Avoid obvious comments

```javascript
// Good: explains the reasoning
// Rate limit to prevent brute-force attacks on login
app.post('/login', rateLimiter, authController.login);

// Bad: states the obvious
// Loop through users
users.forEach(user => { ... });
```

---

## Project Structure

```
reypaletasback/
├── ai/                    # AI agents, skills, tasks
├── docs/                  # Documentation
├── src/
│   ├── controllers/       # Request handlers
│   ├── middlewares/       # Express middleware
│   ├── models/            # DB models & validation
│   ├── routes/            # Route definitions
│   ├── services/          # Business logic
│   ├── utils/             # Helper functions
│   └── index.js           # Entry point
├── package.json
└── AGENTS.md
```

---

## Environment Variables

Required environment variables (store in `.env`, never commit):

```
SUPABASE_URL=your_supabase_url
SUPABASE_ANON_KEY=your_anon_key
SUPABASE_SERVICE_KEY=your_service_key
RESEND_API_KEY=your_resend_api_key
PORT=3000
```

---

## Common Tasks

### Adding a New CRUD Resource

1. Create model in `src/models/`
2. Create service in `src/services/`
3. Create controller in `src/controllers/`
4. Create route in `src/routes/`
5. Register route in `src/index.js`
6. Add validation middleware if needed

### Adding a New Public Endpoint

Routes prefixed with `/public` don't require authentication:

```javascript
router.get('/public/products', productController.getAll);
```

### Adding a New Protected Endpoint

Use auth middleware for protected routes:

```javascript
const authMiddleware = require('../middlewares/auth');

router.get('/products', authMiddleware, productController.getAll);
```
