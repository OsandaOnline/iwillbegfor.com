# 🙏 IWillBegFor.com — Setup Guide for Beginners

Follow these steps exactly, in order. Take your time — each step is explained clearly.

---

## STEP 1 — Install the tools you need

Before anything, install these on your computer:

1. **Node.js** → https://nodejs.org (download the "LTS" version)
2. **VS Code** → https://code.visualstudio.com (free code editor)
3. **Git** → https://git-scm.com

To check they installed correctly, open a Terminal (Mac) or Command Prompt (Windows) and type:
```
node --version
```
You should see something like `v20.x.x`. If yes, you're good!

---

## STEP 2 — Set up Supabase (your database)

Supabase is a free service that stores all your data (users, gigs, donations).

1. Go to https://supabase.com and click **Start for free**
2. Create an account and click **New project**
3. Name it `iwillbegfor`, pick a strong password, choose a region near Sri Lanka (e.g. Singapore)
4. Wait ~2 minutes for it to set up
5. Go to **Project Settings → Database** and copy:
   - **Connection string (Transaction mode)** → this is your `DATABASE_URL`
   - **Connection string (Session mode)** → this is your `DIRECT_URL`
6. Go to **Project Settings → API** and copy:
   - **Project URL** → `NEXT_PUBLIC_SUPABASE_URL`
   - **anon public key** → `NEXT_PUBLIC_SUPABASE_ANON_KEY`
   - **service_role key** → `SUPABASE_SERVICE_ROLE_KEY`

---

## STEP 3 — Set up Stripe (payments)

Stripe lets you receive money from anywhere in the world.

1. Go to https://stripe.com and create a free account
2. Go to **Developers → API Keys** in the Stripe dashboard
3. Copy:
   - **Publishable key** (starts with `pk_test_`) → `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY`
   - **Secret key** (starts with `sk_test_`) → `STRIPE_SECRET_KEY`
4. For the webhook secret — you'll set this up in Step 7. Leave it blank for now.

---

## STEP 4 — Set up Resend (emails)

Resend sends thank-you emails to donors and alerts to creators.

1. Go to https://resend.com and create a free account
2. Go to **API Keys** and click **Create API Key**
3. Copy the key → `RESEND_API_KEY`
4. For `FROM_EMAIL`, use: `hello@iwillbegfor.com` (you'll verify your domain later)

---

## STEP 5 — Configure your environment

1. In the project folder, find the file called `.env.example`
2. Make a COPY of it and name the copy `.env.local`
3. Open `.env.local` in VS Code and fill in all the values you copied above
4. Save the file

⚠️ **Never share `.env.local` with anyone — it contains secret keys!**

---

## STEP 6 — Install and run the project

Open a Terminal, navigate to the project folder, and run these commands one by one:

```bash
# Install all packages (only needed once)
npm install

# Set up the database tables
npm run db:push

# Start the development server
npm run dev
```

Now open your browser and go to: **http://localhost:3000**

You should see your homepage! 🎉

---

## STEP 7 — Set up Stripe webhook (for payments to work)

This tells Stripe to notify your app when someone pays.

1. Install the Stripe CLI: https://stripe.com/docs/stripe-cli
2. In a NEW terminal window, run:
```bash
stripe login
stripe listen --forward-to localhost:3000/api/webhooks/stripe
```
3. It will show a webhook secret starting with `whsec_...`
4. Copy that into your `.env.local` as `STRIPE_WEBHOOK_SECRET`
5. Restart your dev server (`npm run dev`)

---

## STEP 8 — Test a donation

1. Go to http://localhost:3000/gigs/new and post a test gig
2. Go to that gig's page and click "Support this gig"
3. Use Stripe's test card: **4242 4242 4242 4242**, any future date, any CVC
4. You should see the donation go through and the progress bar update!

---

## STEP 9 — Deploy to the internet (go live!)

When you're ready to show the world:

1. Push your code to GitHub (create a free account at https://github.com)
2. Go to https://vercel.com, sign in with GitHub, and import your repo
3. In Vercel's settings, add all your environment variables from `.env.local`
4. Change `NEXT_PUBLIC_APP_URL` to your real domain (e.g. `https://iwillbegfor.com`)
5. Click **Deploy** — your site will be live in ~2 minutes!

---

## Project structure explained

```
iwillbegfor/
├── src/
│   ├── app/                    ← All your pages live here
│   │   ├── page.tsx            ← Homepage (/)
│   │   ├── browse/page.tsx     ← Browse gigs (/browse)
│   │   ├── gigs/
│   │   │   ├── new/page.tsx    ← Create gig form (/gigs/new)
│   │   │   └── [slug]/         ← Individual gig page
│   │   ├── dashboard/page.tsx  ← Creator dashboard (/dashboard)
│   │   ├── auth/login/         ← Login/signup (/auth/login)
│   │   └── api/                ← Backend API routes
│   │       ├── gigs/           ← Gig API endpoints
│   │       ├── donations/      ← Payment endpoints
│   │       └── webhooks/       ← Stripe webhook
│   ├── components/             ← Reusable UI pieces
│   │   ├── layout/Navbar.tsx   ← Top navigation
│   │   ├── layout/Footer.tsx   ← Bottom footer
│   │   └── ui/GigCard.tsx      ← Gig card component
│   └── lib/                    ← Utility functions
│       ├── db.ts               ← Database connection
│       ├── stripe.ts           ← Payment utilities
│       ├── supabase.ts         ← Auth & storage
│       └── utils.ts            ← Helper functions
├── prisma/schema.prisma        ← Database table definitions
├── .env.example                ← Environment variable template
└── .env.local                  ← Your secret keys (never commit!)
```

---

## Need help?

- Next.js docs: https://nextjs.org/docs
- Supabase docs: https://supabase.com/docs
- Stripe docs: https://stripe.com/docs
- Prisma docs: https://prisma.io/docs

Good luck building IWillBegFor.com! 🙏
