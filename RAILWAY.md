# Deploy Invidious on Railway (One-Click)

Deploy your own **Invidious** instance on **Railway** in 2 minutes. No server experience needed.

---

## What you'll need

- A **GitHub account** (free)
- A **Railway account** (free tier, no credit card)

---

## Step 1: Push to GitHub

1. Go to **https://github.com/new** and create a new **Public** repository (e.g. `invidious-railway`)
2. Open a terminal in this folder and run:

```bash
git init
git add .
git commit -m "initial"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/invidious-railway.git
git push -u origin main
```

---

## Step 2: Deploy on Railway

1. Go to **https://railway.com** → **Sign in with GitHub**
2. **New Project** → **Deploy from GitHub** → select your repo
3. Railway auto-detects `railway.toml` and starts building

> First build takes **8-10 minutes** (Invidious compiles from source).

---

## Step 3: Add PostgreSQL

While building:
1. Click **"+ New"** → **"Database"** → **"PostgreSQL"**
2. Wait ~30 seconds — Railway auto-connects it to your app

---

## Step 4: Done!

Once the build finishes:
1. Click **"Generate Domain"** in your service's **Settings** → **Networking**
2. Open the URL — your Invidious is live!

---

## How it works

The `railway.toml` file handles everything automatically:

- **Docker build** — compiles Invidious from source
- **PostgreSQL plugin** — provisions and connects a database
- **`INVIDIOUS_CONFIG`** — sets database URL, HMAC key, and table checks via environment variables

Railway replaces `${{Postgres.DATABASE_URL}}` with your actual database URL at deploy time.

---

## Tips

- **HMAC key** is pre-set to `Kobir2026`. Change it in Railway dashboard → Variables → set `INVIDIOUS_CONFIG` with `hmac_key: "yourNewKey"`
- **Free tier**: Railway gives $5/month credit (enough for 24/7)
- **Updates**: Just `git push` — Railway auto-deploys
