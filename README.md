# NEXORA.AI

Your AI Operating System for Work - A production-ready AI SaaS platform built with Next.js, PostgreSQL, and OpenAI.

## Features

- **Authentication**: Secure login/register with bcrypt password hashing and JWT sessions
- **AI Modules**: 8 specialized AI workspaces (Business, Writer, Code, Research, Data, Automation, Career, Cyber)
- **Real-time AI Streaming**: Server-Sent Events for streaming OpenAI responses
- **File Processing**: Upload and analyze PDF, Excel, CSV, TXT, MD, and JSON files
- **Project Management**: Save, organize, and manage AI projects
- **Usage Tracking**: Monitor AI requests, tokens, and storage usage
- **Plan Limits**: FREE, PRO, and ELITE tiers with configurable limits
- **Billing**: Stripe subscription integration with webhooks
- **Payment Adapters**: Production-ready architecture for multiple payment providers (Stripe, Click)
- **Responsive Design**: Mobile-first design with premium dark theme
- **Command Palette**: Keyboard navigation (Ctrl/Cmd + K)
- **Settings**: Profile management, password changes, account deletion

## Tech Stack

- **Frontend**: Next.js 15, React 18, TypeScript, Tailwind CSS
- **Backend**: Next.js API Routes, Prisma ORM
- **Database**: PostgreSQL
- **AI**: OpenAI API with streaming responses
- **Authentication**: JWT (jose), bcryptjs
- **Validation**: Zod
- **Payment**: Stripe SDK
- **File Processing**: PDF-parse, XLSX
- **UI Components**: Radix UI primitives

## Architecture

```
app/
├── api/
│   ├── auth/          # Authentication endpoints
│   ├── ai/            # AI streaming endpoints
│   ├── files/         # File upload/processing
│   ├── projects/      # Project CRUD
│   ├── usage/         # Usage tracking
│   ├── billing/       # Stripe & Click payment
│   └── user/          # User settings
├── dashboard/         # Protected dashboard pages
├── login/             # Login page
├── register/          # Registration page
├── layout.tsx         # Root layout
└── globals.css        # Global styles

components/
├── ui/                # Reusable UI components
├── Sidebar.tsx        # Navigation sidebar
├── Header.tsx         # Dashboard header
├── AIWorkspace.tsx    # AI workspace component
├── FileUploader.tsx   # File upload component
└── CommandPalette.tsx # Keyboard navigation

lib/
├── prisma.ts          # Prisma client
├── auth.ts            # Authentication utilities
├── ai.ts              # OpenAI integration
├── ai/prompts.ts      # AI system prompts
├── file-parser.ts     # File processing
├── limits.ts          # Plan limits
├── stripe.ts          # Stripe integration
├── click.ts           # Click payment adapter
├── validation.ts      # Zod schemas
├── utils.ts           # Utility functions
├── env-validation.ts  # Environment validation
├── rate-limit.ts      # Rate limiting
└── security.ts        # Security utilities

prisma/
├── schema.prisma      # Database schema
└── migrations/        # Database migrations
```

## Database Models

- **User**: Authentication, plan, Stripe customer ID
- **Project**: AI projects with input/output/metadata
- **FileAsset**: Uploaded files with extracted text
- **Usage**: Request counting, token tracking, storage limits
- **Subscription**: Payment provider integration

## Environment Variables

Create a `.env` file in the project root using `.env.example` as a template:

```env
# Database (Required)
DATABASE_URL="postgresql://user:password@localhost:5432/nexora"

# App (Required)
APP_URL="http://localhost:3000"
AUTH_SECRET="your-secret-key-min-32-characters-long"

# OpenAI (Required for AI features)
OPENAI_API_KEY="sk-your-openai-api-key"
OPENAI_MODEL="gpt-4o-mini"

# Stripe (Required for billing)
STRIPE_SECRET_KEY="sk_test_your-stripe-secret-key"
STRIPE_WEBHOOK_SECRET="whsec_your-webhook-secret"
STRIPE_PRO_PRICE_ID="price_your-pro-price-id"
STRIPE_ELITE_PRICE_ID="price_your-elite-price-id"

# Click Payment (Optional - for regional payments)
# Requires Click merchant account from https://click.uz/
CLICK_SERVICE_ID="your-click-service-id"
CLICK_MERCHANT_ID="your-click-merchant-id"
CLICK_SECRET_KEY="your-click-secret-key"
CLICK_RETURN_URL="http://localhost:3000/billing/click/success"
```

## Local Development

### Prerequisites

- Node.js 18+ installed
- PostgreSQL database running locally or hosted
- OpenAI API key
- Stripe account (for billing features)

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd nexora.ai.full.satck
```

2. Install dependencies:
```bash
npm install
```

3. Set up environment variables:
```bash
cp .env.example .env
# Edit .env with your actual values
```

4. Set up the database:
```bash
npx prisma generate
npx prisma db push
```

5. Run the development server:
```bash
npm run dev
```

6. Open [http://localhost:3000](http://localhost:3000) in your browser

### Database Management

```bash
# Generate Prisma Client
npm run db:generate

# Push schema changes (development)
npm run db:push

# Create migration (production)
npm run db:migrate:dev

# Deploy migrations (production)
npm run db:migrate:deploy

# Open Prisma Studio
npm run db:studio
```

## Production Deployment

### Vercel Deployment

1. **Push code to GitHub**:
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <your-github-repo>
git push -u origin main
```

2. **Import in Vercel**:
   - Go to [vercel.com](https://vercel.com)
   - Click "Add New Project"
   - Import your GitHub repository
   - Configure build settings (automatically detected from `vercel.json`)

3. **Configure Environment Variables**:
   In Vercel project settings > Environment Variables, add:
   - `DATABASE_URL` - Your production PostgreSQL connection string
   - `AUTH_SECRET` - Generate a secure 32+ character secret
   - `OPENAI_API_KEY` - Your OpenAI API key
   - `OPENAI_MODEL` - `gpt-4o-mini` or your preferred model
   - `STRIPE_SECRET_KEY` - Your Stripe secret key
   - `STRIPE_WEBHOOK_SECRET` - Your Stripe webhook secret
   - `STRIPE_PRO_PRICE_ID` - Your PRO plan price ID
   - `STRIPE_ELITE_PRICE_ID` - Your ELITE plan price ID
   - `APP_URL` - Your production URL (e.g., `https://your-app.vercel.app`)

4. **Deploy**:
   - Click "Deploy"
   - Vercel will automatically build and deploy

### Database Setup for Production

Use a managed PostgreSQL service:

**Vercel Postgres** (Recommended):
```bash
# In Vercel dashboard, create a Postgres database
# Vercel will automatically set DATABASE_URL
```

**Alternative options**:
- Supabase: `postgresql://postgres.xxx:@aws-0-us-east-1.pooler.supabase.com:5432/postgres`
- Neon: `postgresql://postgres.xxx:@ep-cool-darkness-123456.us-east-2.aws.neon.tech/neondb`
- Railway: Get connection string from Railway dashboard

### Stripe Setup

1. **Create Stripe Account**:
   - Go to [stripe.com](https://stripe.com)
   - Sign up and verify your account

2. **Create Products and Prices**:
   - Go to Products > Add product
   - Create "PRO Plan" ($19/month)
   - Create "ELITE Plan" ($49/month)
   - Copy the Price IDs

3. **Configure Webhook**:
   - Go to Developers > Webhooks
   - Add endpoint: `https://your-app.vercel.app/api/billing/stripe/webhook`
   - Select events:
     - `checkout.session.completed`
     - `customer.subscription.created`
     - `customer.subscription.updated`
     - `customer.subscription.deleted`
     - `invoice.payment_failed`
   - Copy the webhook signing secret

4. **Add to Environment Variables**:
   - `STRIPE_SECRET_KEY` - From Developers > API keys
   - `STRIPE_WEBHOOK_SECRET` - From webhook configuration
   - `STRIPE_PRO_PRICE_ID` - From PRO product price
   - `STRIPE_ELITE_PRICE_ID` - From ELITE product price

### OpenAI Setup

1. **Create OpenAI Account**:
   - Go to [platform.openai.com](https://platform.openai.com)
   - Sign up and verify your account

2. **Generate API Key**:
   - Go to API keys > Create new secret key
   - Copy the key (starts with `sk-`)

3. **Add to Environment Variables**:
   - `OPENAI_API_KEY` - Your OpenAI API key
   - `OPENAI_MODEL` - `gpt-4o-mini` (default) or your preferred model

### Click Payment Setup (Optional)

1. **Create Click Merchant Account**:
   - Go to [click.uz](https://click.uz)
   - Register as a merchant
   - Get merchant credentials

2. **Required Credentials**:
   - Service ID
   - Merchant ID
   - Secret Key

3. **Implement Signature Verification**:
   - The adapter architecture is ready in `lib/click.ts`
   - You need to implement signature verification based on Click's API documentation
   - Update `verifyClickSignature` function with actual implementation

4. **Add to Environment Variables**:
   - `CLICK_SERVICE_ID` - Your Click service ID
   - `CLICK_MERCHANT_ID` - Your Click merchant ID
   - `CLICK_SECRET_KEY` - Your Click secret key
   - `CLICK_RETURN_URL` - Your app's return URL

### Custom Domain Setup

1. **In Vercel**:
   - Go to Settings > Domains
   - Add your custom domain
   - Follow DNS instructions

2. **Update Environment Variables**:
   - Update `APP_URL` to your custom domain
   - Update Stripe webhook endpoint if needed

## Production Checklist

Before going to production:

- [ ] All environment variables are set in Vercel
- [ ] PostgreSQL database is created and accessible
- [ ] Database migrations are deployed (`npm run db:migrate:deploy`)
- [ ] OpenAI API key is valid and has credits
- [ ] Stripe products and prices are created
- [ ] Stripe webhook is configured with correct endpoint
- [ ] Custom domain is configured (if applicable)
- [ ] HTTPS is enabled (automatic on Vercel)
- [ ] Build succeeds without errors
- [ ] Test registration and login
- [ ] Test AI streaming with real OpenAI API
- [ ] Test file upload and processing
- [ ] Test project creation and saving
- [ ] Test Stripe checkout flow
- [ ] Test webhook handling
- [ ] Verify mobile responsiveness
- [ ] Check all security headers are present

## Development Scripts

```bash
# Development
npm run dev              # Start development server

# Production
npm run build            # Build for production
npm start                # Start production server

# Database
npm run db:generate      # Generate Prisma Client
npm run db:push         # Push schema (dev)
npm run db:migrate:dev   # Create migration
npm run db:migrate:deploy # Deploy migrations (prod)
npm run db:studio       # Open Prisma Studio

# Code Quality
npm run lint             # Run ESLint
npm run typecheck        # Run TypeScript check
```

## Plan Limits

| Feature | FREE | PRO | ELITE |
|---------|------|-----|-------|
| AI Requests/month | 50 | 1,000 | 5,000 |
| Storage | 100 MB | 10 GB | 50 GB |
| Price | - | $19/mo | $49/mo |

## Security Features

- Password hashing with bcrypt
- Secure HTTP-only cookies with SameSite protection
- JWT session management with expiration
- CSRF-conscious architecture
- Input validation with Zod
- File upload validation (type, size, extension)
- Rate limiting utilities
- Security headers (HSTS, X-Frame-Options, CSP)
- Environment variable validation
- Ownership verification for all user data
- XSS prevention utilities

## Troubleshooting

### Database Connection Issues

- Verify `DATABASE_URL` is correct and accessible
- Ensure PostgreSQL is running (local) or service is active (production)
- Check database credentials and permissions
- Test connection: `npx prisma db push`

### OpenAI API Errors

- Verify `OPENAI_API_KEY` is valid and starts with `sk-`
- Check API key has sufficient credits
- Ensure model name is correct (`gpt-4o-mini` or similar)
- Check OpenAI service status

### Stripe Webhook Failures

- Verify `STRIPE_WEBHOOK_SECRET` matches exactly
- Check webhook endpoint is accessible: `https://your-domain.com/api/billing/stripe/webhook`
- Review Stripe dashboard logs for webhook delivery
- Ensure Vercel environment includes the secret

### Build Errors

- Run `npm install` to ensure all dependencies are installed
- Run `npx prisma generate` to update Prisma Client
- Check TypeScript errors: `npm run typecheck`
- Verify all environment variables are set

### Runtime Errors

- Check Vercel deployment logs
- Verify environment variables are set correctly
- Ensure database migrations are deployed
- Check API key validity for external services

## Support

For support, contact support@nexora.ai

## License

Proprietary - All rights reserved
