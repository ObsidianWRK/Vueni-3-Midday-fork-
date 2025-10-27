# Midday Vercel Deployment Setup Guide

## Current Status

- ✅ Vercel CLI is installed and authenticated
- ✅ Projects linked to Vercel team: "Damon Kirk's projects"
- ✅ Vercel configurations updated for single-region deployment (iad1)
- ✅ Dashboard vercel.json updated with correct build commands
- ⚠️ **IMPORTANT:** Root Directory must be set in Vercel Dashboard to `apps/dashboard`
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

## Deploy via Vercel Dashboard (Recommended for Monorepos)

For monorepo deployments, it's best to configure Root Directory in the Vercel Dashboard:

1. Go to your project: https://vercel.com/team_M8ZkinGzi726gTb8ciEL1DCJ/dashboard/settings
2. In **General → Root Directory**, set to: `apps/dashboard`
3. In **Build & Development Settings**:
   - Framework Preset: Auto-detect (should detect Next.js)
   - Build Command: `turbo build --filter=@midday/dashboard` (or leave empty for auto-detection)
   - Install Command: `cd ../.. && bun install`
   - Output Directory: `.next`
4. Add environment variables in the project settings
5. Save and trigger a new deployment

## Deployment URLs

After deployment, your apps will be available at:
- Dashboard: `https://dashboard-<hash>-damon-kirks-projects.vercel.app`
- Website: `https://website-<hash>-damon-kirks-projects.vercel.app`
- Email: `https://email-<hash>-damon-kirks-projects.vercel.app`

## Troubleshooting

### Next.js Not Detected Error

**Error:** `Error: No Next.js version detected. Make sure your package.json has "next" in either "dependencies" or "devDependencies"`

**Cause:** This happens when Vercel is looking for Next.js at the repository root, but the Next.js app is in a subdirectory (e.g., `apps/dashboard`).

**Solution:** Set the **Root Directory** in your Vercel project settings:

1. Navigate to: https://vercel.com/team_M8ZkinGzi726gTb8ciEL1DCJ/dashboard/settings (or your project settings)
2. Scroll to **General** → **Root Directory**
3. Enter: `apps/dashboard` (or `apps/website`, `packages/email` depending on which app)
4. Click **Save**
5. Trigger a new deployment

**Note:** The `rootDirectory` setting cannot be configured in `vercel.json` - it must be set in the Vercel Dashboard project settings.

### Current Configuration

The `apps/dashboard/vercel.json` has been updated with:
- `framework: "nextjs"` - Explicitly tells Vercel this is a Next.js project
- `buildCommand: "turbo build --filter=@midday/dashboard"` - Uses Turbo for monorepo builds
- `installCommand: "cd ../.. && bun install"` - Installs from monorepo root

### Build Errors
- Ensure all dependencies are in `package.json`
- Check turbo.json configuration
- Verify root package.json has correct scripts
- Make sure Root Directory is set correctly in Vercel project settings

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

