# Supabase Configuration

## 1. Initial Setup

For the backend to work correctly, follow these manual steps in the Supabase dashboard:

1. **Create Project**: Create a new project in Supabase and obtain `SUPABASE_URL` and `SUPABASE_KEY` (anon) for the `.env` file.
2. **Execute Schemas**: Copy and paste the SQL script (generated from DATA.md) into the Supabase **SQL Editor**.
3. **Enable RLS**: It is essential to activate **Row Level Security** on all tables to protect admin data.
4. **Configure Auth**: Ensure the Email provider is active for admin panel access.

## 2. Environment Variables

The backend requires the following keys in the `.env` file:

- `SUPABASE_URL`: Your API endpoint.
- `SUPABASE_KEY`: Your anon/public key.
   _Note: Never push these keys to version control._

## 3. Security Considerations

- Private tables should only be accessible via a Bearer token from Supabase Auth
- Internal database IDs should not be exposed in public endpoint responses.
