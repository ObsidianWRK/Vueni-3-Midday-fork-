# Midday Vercel Deployment Setup Guide

## Current Status

- ✅ Vercel CLI is installed and authenticated
- ✅ Projects linked to Vercel team: "Damon Kirk's projects"
- ✅ Vercel configurations updated for single-region deployment (iad1)
- ✅ All vercel.json files updated with correct build commands using `bun install --cwd`
- ⚠️ **CRITICAL:** Root Directory MUST be set in Vercel Dashboard for each project
- ⏳ Supabase project needs to be created/setup
- ⏳ Environment variables need to be configured

## Deployed Projects

### 1. Dashboard App
- **Project**: `dashboard` (prj_5kZ9MA1Mmk1ATSl1c1ncsgUSosdq)
- **Location**: `apps/dashboard`
- **Vercel Config**: `apps/dashboard/vercel.json`
- **Status**: Linked to Vercel, build configuration updated
- **Root Directory**: MUST be set to `apps/dashboard` in Dashboard

### 2. Website App
- **Project**: `website` (prj_HwQSzA455bYCnALsF2V4dCYxlwDU)
- **Location**: `apps/website`
- **Vercel Config**: `apps/website/vercel.json`
- **Status**: Linked to Vercel, build configuration updated
- **Root Directory**: MUST be set to `apps/website` in Dashboard

### 3. Email App
- **Project**: `email` (prj_VvoSjuUrKp5AcgaxrkXoiqZjmdj3)
- **Location**: `packages/email`
- **Vercel Config**: `packages/email/vercel.json`
- **Status**: Linked to Vercel, build configuration updated
- **Root Directory**: MUST be set to `packages/email` in Dashboard

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

For monorepo deployments, you MUST configure Root Directory in the Vercel Dashboard. This is **critical** - the deployment will fail without it.

### Configuration Steps for Each App

#### 1. Dashboard App
1. Go to: https://vercel.com/damon-kirks-projects/dashboard/settings
2. In **General → Root Directory**, set to: `apps/dashboard`
3. Save the settings (this is critical - deployment will fail without it!)
4. Add environment variables (see below)
5. Trigger a new deployment (Git push will auto-deploy)

#### 2. Website App
1. Go to: https://vercel.com/damon-kirks-projects/website/settings
2. In **General → Root Directory**, set to: `apps/website`
3. Save the settings (this is critical - deployment will fail without it!)
4. Add environment variables if needed
5. Trigger a new deployment (Git push will auto-deploy)

#### 3. Email App
1. Go to: https://vercel.com/damon-kirks-projects/email/settings
2. In **General → Root Directory**, set to: `packages/email`
3. Save the settings (this is critical - deployment will fail without it!)
4. Trigger a new deployment (Git push will auto-deploy)

**Important Notes:**
- Build and install commands are already configured in `vercel.json` files - no need to set them manually
- Root Directory **CANNOT** be set via CLI or API - it must be done manually in the Dashboard
- Without setting Root Directory, Vercel will look for package.json at the repository root and fail

## Deployment URLs

After deployment, your apps will be available at:
- Dashboard: `https://dashboard-<hash>-damon-kirks-projects.vercel.app`
- Website: `https://website-<hash>-damon-kirks-projects.vercel.app`
- Email: `https://email-<hash>-damon-kirks-projects.vercel.app`

## Troubleshooting

### Build Failure: "Bun could not find a package.json file"

**Error:** 
```
error: Bun could not find a package.json file to install from
note: Run "bun init" to initialize a project
Error: Command "cd ../.. && bun install" exited with 1
```

**Cause:** This happens when:
1. Root Directory is NOT set in Vercel Dashboard
2. The old install command tried to go up too many directory levels

**Solution:** 
1. Go to your Vercel project settings
2. Set **Root Directory** to the appropriate app directory (`apps/dashboard`, `apps/website`, or `packages/email`)
3. The `vercel.json` files now use `bun install --cwd ../..` which works correctly with Root Directory set
4. Trigger a new deployment

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

All `vercel.json` files have been updated with:
- Files: `apps/dashboard/vercel.json`, `apps/website/vercel.json`, and `packages/email/vercel.json`
- `framework: "nextjs"` - Explicitly tells Vercel this is a Next.js project
- `buildCommand: "cd ../.. && turbo build --filter=@midday/<app>"` - Uses Turbo for monorepo builds
- `installCommand: "bun install --cwd ../.."` - Installs from monorepo root without directory context issues
- `outputDirectory: ".next"` - For Next.js apps
- `regions: ["iad1"]` - Single region deployment

### Build Errors
- Ensure all dependencies are in `package.json`
- Check turbo.json configuration
- Verify root package.json has correct scripts
- **Make sure Root Directory is set correctly in Vercel project settings** (this is the #1 issue!)

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

