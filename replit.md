# Luxe Wellness Membership Platform

## Overview

This is a full-stack web application for a luxury aesthetic and wellness clinic that provides a comprehensive membership and loyalty card system. The platform allows clients to purchase annual memberships with tiered benefits, manage digital wallets, book treatments, and access VIP events. The system supports both client-facing features and administrative tools for staff management.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter (lightweight client-side routing)
- **State Management**: TanStack Query (React Query) for server state
- **UI Components**: shadcn/ui with Radix UI primitives
- **Styling**: Tailwind CSS with custom luxury theme variables
- **Build Tool**: Vite with custom configuration for monorepo structure

### Backend Architecture  
- **Runtime**: Node.js with Express.js server
- **Language**: TypeScript with ES modules
- **API Pattern**: REST API with structured route handling
- **Session Management**: Express sessions with PostgreSQL storage
- **Authentication**: Replit OpenID Connect (OIDC) integration

### Database Layer
- **Database**: PostgreSQL via Neon serverless
- **ORM**: Drizzle ORM with connection pooling
- **Schema Management**: Drizzle Kit for migrations
- **Connection**: Neon serverless driver with WebSocket support

## Key Components

### Authentication System
Uses Replit's OIDC authentication with session-based persistence. The system maintains user sessions in PostgreSQL and provides middleware for route protection. User authentication is mandatory for all protected routes.

### Membership Management
Three-tier membership system (Silver, Gold, Platinum) with distinct benefits:
- **Silver**: $12,000 MXN balance, 5% discounts, 1 VIP event
- **Gold**: $20,000 MXN balance, 10% discounts, 2 VIP events, priority booking  
- **Platinum**: $30,000 MXN balance, 15% discounts, unlimited VIP events, exclusive access

### Digital Wallet System
Preloaded balance system where users can spend credits on treatments and products. Includes transaction history, balance tracking, and staff tools for balance adjustments.

### Admin Dashboard
Staff interface for membership management, balance adjustments, transaction monitoring, and QR code scanning for check-ins.

### Internationalization
Bilingual support (English/Spanish) with context-based translation system and localStorage persistence.

## Data Flow

### User Journey
1. User authenticates via Replit OIDC
2. System creates/retrieves user profile and membership
3. Frontend fetches membership data, transactions, and events via React Query
4. User interactions trigger API calls with optimistic updates
5. Staff can manage user accounts through admin interface

### Data Synchronization
- React Query handles caching and synchronization
- Real-time updates through query invalidation
- Optimistic updates for better UX
- Error boundaries for graceful failure handling

## External Dependencies

### Core Framework Dependencies
- **React Ecosystem**: React 18, React DOM, React Query for state management
- **UI Framework**: Radix UI primitives with shadcn/ui component system
- **Styling**: Tailwind CSS with PostCSS processing
- **Form Handling**: React Hook Form with Zod validation

### Backend Dependencies
- **Database**: Neon PostgreSQL serverless with Drizzle ORM
- **Authentication**: Replit OIDC with Passport.js strategy
- **Session Storage**: connect-pg-simple for PostgreSQL session store
- **Security**: Express middleware for CORS, rate limiting, and validation

### Development Tools
- **Build System**: Vite with TypeScript support and custom plugins
- **Code Quality**: TypeScript strict mode, ESLint integration
- **Development**: Hot reload, error overlay, and Replit integration

## Deployment Strategy

### Development Environment
- Replit-hosted with automatic hot reload
- Vite dev server with Express backend integration
- PostgreSQL database provisioned via Replit
- Environment variables managed through Replit secrets

### Production Build
- Vite builds frontend to `dist/public`
- esbuild bundles server code to `dist/index.js`
- Single executable deployment with static file serving
- Database migrations run via `drizzle-kit push`

### Environment Configuration
- `NODE_ENV` controls development vs production behavior
- `DATABASE_URL` required for PostgreSQL connection
- `SESSION_SECRET` for secure session management
- `ISSUER_URL` and `REPL_ID` for Replit authentication

### Key Files
- `package.json`: Defines build scripts and dependencies
- `drizzle.config.ts`: Database configuration and migration settings
- `vite.config.ts`: Frontend build configuration with path aliases
- `server/index.ts`: Express server entry point with middleware setup

## Recent Changes: Latest modifications with dates

### January 25, 2025 - Mobile-First PWA Transformation
- **Converted to Progressive Web App (PWA)**: Added service worker, manifest.json, and offline functionality
- **Mobile-First Design**: Implemented bottom navigation, touch targets, and mobile-optimized layouts
- **Free Deployment Ready**: Created complete GitHub + Vercel + Neon deployment instructions
- **PWA Features**: Add to home screen, offline mode, push notifications ready
- **Mobile Navigation**: Bottom tab bar with native app feel
- **Touch Optimization**: 44px touch targets, haptic feedback, swipe gestures
- **Performance**: Service worker caching, fast loading, mobile-specific CSS optimizations

## Free Deployment Strategy

### Mobile-First PWA (Production Ready)
- **Hosting**: Vercel (free tier) with automatic deployments from GitHub
- **Database**: Neon PostgreSQL (free 512MB tier)
- **CDN**: Global distribution included
- **SSL**: Automatic HTTPS certification
- **PWA Features**: Offline mode, installable, push notifications
- **Cost**: $0/month forever for typical clinic usage

### Deployment Files Created
- `GITHUB_DEPLOYMENT_INSTRUCTIONS.md`: Complete step-by-step deployment guide
- `FREE_MOBILE_DEPLOYMENT.md`: Technical PWA specifications
- `VERCEL_DEPLOYMENT.md`: Platform-specific deployment guide
- `client/public/manifest.json`: PWA configuration
- `client/public/sw.js`: Service worker for offline functionality