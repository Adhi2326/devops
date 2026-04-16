# ⬡ DevOps — Secure CI/CD Pipeline

A minimal, production-ready CI/CD pipeline with automated security scanning, vulnerability detection, and email alerts — deployed to GitHub Pages.

---

## 🗂 Project Structure

```
devops/
├── .github/
│   └── workflows/
│       └── ci.yml          # CI pipeline (build → scan → alert → deploy)
├── src/
│   └── index.html          # Webpage
└── README.md
```

---

## 🚀 Setup — Step by Step

### Step 1 — Create the GitHub Repository

1. Go to [github.com/new](https://github.com/new)
2. Name it `devops`
3. Set visibility to **Public** (required for free GitHub Pages)
4. Do **not** initialize with any files — push from local
5. Click **Create repository**

---

### Step 2 — Push This Project to GitHub

Run these commands in your terminal:

```bash
cd devops
git init
git add .
git commit -m "initial: devops ci pipeline"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/devops.git
git push -u origin main
```

> Replace `YOUR_USERNAME` with your actual GitHub username.

---

### Step 3 — Add GitHub Secrets

Go to your repo → **Settings → Secrets and variables → Actions → New repository secret**

Add these 3 secrets:

| Secret Name      | Value                                      |
|------------------|--------------------------------------------|
| `MAIL_USERNAME`  | Your Gmail address (e.g. you@gmail.com)    |
| `MAIL_PASSWORD`  | Your Gmail **App Password** (see below)    |
| `ALERT_EMAIL`    | Email to receive alerts (can be same)      |

#### How to get a Gmail App Password

1. Go to [myaccount.google.com/security](https://myaccount.google.com/security)
2. Enable **2-Step Verification** if not already on
3. Search for **App Passwords** in the search bar
4. Select App: **Mail** → Device: **Other** → name it `devops`
5. Copy the 16-character password → paste as `MAIL_PASSWORD`

---

### Step 4 — Enable GitHub Pages

1. Go to your repo → **Settings → Pages**
2. Under **Source**, select **Deploy from a branch**
3. Branch: `gh-pages` → Folder: `/ (root)`
4. Click **Save**

> The `gh-pages` branch is created automatically after the first successful pipeline run.

---

### Step 5 — Trigger the Pipeline

Push any change to `main` to run the full pipeline:

```bash
git commit --allow-empty -m "trigger: run pipeline"
git push
```

Or open a Pull Request — the pipeline also runs on PRs.

---

### Step 6 — View Results

| What               | Where                                                        |
|--------------------|--------------------------------------------------------------|
| Pipeline runs      | Repo → **Actions** tab                                       |
| Security report    | Actions run → **Artifacts → trivy-security-report**         |
| Live webpage       | `https://YOUR_USERNAME.github.io/devops`                     |
| Email alert        | Your inbox (check spam if not received)                      |

---

## 🔒 What the Pipeline Does

```
Push to main
     │
     ▼
[1] BUILD        — Checkout code, validate HTML
     │
     ▼
[2] SECURITY     — Trivy scan (CVEs), Gitleaks (secrets), OSSF Scorecard
     │
     ▼
[3] ALERT        — Send email: ⚠️ if vulns found, ✅ if clean
     │
     ▼
[4] DEPLOY       — Push src/ to GitHub Pages (main branch only)
```

---

## 🛠 Tools Used

| Tool            | Purpose                        |
|-----------------|-------------------------------|
| GitHub Actions  | CI/CD automation               |
| Trivy           | CVE & misconfiguration scanner |
| Gitleaks        | Secret/credential leak scanner |
| OSSF Scorecard  | Open-source security score     |
| action-send-mail| Email notification             |
| gh-pages action | Static site deployment         |

---

## ❓ Troubleshooting

**Email not received?**
- Check spam/junk folder
- Verify `MAIL_PASSWORD` is an App Password, not your login password
- Make sure 2FA is enabled on your Google account

**gh-pages branch missing?**
- The deploy job only runs on `push` to `main`, not on PRs
- Check the **Deploy** job in Actions for errors

**Trivy not finding anything?**
- This is expected for a simple HTML project — no npm packages or Docker images
- The scanner still runs and confirms clean status

**Pipeline fails on Scorecard?**
- Scorecard requires a public repo; it will silently skip on private repos
- `continue-on-error: true` ensures it won't block the pipeline

---

## 📄 License

MIT
