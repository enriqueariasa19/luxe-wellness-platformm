# Quick Setup Guide - Luxe Wellness Platform

## What You're Getting
A complete luxury wellness membership platform with:
- 3-tier membership system (Silver/Gold/Platinum)
- Digital wallet with transaction tracking
- QR code member cards
- Admin panel for staff
- English/Spanish language support
- Professional luxury design

## Fastest Free Deployment Options

### Option 1: Vercel + Neon (Recommended)
**Total time: 15 minutes**

1. **Get Neon Database (Free)**
   - Go to neon.tech
   - Sign up and create new project
   - Copy the DATABASE_URL

2. **Deploy to Vercel (Free)**
   - Go to vercel.com
   - Import your code from GitHub
   - Add environment variables:
     ```
     DATABASE_URL=your-neon-url
     SESSION_SECRET=any-random-32-character-string
     NODE_ENV=production
     ```

3. **Setup Database**
   - Run: `npm run db:push`

### Option 2: Railway (Easiest)
**Total time: 10 minutes**

1. Go to railway.app
2. Connect your GitHub repo
3. Add PostgreSQL service
4. Set environment variables (auto-configured)
5. Deploy

### Option 3: Render
**Total time: 20 minutes**

1. Go to render.com
2. Create PostgreSQL database (free)
3. Create web service from GitHub
4. Set environment variables
5. Deploy

## Required Environment Variables (Minimum)
```env
DATABASE_URL=postgresql://user:pass@host:port/db
SESSION_SECRET=your-secret-min-32-characters
NODE_ENV=production
```

## Database Setup (One-time)
After deployment, run this command once:
```bash
npm run db:push
```

This creates all required tables automatically.

## Default Admin Access
- Any user can access admin features initially
- Modify in `server/storage.ts` to restrict access

## Customization Quick Tips

### Change Membership Prices
Edit `client/src/pages/landing.tsx`:
```javascript
// Find these lines and modify:
Silver: $12,000 MXN → Your price
Gold: $20,000 MXN → Your price  
Platinum: $30,000 MXN → Your price
```

### Change Colors/Branding
Edit `client/src/index.css`:
```css
:root {
  --luxury-gold: hsl(45, 100%, 85%);    /* Change this */
  --luxury-sage: hsl(120, 25%, 85%);    /* Change this */
  --luxury-dark: hsl(220, 25%, 15%);    /* Change this */
}
```

### Add Your Logo
Replace the text logo in `client/src/pages/landing.tsx` with:
```jsx
<img src="/your-logo.png" alt="Your Clinic" className="h-8" />
```

## Support Channels
- GitHub Issues for code problems
- Platform documentation for hosting issues
- Database provider support for DB issues

## What's Included in Files
- Complete React frontend with luxury UI
- Express.js backend with authentication
- PostgreSQL database schema
- All required dependencies
- Production build configuration
- Security best practices

Your platform is enterprise-ready and includes everything needed for a professional wellness clinic membership system.