# Doctor Patient System - AI Coding Instructions

## Architecture Overview

This is a **Next.js 16 full-stack application** with TypeScript, serving a doctor-patient management system.

### Technology Stack
- **Framework**: Next.js 16 (App Router)
- **Language**: TypeScript (strict mode enabled)
- **Styling**: Tailwind CSS v4 with PostCSS
- **Database**: PostgreSQL (via `pg` driver)
- **Runtime**: Node.js with ES2017 target

### Project Structure
```
server/
├── app/
│   ├── page.tsx          # Landing page (Server Component)
│   ├── layout.tsx        # Root layout with metadata
│   ├── globals.css       # Tailwind directives + global styles
│   └── backend/
│       ├── api/          # Next.js API routes
│       │   └── hello/route.ts  # Health check endpoint
│       └── lib/
│           └── db.ts     # PostgreSQL connection pool
├── public/               # Static assets
├── package.json          # Dependencies (React 19, Next 16, pg, TypeScript)
└── tsconfig.json         # TypeScript config with path alias @/*
```

## Key Patterns & Conventions

### 1. API Route Structure
- **Location**: `app/backend/api/[endpoint]/route.ts`
- **Pattern**: Next.js App Router convention (not Pages Router)
- **Example**: [app/backend/api/hello/route.ts](app/backend/api/hello/route.ts)
  - Uses `NextResponse` from "next/server"
  - Error handling with try/catch returning 500 status
  - Database interaction via centralized pool

### 2. Database Access Pattern
- **Centralized Pool**: [app/backend/lib/db.ts](app/backend/lib/db.ts) exports singleton `Pool`
- **Configuration**: Uses `process.env.DATABASE_URL` (PostgreSQL connection string)
- **Usage**: `import pool from "@/app/backend/lib/db"`; then `pool.query(sql)`

### 3. Component Patterns
- **Server Components by Default**: Page components are server-side (see [app/page.tsx](app/page.tsx))
- **Styling**: Tailwind classes with dark mode support (e.g., `dark:bg-black`)
- **Fonts**: Google fonts imported in layout with CSS variables

### 4. TypeScript Configuration
- **Path Alias**: `@/*` resolves to workspace root
- **Strict Mode**: All TypeScript strict checks enabled
- **React JSX**: Using `react-jsx` (automatic JSX runtime)

## Development Workflow

### Commands
```bash
npm run dev       # Start Next.js dev server (http://localhost:3000, HMR enabled)
npm run build     # Build for production
npm start         # Start production server
npm run lint      # Run ESLint
```

### Environment Setup
- **Required**: `DATABASE_URL` env var (PostgreSQL connection string)
- **Location**: `.env.local` (create if needed; git-ignored)

### Debugging Database Issues
- API route [app/backend/api/hello/route.ts](app/backend/api/hello/route.ts) is a test endpoint showing:
  - `GET /api/hello` returns server time and connection status
  - Use to verify `DATABASE_URL` is set correctly

## Critical Integration Points

### Frontend ↔ Backend
- **API Calls**: Must use Next.js API routes under `app/backend/api/`
- **Response Format**: Use `NextResponse.json()` for consistency
- **Error Handling**: Always wrap DB calls in try/catch, return 500 on failure

### Database Connection
- PostgreSQL pool initialized on first import of `app/backend/lib/db.ts`
- Connection pooling is automatic via `pg` library
- **Single pool instance**: Do NOT create multiple pools in different files

## Important Notes for AI Agents

1. **Never hardcode database credentials** - always use `process.env.DATABASE_URL`
2. **Maintain path alias usage** - new files should import via `@/` prefix
3. **Keep server/client component separation** - use `"use client"` directive only when needed
4. **Follow Next.js App Router patterns** - not Pages Router (legacy)
5. **Preserve TypeScript strict mode** - no `any` types without justification
6. **Test API routes** via the health check endpoint before deploying

## Next Steps for Development
- Add domain models (Doctor, Patient, Appointment entities)
- Implement authentication/authorization middleware
- Create database migration strategy
- Build CRUD endpoints for core entities
