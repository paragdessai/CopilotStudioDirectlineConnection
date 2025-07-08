# CopilotStudioDirectlineConnection
Connect to copilot studio agent with SSO using directline API's
# Copilot Studio Web Chat (Pure-HTML Demo)

> One self-contained HTML page  
> • Microsoft Entra sign-in via **MSAL.js**  
> • Conversation UI via **Bot Framework Web Chat**  
> • Connects to a Copilot Studio (Bot Framework) agent through a **Direct Line** secret  

---

## ‼️‼️‼️ For Demo Only. Do not use copilot studio secret directly in production.

If you publish `index.html` with GitHub Pages or any static host, add the link here.

---

## 1️⃣ What you get

| Feature | Tech |
|---------|------|
| **No build step** | Plain HTML + vanilla JS – drop into any web server |
| **Microsoft sign-in** | MSAL browser v2.35 (popup) |
| **Friendly greeting** | Shows signed-in UPN on the page |
| **Short userID guard** | Trims `userID` to ≤ 64 chars to satisfy Direct Line |
| **Custom chat theme** | Blue user bubbles, gray bot bubbles (adjust CSS easily) |

---

## 2️⃣ Prerequisites

| Tool / resource | Why |
|-----------------|-----|
| **Direct Line secret** | Authorizes the browser to talk to your Copilot Studio agent. Get one per environment/agent from **Copilot Studio ➜ Settings ➜ Channels ➜ Direct Line ➜ “Show”**. |
| **Microsoft Entra ID tenant** | Users will sign in here. |
| **SPA app registration** | MSAL needs a client ID & redirect URI. (Steps below.) |
| **Any web server (or VS Code Live Preview)** | To serve `index.html`. A file URL works for quick tests but some browsers block pop-ups. |

---

## 3️⃣ Create / configure the Entra SPA app

1. Azure portal ➜ **Microsoft Entra ID ➜ App registrations ➜ New registration**  
2. **Name:** *CopilotChat-SPA*  
3. **Platform:** *Single-page application* ➜ Redirect URI `http://localhost:5000/getAToken` (must match the one hard-coded in `index.html`, line 17).  
4. **API permissions ➜ Add a permission ➜ Microsoft Graph ➜ Delegated ➜ `User.Read`** ➜ **Add** ➜ **Grant admin consent**.  
5. Copy the **Application (client) ID** – paste it into `clientId` in the HTML (`index.html`, line 34).  
6. (Optional) If you deploy to production, add that origin as an extra redirect URI.

---

## 4️⃣ Replace placeholder values

Open **index.html** and edit:

| Placeholder | Replace with |
|-------------|--------------|
| `clientId` (line 34) | Your SPA’s **Application (client) ID** |
| `authority` (line 35) | `https://login.microsoftonline.com/<YOUR_TENANT_ID>` or keep `common` if multi-tenant |
| `redirectUri` (line 36) | Must match one of the SPA redirect URIs (dev = `http://localhost:5000/getAToken`) |
| **DirectLine `secret`** (line 59) | Your own direct-line secret (32+ char string) |

> **Important:** A Direct Line secret is effectively an “app password.”  
> Share it only inside secure repos or use a **server-generated short-lived token** for production.

---

## 5️⃣ Run locally

```bash
# Serve index.html from the repo root on port 5000
npx serve . --listen 5000
# or, if you have Python 3 installed
python -m http.server 5000
