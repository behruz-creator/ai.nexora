# NEXORA.AI - Agent Documentation

## Project Overview

NEXORA.AI is a production-ready AI SaaS platform built with Next.js, PostgreSQL, and OpenAI. This document contains important information for agents working on this project.

## Build Commands

### Installation
```bash
npm install
```

### Database Setup
```bash
npx prisma generate
npx prisma db push
npx prisma studio
```

### Development
```bash
npm run dev
```

### Production Build
```bash
npm run build
npm start
```

### Code Quality
```bash
npm run lint
npx tsc --noEmit
```

## Environment Variables

Required environment variables:
- `DATABASE_URL` - PostgreSQL connection string
- `AUTH_SECRET` - JWT secret (min 32 characters)
- `OPENAI_API_KEY` - OpenAI API key

Optional for billing:
- `STRIPE_SECRET_KEY`
- `STRIPE_WEBHOOK_SECRET`
- `STRIPE_PRO_PRICE_ID`
- `STRIPE_ELITE_PRICE_ID`

Optional for Click payment:
- `CLICK_SERVICE_ID`
- `CLICK_MERCHANT_ID`
- `CLICK_SECRET_KEY`
- `CLICK_RETURN_URL`

## Architecture Notes

### Database Models
- User (authentication, plan, Stripe customer)
- Project (AI projects with metadata)
- FileAsset (uploaded files with extracted text)
- Usage (request counting, token tracking)
- Subscription (payment provider integration)

### API Structure
All API routes are under `/api/` and follow REST conventions:
- Authentication: `/api/auth/*`
- AI streaming: `/api/ai/stream`
- Files: `/api/files/*`
- Projects: `/api/projects/*`
- Usage: `/api/usage`
- Billing: `/api/billing/*`
- User settings: `/api/user/*`

### Security Implementation
- All protected routes verify JWT sessions
- Database operations scoped by user ID
- File uploads validated for type and size
- Input validation with Zod schemas
- Security headers in Next.js config
- Rate limiting utilities available

### AI Integration
- Uses OpenAI Responses API with streaming
- Server-Sent Events for real-time output
- Module-specific system prompts in `lib/ai/prompts.ts`
- Token counting and usage tracking
- AbortController for stopping generation

### File Processing
- Supports PDF, Excel, CSV, TXT, MD, JSON
- Text extraction limited to 100k characters
- Filename sanitization
- Storage limit checking per plan

## Common Tasks

### Adding a New AI Module
1. Add prompt to `lib/ai/prompts.ts`
2. Create page in `app/dashboard/[module]/page.tsx`
3. Add navigation item to `components/Sidebar.tsx`
4. Add to command palette in `components/CommandPalette.tsx`

### Modifying Plan Limits
Edit `lib/limits.ts` to adjust request/storage limits for FREE, PRO, ELITE plans.

### Adding Payment Provider
1. Create adapter in `lib/[provider].ts`
2. Add checkout endpoint in `app/api/billing/[provider]/`
3. Add webhook handler if needed
4. Update billing page UI

### Database Schema Changes
1. Edit `prisma/schema.prisma`
2. Run `npx prisma db push`
3. Run `npx prisma generate`

## Troubleshooting

### TypeScript Errors
- Run `npx tsc --noEmit` to see all errors
- Check imports are correct (@/ alias)
- Verify Prisma types are generated

### Build Failures
- Ensure all dependencies installed
- Check environment variables are set
- Verify Prisma client is generated
- Check for syntax errors in TypeScript files

### Database Issues
- Verify DATABASE_URL is correct
- Check PostgreSQL is running
- Run `npx prisma db push` to sync schema
- Use `npx prisma studio` to inspect data

### AI Streaming Issues
- Verify OPENAI_API_KEY is valid
- Check model name is correct
- Ensure stream is properly consumed
- Check network connectivity

## Testing Checklist

Before considering changes complete:

- [ ] TypeScript compiles without errors
- [ ] ESLint passes
- [ ] Production build succeeds
- [ ] Database schema is valid
- [ ] All API routes respond correctly
- [ ] Authentication works (login/register/logout)
- [ ] Protected routes require authentication
- [ ] AI streaming works with OpenAI
- [ ] File upload processes correctly
- [ ] Project CRUD operations work
- [ ] Usage tracking updates correctly
- [ ] Plan limits are enforced
- [ ] Settings page functions work
- [ ] Responsive design works on mobile
- [ ] Security headers are present
- [ ] Environment variables are validated

## Important Notes

- Never commit `.env` files
- Never expose API keys in client code
- Always verify user ownership for data operations
- Use AbortController for cancellable operations
- Implement proper error handling with meaningful messages
- Follow existing code patterns and conventions
- Keep components modular and reusable
- Use TypeScript strictly (avoid `any`)
- Test file uploads with actual files, not mocks
- Verify AI streaming with real OpenAI API calls
