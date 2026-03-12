# Tareas - ReyPaletas Backend

## Fase 1: Configuración

- [ ] 1.1 Crear archivo `.env` con variables requeridas
- [ ] 1.2 Instalar dependencias (express, @supabase/supabase-js, resend, dotenv, cors)
- [ ] 1.3 Crear estructura de carpetas src/

## Fase 2: Núcleo

- [ ] 2.1 Configurar Supabase client (config/supabase.js)
- [ ] 2.2 Crear middleware de autenticación
- [ ] 2.3 Crear servidor base (index.js/app.js)

## Fase 3: Endpoints Públicos

- [ ] 3.1 GET /public/products (con filtros category, available)
- [ ] 3.2 GET /public/categories
- [ ] 3.3 GET /public/announcements (solo active=true)
- [ ] 3.4 GET /public/franchises
- [ ] 3.5 POST /public/login

## Fase 4: Endpoints Privados (Admin)

- [ ] 4.1 CRUD /categories
- [ ] 4.2 CRUD /products
- [ ] 4.3 CRUD /product_variants
- [ ] 4.4 CRUD /announcements
- [ ] 4.5 CRUD /franchises

## Fase 5: Servicios

- [ ] 5.1 Servicio de email (Resend) para contacto

## Fase 6: Deployment

- [ ] 6.1 Configurar vercel.json (si es necesario)
- [ ] 6.2 Variables de entorno en Vercel
