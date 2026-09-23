# Ledger — Vercel & Supabase Deployment Guide

Ledger is a **local-first personal finance assistant** with natural language input, instant offline responsiveness, and cloud synchronization powered by **Supabase** and deployed on **Vercel**.

---

## 🚀 Quick Setup (3 Steps)

### Step 1: Create a Free Supabase Project

1. Go to [supabase.com](https://supabase.com) and click **Start your project** (Free).
2. Choose a project name (e.g. `ledger-finance`), set a strong database password, and choose your preferred region.
3. Once created, go to **Project Settings $\rightarrow$ API** (or the **Connect** button at the top):
   - Copy **Project URL** (e.g., `https://abcdefghijkl.supabase.co`)
   - Copy **Project API Key (anon/public)** (e.g., `eyJhbGciOi...`)

### Step 2: Run the Database Schema Migration

1. In your Supabase dashboard, click **SQL Editor** on the left navigation bar.
2. Click **New Query**.
3. Open the [`supabase/schema.sql`](supabase/schema.sql) file from this repository, copy its entire contents, and paste it into the query editor.
4. Click **Run** (green button).
   - This creates the `transactions` and `user_settings` tables.
   - It enables **Row Level Security (RLS)** so users can only ever access their own data.
   - It sets up automatic performance indexes and triggers.

---

### Step 3: Deploy to Vercel

#### Method A: Git Push (Recommended for Production)

1. Push this folder to a new **GitHub repository**:
   ```bash
   git init
   git add .
   git commit -m "Initial Ledger release with Supabase and Vercel"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
   git push -u origin main
   ```
2. Open [vercel.com](https://vercel.com), sign in, and click **Add New... $\rightarrow$ Project**.
3. Import your GitHub repository.
4. Under **Environment Variables**, add:
   - `VITE_SUPABASE_URL`: Your Supabase Project URL
   - `VITE_SUPABASE_ANON_KEY`: Your Supabase anon/public key
5. Click **Deploy**! Vercel will automatically build and publish your app with a global CDN and HTTPS.

---

#### Method B: Test Locally First

1. Install dependencies:
   ```bash
   npm install
   ```
2. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```
   Add your `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`.
3. Run the development server:
   ```bash
   npm run dev
   ```
4. Open [http://localhost:3000](http://localhost:3000) in your browser.

*(Note: Even without a `.env` file, you can click the **Sync** button in the header and paste your credentials directly under the **Supabase Config** tab to test immediately!)*

---

## 🔐 How Authentication & Sync Work

- **Guest Mode**: Anyone can use Ledger instantly without an account. All data is saved in browser storage.
- **Dual Authentication**:
  - **Email + Password**: Standard sign up and sign in.
  - **Magic Link**: Passwordless email OTP / login link.
- **Bidirectional Sync Engine**:
  - Writes locally first with **0ms latency**.
  - Pushes changes to Supabase in the background.
  - Automatically merges guest transactions into the cloud when a user signs in.
  - Handles offline states gracefully—queued changes sync when reconnected.
