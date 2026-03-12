# ARCHITECTURE.md

## System Vision

This system serves as the backend for a web application, providing REST APIs to manage products, categories, franchises, announcements, and variants. It supports an admin panel with authentication handled by Supabase and exposes only minimal public endpoints for the main website.

---

## Components

### Backend API

**Technology:** Node.js, Express  
**Deployment:** Vercel

**Responsibilities:**

- Provide REST APIs
- Handle data management
- Secure admin operations
- Send email notifications via Resend
- No payment or order processing

---

### Database

**Platform:** Supabase (PostgreSQL)

**Responsibilities:**

- Store business data: products (with `exists` flag), franchises, announcements
- Manage authentication and admin sessions via Supabase Auth
- Enforce row-level security

---

### Authentication

**Platform:** Supabase Auth  
**Usage:** Admin panel only

**Responsibilities:**

- Admin login
- Session management
- Secure access to admin operations

---

### Email Service

**Platform:** Resend

**Responsibilities:**

- Send notifications
- Send contact emails

---

## Agents / Skills (conceptual)

- **Notification Agent:** Sends emails via Resend when triggered by backend events (e.g., new announcement).
- **Data Validation Agent:** Ensures data consistency and enforces business rules before CRUD operations.
- **Admin Operations Agent:** Orchestrates CRUD operations for categories, products, variants, announcements, and franchises.

---

## Data Models

### Categories

- **Fields:** id, name
- **Usage:** Group products on the web and manage dynamically from the admin panel

### Products

- **Fields:** id, name, price, exists, category_id, price_varies, image_url
- **Usage:** Displayed on the Sabores page; may have fixed or variable prices

### Product Variants

- **Fields:** id, product_id, name, price
- **Usage:** Override base product price when variants exist

### Announcements

- **Fields:** id, title, description, image_url, active
- **Usage:** Shown on the Home page; managed via admin panel

### Franchises

- **Fields:** id, city, location_name, latitude, longitude, manager_name, manager_description, manager_photo, description
- **Usage:** Display franchise information on the web

---

## Endpoints

### Public Endpoints (no authentication)

- `GET /public/products` → Fetch list of products for the main website
- `POST /public/login` → Admin login via Supabase Auth

### Private Endpoints (admin only, requires Supabase Auth)

- `GET|POST|PUT|DELETE /categories` → CRUD for categories
- `GET|POST|PUT|DELETE /products` → CRUD for products
- `GET|POST|PUT|DELETE /product_variants` → CRUD for product variants
- `GET|POST|PUT|DELETE /announcements` → CRUD for announcements
- `GET|POST|PUT|DELETE /franchises` → CRUD for franchises

---

## Data Flow

1. **Public access:** Web client fetches products via `GET /public/products`. No authentication required.
2. **Admin operations:** Admin logs in via `POST /public/login`. Supabase Auth issues session tokens.
3. **CRUD operations:** Authenticated admin interacts with private endpoints. Data is validated and stored in Supabase.
4. **Notifications:** Notification Agent triggers emails through Resend when relevant events occur.
5. **Frontend consumption:** Web frontend consumes public endpoints; admin panel consumes private endpoints securely.
