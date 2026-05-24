# Deploy Invidious on Railway (One-Click)

This guide walks you through deploying your own **Invidious** instance on **Railway** — a cloud platform that takes care of hosting, databases, SSL, and domains for you. No server experience needed.

---

## What you'll need

- A **GitHub account** (free)
- A **Railway account** (free tier available, no credit card required)
- This Invidious code (you already have it!)

---

## Step 1: Push this code to GitHub

1. Go to **https://github.com** and sign in
2. Click the **"+"** icon (top-right) → **"New repository"**
3. Name it something like `invidious-railway`, keep it **Public**, click **"Create repository"**
4. Copy the commands from the "push an existing repository" section

5. Open a terminal in **this folder** (`C:\Users\Kobir Shah\Downloads\invidious-master`) and run:

```bash
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/invidious-railway.git
git push -u origin main
```

> Replace `YOUR_USERNAME` with your actual GitHub username.

---

## Step 2: Deploy on Railway

1. Go to **https://railway.com** and click **"Sign in"** → **"Continue with GitHub"**
2. Authorize Railway to access your GitHub account
3. Click **"New Project"** → **"Deploy from GitHub repo"**
4. Select **"Configure GitHub App"** → grant access to your `invidious-railway` repo
5. Go back, click **"New Project"** → **"Deploy from GitHub repo"** → select `invidious-railway`
6. Railway will automatically detect the `railway.toml` file and start building

> The first build takes about **8–10 minutes**. That's normal — Invidious compiles itself from source.

---

## Step 3: Add PostgreSQL

While the build is running:

1. Click **"+ New"** in the Railway dashboard
2. Select **"Database"** → **"PostgreSQL"**
3. Wait ~30 seconds for it to provision

Railway will automatically connect your database to the Invidious app and add the `DATABASE_URL` environment variable.

---

## Step 4: Set your secret key

You need to set one environment variable for security:

1. Click on your **Invidious service** (not the database)
2. Go to the **"Variables"** tab
3. Click **"New Variable"** and add:

| Key | Value |
|---|---|
| `HMAC_KEY` | `myRandomSecretKey123` |

> Change the value to anything random — this is used for signing cookies and CSRF tokens. Something like `aB3xK9mQ7wZ2pL5rT8vY` works great.

---

## Step 5: Redeploy

1. Go to the **"Deployments"** tab
2. Click **"Redeploy"**
3. Wait for the build to finish (~8 minutes)

---

## Step 6: Get your public URL

1. Go to **"Settings"** tab of your Invidious service
2. In the **"Networking"** section, click **"Generate Domain"**
3. Click the generated URL — your Invidious instance is live! 🎉

Your URL will look like: `https://invidious-railway.up.railway.app`

---

## Final checks

- Open the URL in your browser — you should see the Invidious homepage
- Try searching for a video
- Visit `https://your-url/api/v1/stats` to confirm everything is working

---

## How it works (the `railway.toml` file)

The file `railway.toml` in this project tells Railway exactly what to do:

| Setting | What it does |
|---|---|
| `builder = "DOCKERFILE"` | Use Docker to build Invidious |
| `dockerfilePath` | Where the Dockerfile lives |
| `[[plugins]]` | Add a PostgreSQL database automatically |
| `INVIDIOUS_CONFIG` | Set database URL, domain, and security key via environment variables |

Railway replaces `${{Postgres.DATABASE_URL}}` with your actual database URL and `${{RAILWAY_PUBLIC_DOMAIN}}` with your app's domain — all automatically.

---

## Tips

- **Change your HMAC key** to something unique — don't leave the default
- **Set a custom domain** in Railway Settings → Domains if you have one
- **Free tier limit**: Railway gives $5/month credit (enough to run this 24/7)
- **Updates**: To update, just `git push` to your GitHub repo — Railway auto-deploys

---

## Need help?

- Open an issue on the Invidious GitHub: https://github.com/iv-org/invidious/issues
- Railway docs: https://docs.railway.com
