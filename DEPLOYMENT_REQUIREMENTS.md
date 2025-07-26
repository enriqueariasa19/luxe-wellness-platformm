# Luxe Wellness Membership Platform - Deployment Requirements

## Overview
This is a complete luxury wellness clinic membership platform with three-tier memberships (Silver, Gold, Platinum), digital wallet system, QR code generation, and admin panel. The platform supports both English and Spanish languages.

## Technical Stack

### Frontend
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter (lightweight client-side routing)
- **State Management**: TanStack Query (React Query) v5
- **UI Components**: shadcn/ui with Radix UI primitives
- **Styling**: Tailwind CSS v3 with custom luxury theme
- **Build Tool**: Vite
- **Form Handling**: React Hook Form with Zod validation

### Backend
- **Runtime**: Node.js 18+
- **Framework**: Express.js
- **Language**: TypeScript with ES modules
- **Authentication**: OpenID Connect (Replit Auth)
- **Session Management**: Express sessions with PostgreSQL storage

### Database
- **Database**: PostgreSQL 14+
- **ORM**: Drizzle ORM
- **Connection**: Neon serverless driver (or standard PostgreSQL)

## Required Environment Variables

```env
# Database (Required)
DATABASE_URL=postgresql://username:password@host:port/database

# Authentication (Required for Replit Auth)
SESSION_SECRET=your-secure-session-secret-min-32-chars
ISSUER_URL=https://replit.com/oidc
REPL_ID=your-replit-app-id
REPLIT_DOMAINS=your-domain.com,your-other-domain.com

# PostgreSQL Connection Details (Auto-generated from DATABASE_URL)
PGHOST=your-postgres-host
PGPORT=5432
PGUSER=your-postgres-user
PGPASSWORD=your-postgres-password
PGDATABASE=your-database-name

# Application Environment
NODE_ENV=production
```

## Database Schema

### Required Tables
The application requires these PostgreSQL tables:

#### 1. Sessions Table (Required for authentication)
```sql
CREATE TABLE sessions (
  sid VARCHAR PRIMARY KEY,
  sess JSONB NOT NULL,
  expire TIMESTAMP NOT NULL
);
CREATE INDEX "IDX_session_expire" ON sessions (expire);
```

#### 2. Users Table
```sql
CREATE TABLE users (
  id VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR UNIQUE,
  first_name VARCHAR,
  last_name VARCHAR,
  profile_image_url VARCHAR,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

#### 3. Memberships Table
```sql
CREATE TABLE memberships (
  id VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id VARCHAR NOT NULL REFERENCES users(id),
  tier VARCHAR NOT NULL CHECK (tier IN ('silver', 'gold', 'platinum')),
  balance INTEGER NOT NULL DEFAULT 0,
  currency VARCHAR NOT NULL DEFAULT 'MXN',
  discount_percentage INTEGER NOT NULL,
  vip_events_remaining INTEGER NOT NULL,
  expires_at TIMESTAMP NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

#### 4. Transactions Table
```sql
CREATE TABLE transactions (
  id VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id VARCHAR NOT NULL REFERENCES users(id),
  amount INTEGER NOT NULL,
  currency VARCHAR NOT NULL DEFAULT 'MXN',
  type VARCHAR NOT NULL CHECK (type IN ('debit', 'credit', 'refund')),
  description VARCHAR NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);
```

#### 5. VIP Events Table
```sql
CREATE TABLE vip_events (
  id VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
  title VARCHAR NOT NULL,
  description TEXT,
  date TIMESTAMP NOT NULL,
  required_tier VARCHAR NOT NULL CHECK (required_tier IN ('silver', 'gold', 'platinum')),
  max_attendees INTEGER,
  created_at TIMESTAMP DEFAULT NOW()
);
```

#### 6. Welcome Gifts Table
```sql
CREATE TABLE welcome_gifts (
  id VARCHAR PRIMARY KEY DEFAULT gen_random_uuid(),
  tier VARCHAR NOT NULL CHECK (tier IN ('silver', 'gold', 'platinum')),
  title VARCHAR NOT NULL,
  description TEXT,
  value INTEGER,
  currency VARCHAR DEFAULT 'MXN',
  created_at TIMESTAMP DEFAULT NOW()
);
```

## Package Dependencies

### Production Dependencies
```json
{
  "@hookform/resolvers": "^3.10.0",
  "@neondatabase/serverless": "^0.10.4",
  "@radix-ui/react-accordion": "^1.2.4",
  "@radix-ui/react-alert-dialog": "^1.1.7",
  "@radix-ui/react-aspect-ratio": "^1.1.3",
  "@radix-ui/react-avatar": "^1.1.4",
  "@radix-ui/react-checkbox": "^1.1.5",
  "@radix-ui/react-collapsible": "^1.1.4",
  "@radix-ui/react-context-menu": "^2.2.7",
  "@radix-ui/react-dialog": "^1.1.7",
  "@radix-ui/react-dropdown-menu": "^2.1.7",
  "@radix-ui/react-hover-card": "^1.1.7",
  "@radix-ui/react-label": "^2.1.3",
  "@radix-ui/react-menubar": "^1.1.7",
  "@radix-ui/react-navigation-menu": "^1.2.6",
  "@radix-ui/react-popover": "^1.1.7",
  "@radix-ui/react-progress": "^1.1.3",
  "@radix-ui/react-radio-group": "^1.2.4",
  "@radix-ui/react-scroll-area": "^1.2.4",
  "@radix-ui/react-select": "^2.1.7",
  "@radix-ui/react-separator": "^1.1.3",
  "@radix-ui/react-slider": "^1.2.4",
  "@radix-ui/react-slot": "^1.2.0",
  "@radix-ui/react-switch": "^1.1.4",
  "@radix-ui/react-tabs": "^1.1.4",
  "@radix-ui/react-toast": "^1.2.7",
  "@radix-ui/react-toggle": "^1.1.3",
  "@radix-ui/react-toggle-group": "^1.1.3",
  "@radix-ui/react-tooltip": "^1.2.0",
  "@tanstack/react-query": "^5.60.5",
  "class-variance-authority": "^0.7.1",
  "clsx": "^2.1.1",
  "cmdk": "^1.1.1",
  "connect-pg-simple": "^10.0.0",
  "date-fns": "^3.6.0",
  "drizzle-orm": "^0.39.1",
  "drizzle-zod": "^0.7.0",
  "embla-carousel-react": "^8.6.0",
  "express": "^4.21.2",
  "express-session": "^1.18.1",
  "framer-motion": "^11.13.1",
  "input-otp": "^1.4.2",
  "lucide-react": "^0.453.0",
  "memoizee": "^0.4.17",
  "nanoid": "^5.1.5",
  "next-themes": "^0.4.6",
  "openid-client": "^6.6.2",
  "passport": "^0.7.0",
  "react": "^18.3.1",
  "react-day-picker": "^8.10.1",
  "react-dom": "^18.3.1",
  "react-hook-form": "^7.55.0",
  "react-icons": "^5.4.0",
  "react-resizable-panels": "^2.1.7",
  "recharts": "^2.15.2",
  "tailwind-merge": "^2.6.0",
  "tailwindcss-animate": "^1.0.7",
  "vaul": "^1.1.2",
  "wouter": "^3.3.5",
  "ws": "^8.18.0",
  "zod": "^3.24.2",
  "zod-validation-error": "^3.4.0"
}
```

### Development Dependencies
```json
{
  "@tailwindcss/typography": "^0.5.15",
  "@tailwindcss/vite": "^4.1.3",
  "@types/connect-pg-simple": "^7.0.3",
  "@types/express": "4.17.21",
  "@types/express-session": "^1.18.0",
  "@types/node": "20.16.11",
  "@types/passport": "^1.0.16",
  "@types/react": "^18.3.11",
  "@types/react-dom": "^18.3.1",
  "@types/ws": "^8.5.13",
  "@vitejs/plugin-react": "^4.3.2",
  "autoprefixer": "^10.4.20",
  "drizzle-kit": "^0.30.4",
  "esbuild": "^0.25.0",
  "postcss": "^8.4.47",
  "tailwindcss": "^3.4.17",
  "tsx": "^4.19.1",
  "typescript": "5.6.3",
  "vite": "^5.4.19"
}
```

## Build Scripts

Add these scripts to your `package.json`:

```json
{
  "scripts": {
    "dev": "NODE_ENV=development tsx server/index.ts",
    "build": "vite build && esbuild server/index.ts --platform=node --packages=external --bundle --format=esm --outdir=dist",
    "start": "NODE_ENV=production node dist/index.js",
    "check": "tsc",
    "db:push": "drizzle-kit push"
  }
}
```

## Deployment Steps

### 1. Platform Setup
- Node.js 18+ environment
- PostgreSQL database (14+)
- Domain/subdomain for hosting

### 2. Database Setup
- Create PostgreSQL database
- Run the SQL schema creation commands above
- Set DATABASE_URL environment variable

### 3. Authentication Setup
For Replit Auth (recommended):
- Set ISSUER_URL to https://replit.com/oidc
- Set REPL_ID to your application ID
- Set REPLIT_DOMAINS to your domain(s)

For custom auth:
- Replace the auth system in `server/replitAuth.ts`
- Implement your own OAuth provider

### 4. Build and Deploy
```bash
# Install dependencies
npm install

# Build the application
npm run build

# Push database schema
npm run db:push

# Start production server
npm start
```

## File Structure Required

```
/
├── client/
│   ├── src/
│   │   ├── components/ui/ (shadcn components)
│   │   ├── hooks/
│   │   ├── lib/
│   │   ├── pages/
│   │   ├── App.tsx
│   │   ├── index.css
│   │   └── main.tsx
│   └── index.html
├── server/
│   ├── db.ts
│   ├── index.ts
│   ├── replitAuth.ts
│   ├── routes.ts
│   ├── storage.ts
│   └── vite.ts
├── shared/
│   └── schema.ts
├── components.json
├── drizzle.config.ts
├── package.json
├── postcss.config.js
├── tailwind.config.ts
├── tsconfig.json
└── vite.config.ts
```

## Key Features Included

### Membership System
- Three tiers: Silver ($12,000 MXN), Gold ($20,000 MXN), Platinum ($30,000 MXN)
- Automatic balance loading and discount application
- VIP event access based on tier

### Digital Wallet
- Balance tracking in MXN/USD
- Transaction history
- QR code generation for member cards

### Admin Panel
- Staff interface for membership management
- Balance adjustments
- Transaction monitoring
- User management

### Multi-language
- English/Spanish support
- Automatic language detection
- localStorage persistence

## Free Hosting Options

1. **Replit Deployments** (Recommended)
   - Free tier available
   - Automatic PostgreSQL database
   - Built-in authentication

2. **Vercel + Neon**
   - Free Vercel hosting
   - Free Neon PostgreSQL
   - Custom domain support

3. **Railway**
   - Free tier with PostgreSQL
   - Easy deployment from Git

4. **Render**
   - Free web service tier
   - PostgreSQL addon available

## Security Notes

- Use HTTPS in production
- Set secure session cookies
- Validate all user inputs
- Use environment variables for secrets
- Enable CORS protection
- Implement rate limiting

## Support

For questions about deployment or customization, ensure you have:
- Node.js knowledge for backend modifications
- React/TypeScript for frontend changes
- PostgreSQL for database management
- Basic understanding of web deployment

This platform is production-ready and includes all necessary security measures, authentication, and database management for a professional wellness clinic membership system.