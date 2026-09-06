# Terminal Replacements Tracker

Live dashboard for tracking POS terminal replacements at Viva Wallet.

## 🔗 Live Dashboard
**[View Dashboard](https://YOUR_USERNAME.github.io/terminal-tracker/)**

## Architecture

```
Teams → lookup <ID>
  ↓
Power Automate → Query Dataverse → Show Adaptive Card
  ↓
User fills dropdowns → Submit
  ↓
Power Automate → Push to GitHub (data/replacements.json)
  ↓
GitHub Pages → Live dashboard auto-refreshes
```

## Setup

### 1. Create GitHub Repository
1. Create new repo: `terminal-tracker`
2. Upload `index.html` and `data/replacements.json`
3. Settings → Pages → Source: Deploy from branch → Branch: main → Save
4. Dashboard live at: `https://YOUR_USERNAME.github.io/terminal-tracker/`

### 2. Create GitHub Personal Access Token
1. GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token
3. Scopes: `repo` (full control)
4. Copy the token — you'll need it in Power Automate

### 3. Power Automate — Push data to GitHub
After the Teams Submit, add HTTP action:
- Method: `PUT`
- URI: `https://api.github.com/repos/YOUR_USERNAME/terminal-tracker/contents/data/replacements.json`
- Headers:
  - `Authorization`: `Bearer YOUR_GITHUB_TOKEN`
  - `Content-Type`: `application/json`
  - `Accept`: `application/vnd.github.v3+json`
- Body: (see flow guide for details)

## Data Format
Each replacement in `data/replacements.json`:
```json
{
  "id": "REP-001",
  "date": "2026-09-06",
  "accountName": "Customer Name",
  "company": "Company Title",
  "country": "Greece",
  "terminal": "SoftPos - abc123",
  "model": "CS50C",
  "reason": "Contactless",
  "priority": "Normal",
  "serialOld": "SN001",
  "serialNew": "SN002",
  "ticket": "CAS-04363832",
  "faultDetails": "Description",
  "status": "Pending",
  "submittedBy": "Name",
  "submittedAt": "2026-09-06T14:30:00Z"
}
```

## Features
- 📊 KPI cards (Total, Pending, Sent, Completed, VIP)
- 🌍 Chart by country
- 🔧 Chart by fault reason
- 📱 Chart by model
- 📋 Full data table with status pills
- 🔍 Country filter chips
- ↻ Auto-refresh every 30 seconds
