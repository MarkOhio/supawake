<div align="center">

<img src="https://img.shields.io/badge/SupaWake-Free%20%26%20Open%20Source-3ecf8e?style=for-the-badge&logo=supabase&logoColor=white" alt="SupaWake" />

# ⚡ SupaWake

### Stop your Supabase free-tier projects from pausing.

SupaWake automatically pings your Supabase database every **3 days** so it never hits the 7-day inactivity pause. Add your projects once, and forget about it forever.

**[🚀 Open App](https://markohio.github.io/supawake/)** • **[📖 About](https://markohio.github.io/supawake/about.html)** • **[🔒 Privacy](https://markohio.github.io/supawake/privacy.html)**

---

![GitHub deployments](https://img.shields.io/github/deployments/MarkOhio/supawake/github-pages?label=Deployed&style=flat-square&color=3ecf8e)
![GitHub last commit](https://img.shields.io/github/last-commit/MarkOhio/supawake?style=flat-square&color=3ecf8e)
![License](https://img.shields.io/badge/License-MIT-3ecf8e?style=flat-square)
![Made with HTML](https://img.shields.io/badge/Built%20with-HTML%2FCSS%2FJS-3ecf8e?style=flat-square)

</div>

---

## 🔥 The Problem

Supabase automatically **pauses free-tier projects after 7 days of inactivity**. When paused:

- Users cannot sign up or log in
- Data cannot be read or written
- Your app appears completely broken
- Unpausing takes several minutes

If a recruiter, client, or friend visits your project and hits a broken signup — they move on. You never know it happened.

---

## ✅ The Solution

SupaWake sends a lightweight authenticated request to your Supabase database every **3 days** — resetting the inactivity clock automatically. Even if one ping fails, the next one fires 3 days later, still well within the 7-day window.

---

## ✨ Features

| Feature | Description |
|---|---|
| ⚡ **Auto Ping** | Pings every 3 days automatically via GitHub Actions |
| 🔘 **Ping Now** | Manually ping any project instantly (once per day per project) |
| ⚡ **Ping All** | Ping all your projects in one click |
| 📊 **Live Dashboard** | See last pinged date and status (Awake / Pending / Error) for each project |
| 🔐 **Private by Default** | Each user only sees their own projects — secured by Firebase Auth + Firestore rules |
| 🗑️ **Delete Account** | Full GDPR-compliant account and data deletion |
| 📱 **Mobile Friendly** | Works on any device |
| 🆓 **Completely Free** | No credit card. No paid plan. Forever. |

---

## 🚀 How to Use

### 1. Sign in with Google
One click — no password, no email verification.

### 2. Add your Supabase project
Paste your **project URL** and **anon key** (the public key — never your service_role key).

### 3. One-time setup (30 seconds)
Run this SQL once in your **Supabase SQL Editor**:

```sql
CREATE OR REPLACE FUNCTION public.supawake_ping()
RETURNS json
LANGUAGE sql
SECURITY DEFINER
AS $$
  SELECT json_build_object('status', 'ok', 'ts', now());
$$;

GRANT EXECUTE ON FUNCTION public.supawake_ping() TO anon;
```

This creates a lightweight function SupaWake calls to keep your database active.

### 4. Done
SupaWake pings your project every 3 days automatically. Your dashboard shows the last pinged date and status in real time.

---

## 🔒 Security

- **Your anon key is safe to store.** It is designed to be public — it is embedded in every Supabase frontend app by default. Real security comes from Row Level Security on your tables.
- **Never store your service_role key** in SupaWake or any third-party tool.
- **Firestore Security Rules** ensure only you can read, write, or delete your own projects. No other user can access your data.
- **Firebase Authentication** blocks all unauthenticated access entirely.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML, CSS, JavaScript (single file) |
| Auth | Firebase Authentication (Google Sign-In) |
| Database | Firebase Firestore |
| Scheduler | GitHub Actions (cron every 3 days) |
| Hosting | GitHub Pages |

---

## 📁 Project Structure

```
supawake/
├── index.html              ← Full app (landing + dashboard)
├── about.html              ← How it works + SQL guide
├── privacy.html            ← Privacy Policy
├── terms.html              ← Terms of Service
├── sitemap.xml             ← For search engine indexing
├── robots.txt              ← Crawler instructions
├── README.md               ← You are here
└── .github/
    └── workflows/
        └── wake.yml        ← GitHub Actions pinger
```

---

## ⚙️ Self-Hosting / Contributing

Want to run your own instance?

1. **Fork this repo**

2. **Create a Firebase project** at [console.firebase.google.com](https://console.firebase.google.com)
   - Enable **Firestore** and **Google Authentication**
   - Set Firestore rules (see below)

3. **Add your Firebase config** to `index.html`:
```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

4. **Add GitHub Secrets** in your repo Settings → Secrets → Actions:
   - `FIREBASE_PROJECT_ID`
   - `FIREBASE_API_KEY`

5. **Set Firestore Security Rules:**
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /projects/{docId} {
      allow read: if true;
      allow update: if true;
      allow create: if request.auth != null
        && request.auth.uid == request.resource.data.uid;
      allow delete: if request.auth != null
        && request.auth.uid == resource.data.uid;
    }
  }
}
```

6. **Enable GitHub Pages** in repo Settings → Pages → Deploy from `main` branch

---

## 🔍 Verify Your Pings

Run this in your **Supabase SQL Editor** to confirm pings are reaching your database:

```sql
SELECT
  query,
  calls,
  mean_exec_time,
  total_exec_time
FROM pg_stat_statements
WHERE query ILIKE '%supawake%'
ORDER BY calls DESC
LIMIT 10;
```

The `calls` column increments every time SupaWake successfully pings your database.

---

## 📄 License

MIT License — free to use, fork, and modify.

---

<div align="center">

Built with ❤️ by **[Mark Ohio](https://markohio.github.io)**

⭐ Star this repo if SupaWake saved your project from pausing!

</div>

