# Midday Vercel Deployment Setup Guide

## Current Status

- ✅ Vercel CLI is installed and authenticated
- ✅ Projects linked to Vercel team: "Damon Kirk's projects"
- ✅ Vercel configurations updated for single-region deployment (iad1)
- ⏳ Supabase project needs to be created/setup
- ⏳ Environment variables need to be configured

## Deployed Projects

### 1. Dashboard App
- **Project**: `dashboard` (team_M8ZkinGzi726gTb8ciEL1DCJ)
- **Location**: `apps/dashboard`
- **Vercel Config**: `apps/dashboard/vercel.json`
- **Status**: Linked to Vercel, ready for deployment

### 2. Website App
- **Location**: `apps/website`
- **Vercel Config**: `apps/website/vercel.json`
- **Status**: Configuration updated, needs linking

### 3. Email App
- **Location**: `packages/email`
- **Vercel Config**: `packages/email/vercel.json`
- **Status**: Configuration updated, needs linking

## Required Environment Variables

### For Dashboard App (Required)
```bash
cd apps/dashboard

# Supabase configuration
vercel env add NEXT_PUBLIC_SUPABASE_URL production
vercel env add NEXT_PUBLIC_SUPABASE_ANON_KEY production
vercel env add NEXT_PUBLIC_SUPABASE_ID production
vercel env add DATABASE_SESSION_POOLER production

# API configuration
vercel env add NEXT_PUBLIC_API_URL production
```

### For Website App (Optional - for stats/ticker features)
```bash
cd apps/website
vercel env add NEXT_PUBLIC_SUPABASE_URL production
vercel env add NEXT_PUBLIC_SUPABASE_ANON_KEY production
```

### For Email App
No environment variables required for basic functionality.

## Deploy Commands

### Dashboard
```bash
cd apps/dashboard
vercel --prod --yes
```

### Website
```bash
cd apps/website
vercel --prod --yes
```

### Email
```bash
cd packages/email
vercel --prod --yes
```

## Supabase Setup Steps

1. **Create Supabase Project**
   - Go to https://supabase.com/dashboard
   - Click "New Project"
   - Choose a name and database password
   - Select a region

2. **Get Credentials**
   - Project URL: Found in Settings → API (e.g., `https://xxxxx.supabase.co`)
   - Anon Key: Found in Settings → API → anon/public key
   - Project ID: Extract from URL (the part before `.supabase.co`)
   - Connection String: Settings → Database → Connection string (Session pooler)

3. **Set Environment Variables**
   - Run the commands above in each app directory
   - When prompted, paste the values from Supabase

## Alternative: Deploy via Vercel Dashboard

If you prefer using the web interface:

1. Go to https://vercel.com/damon-kirks-projects
2. For each app, add the corresponding project
3. Configure:
   - Root Directory: `apps/dashboard`, `apps/website`, or `packages/email`
   - Install Command: `cd ../.. && bun install`
   - Build Command: `turbo build --filter=<package-name>`
4. Add environment variables in the project settings

## Deployment URLs

After deployment, your apps will be available at:
- Dashboard: `https://dashboard-<hash>-damon-kirks-projects.vercel.app`
- Website: `https://website-<hash>-damon-kirks-projects.vercel.app`
- Email: `https://email-<hash>-damon-kirks-projects.vercel.app`

## Troubleshooting

### Build Errors
- Ensure all dependencies are in `package.json`
- Check turbo.json configuration
- Verify root package.json has correct scripts

### Multiple Regions Error
- Configurations already updated to use only `iad1` region
- If you upgrade to Pro plan, you can change back to multiple regions

### Environment Variables Not Working
- Ensure `NEXT_PUBLIC_*` variables are added for production
- Redeploy after adding new environment variables
- Check Vercel dashboard → Settings → Environment Variables

## Next Steps

1. Create a Supabase project (if you don't have one yet)
2. Add environment variables for each app
3. Deploy each app using the commands above
4. Test the deployments
5. Configure custom domains (optional)

