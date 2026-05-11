diff --git a/README.md b/README.md
index 3d0fb2502306cf23453aa3a05d88d1b7a5ed9293..459ec61e0646abdbc3eafd1f2879c7cefe681c78 100644
--- a/README.md
+++ b/README.md
@@ -1,147 +1,53 @@
-# Wakuwaku Studio — Production Dashboard
+# AI Customer Service Agency Platform Demo
 
-Content production dashboard with Google Drive sync, handoff bundles, and audit logging.
+เว็บแอปตัวอย่างสำหรับโมเดล Agency/SaaS ที่ต่อยอดจากบริการ 1.1–1.6:
 
-**Live demo:** _Add your GitHub Pages URL here after deployment_
+1. Multilingual Chatbot
+2. Agent Assist
+3. Ticket Triage
+4. Call Summary & QA
+5. Knowledge Base
+6. Voice of Customer Analytics
 
----
+## จุดประสงค์
 
-## 🚀 Deploy on GitHub Pages (5 minutes)
+โปรเจกต์นี้ออกแบบให้เห็นภาพระบบที่ใช้งานได้จริงแบบครบลูป แยกมุมมอง **Internal Agency** และ **Client Portal** พร้อม backend API จำลองสำหรับทดสอบ flow ก่อนต่อ LLM, LINE OA, CRM, Google Cloud หรือ agent ตัวอื่น
 
-### Step 1 — Create a new GitHub repo
+## Run locally
 
 ```bash
-# In the folder containing index.html
-git init
-git add index.html README.md
-git commit -m "Initial deploy: Wakuwaku Studio v3"
-git branch -M main
+npm start
 ```
 
-Then create a new repo on [github.com/new](https://github.com/new) (name it whatever you like, e.g. `wakuwaku-studio`).
+เปิดเว็บที่ <http://localhost:8080>
 
-```bash
-git remote add origin https://github.com/YOUR_USERNAME/wakuwaku-studio.git
-git push -u origin main
-```
-
-### Step 2 — Enable GitHub Pages
-
-1. Go to your repo on GitHub
-2. **Settings** → **Pages** (left sidebar)
-3. Under "Build and deployment":
-   - Source: **Deploy from a branch**
-   - Branch: **main** / **/ (root)**
-4. Click **Save**
-
-After 1–2 minutes, your site will be live at:
-```
-https://YOUR_USERNAME.github.io/wakuwaku-studio/
-```
-
-### Step 3 — Update Google OAuth (CRITICAL)
-
-Google won't let your app login until you whitelist the GitHub Pages URL.
-
-1. Go to [console.cloud.google.com/apis/credentials](https://console.cloud.google.com/apis/credentials)
-2. Click your OAuth 2.0 Client ID (the one ending in `gbee5...rrei`)
-3. **Authorized JavaScript origins** → add:
-   ```
-   https://YOUR_USERNAME.github.io
-   ```
-   ⚠️ No trailing slash, no path — just the origin.
-4. Click **Save** (changes take ~5 min to propagate)
+## Download ZIP
 
-### Step 4 — Test
+หลังรัน `npm start` แล้วสามารถดาวน์โหลดไฟล์โปรเจกต์ ZIP ได้จากปุ่ม **Download ZIP** บนหน้าเว็บ หรือเปิด URL นี้โดยตรง:
 
-Open `https://YOUR_USERNAME.github.io/wakuwaku-studio/` and click **Continue with Google Workspace**. Should login normally now.
+<http://localhost:8080/download/ai-customer-service-agency-demo.zip>
 
----
-
-## 🔒 Make repo private (optional but recommended)
-
-If your CLIENT_ID is hardcoded in the file, anyone can see it. While CLIENT_ID isn't a secret (it's meant to be public), a private repo prevents random crawlers from hitting it.
-
-**Option A — Private repo + GitHub Pages:** Requires a paid plan (Pro/Team). Not free.
-
-**Option B — Public repo, restrict OAuth (free, recommended):** 
-- Keep the repo public
-- In Google Cloud Console, OAuth Consent Screen, set **User Type: Internal** (only allows users in your Google Workspace organization)
-- Or restrict by domain: only `@wakuwaku.co.th` emails can login
-
----
-
-## 🔄 Updating later
-
-Just edit `index.html`, then:
+## Checks ก่อน push
 
 ```bash
-git add index.html
-git commit -m "Update: <what changed>"
-git push
+npm run check
 ```
 
-GitHub Pages auto-deploys within ~1 minute.
-
----
-
-## 🛠 Local development
-
-To test changes before pushing:
-
-```bash
-# Python (built-in)
-python3 -m http.server 8080
-
-# Or Node
-npx serve -p 8080
-```
-
-Then add `http://localhost:8080` to Authorized JavaScript origins for local OAuth testing.
-
----
-
-## 📁 What's in the bundle
-
-When you click **Prepare Handoff** → **Finalize**, users get a ZIP with:
-
-```
-{ClientName}_Handoff/
-├── campaign-data.json   ← all assets, prompts, metadata
-├── activity-log.json    ← full audit trail
-├── final-report.pdf     ← formatted PDF report
-├── README.html          ← client-facing instructions (Thai)
-└── images/              ← reserved for future image storage
-```
-
-The bundle is **fully self-contained** — clients can re-import it anytime by dragging into the auth screen.
-
----
-
-## ⚙️ Configuration
-
-Key constants in `index.html` (around line 686):
-
-```js
-const CLIENT_ID = '165279376241-gbee5rudn234trve8fsm6ccl35kgrrei.apps.googleusercontent.com';
-const SAVE_DEBOUNCE_MS = 2500;  // batch saves to avoid Drive API spam
-const DB_FILENAME = 'wakuwaku_database.json';
-```
-
----
-
-## 🐛 Troubleshooting
+คำสั่งนี้ตรวจ syntax ของ backend และรัน automated tests ของ API หลัก
 
-| Problem | Fix |
-|---|---|
-| `redirect_uri_mismatch` | Add the exact URL (origin only, no path) to OAuth Authorized origins |
-| `idpiframe_initialization_failed` | OAuth Consent Screen not configured. Go to Cloud Console → OAuth consent screen → fill app info |
-| Login button does nothing | Check browser console. Usually means Google Identity Services hasn't loaded — refresh page |
-| Save indicator stays "Saving..." | Drive API might not be enabled. Cloud Console → APIs & Services → Library → search "Google Drive API" → Enable |
-| Bundle download fails | Browser blocked download. Allow downloads from your domain |
+## Backend APIs
 
----
+- `GET /api/health` — ตรวจสถานะ backend
+- `GET /api/workspace` — โหลด dashboard, tenants, integrations, KB และ sample tickets
+- `POST /api/chatbot/message` — โมดูล 1.1 chatbot
+- `POST /api/agent-assist/suggest` — โมดูล 1.2 agent assist
+- `POST /api/tickets/triage` — โมดูล 1.3 ticket triage
+- `POST /api/calls/qa` — โมดูล 1.4 call QA
+- `POST /api/knowledge-base/generate` — โมดูล 1.5 knowledge base
+- `POST /api/voc/analyze` — โมดูล 1.6 voice of customer
+- `POST /api/integrations/test` — ทดสอบแผนเชื่อม LINE, Google Cloud, CRM, BigQuery
+- `POST /api/agents/orchestrate` — ตัวอย่าง route งานไป agent อื่น
 
-## License
+## Architecture ที่แนะนำต่อ production
 
-© Wakuwaku Production Studio · Internal use only
+Frontend UI → Node.js API → LLM Gateway → PII/PDPA Redaction → Vector Search/KB → Google Cloud Speech/Storage/BigQuery → CRM/Helpdesk/LINE/Meta → Audit Log + Client Dashboard
