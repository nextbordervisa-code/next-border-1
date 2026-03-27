# Project: NextBorder AI Platform
> Claude Code workspace for NextBorder — Bangladesh's AI-powered visa consultancy & IELTS preparation platform.

---

## 🏢 Project Overview

**NextBorder** is a dual-product platform:
1. **NextBorder Consultancy** — Student & work visa consultancy targeting Bangladeshi students (USA I-20, Canada, UK, Serbia)
2. **IELTS AI Tutor** — AI-powered IELTS practice app with avatar (Priya), band scoring, and WhatsApp result delivery

**Owner:** Ishakdawan (Founder, Dhaka, Bangladesh)
**Target Users:** Bangladeshi students seeking study abroad opportunities
**Primary Goal:** 1,000 students in 3 months

---

## 🛠️ Tech Stack & Architecture

### Frontend
- **HTML5 / CSS3 / Vanilla JS** — Landing pages, IELTS app UI
- **React (JSX)** — Widget components (Nora avatar, chat widgets, IELTS speaking widget)
- **Three.js (r128)** — 3D avatar rendering for Nora MetaHuman
- **HLS.js** — Video background streaming (Mux stream)
- **Web Speech API** — STT (Speech-to-Text) for IELTS speaking practice
- **Theme:** Navy (#1a2b5e) + Gold (#c9a84c) — Playfair Display / elegant serif

### Backend
- **Node.js / Express** — Voice IELTS tutor backend, WebSocket bridge
- **Google Apps Script** — Proxies GPT-4 and Gemini Pro for AI band scoring
- **edge-tts (en-US-AriaNeural)** — Text-to-speech for IELTS tutor
- **WebSocket** — Real-time bridge: React frontend ↔ Unreal Engine (MetaHuman lip sync)

### AI & APIs
- **Anthropic Claude API** — Core AI (Nora avatar, chat, tutoring)
- **Google Gemini Pro** — Band scoring fallback, content generation
- **GPT-4 (via Google Apps Script proxy)** — IELTS scoring
- **ElevenLabs TTS** — High-quality voice for Nora avatar
- **Simli.ai** — Real-time lip sync for Nora (replacing D-ID)

### Infrastructure
- **Netlify** — Primary deployment (TWO existing sites only — never create new)
  - `student-bd.netlify.app` → StudentBridge / Nora (Site ID: 4cc4de83)
  - `cosmic-licorice-867c51.netlify.app` → NextBorder IELTS / Priya (Site ID: 4df6aad9)
- **GitHub** — Source control
  - `dawantradelink-byte/studentbridge` → StudentBridge / Nora
  - `dawantradelink-byte/next-border` → IELTS app (GitHub Pages fallback)
- **Netlify Serverless Functions** — AI API proxies (Claude, Gemini, ChatGPT)

---

## 📁 File & Folder Conventions

```
nextborder/
├── CLAUDE.md                    ← This file (project memory)
├── .claude/
│   └── settings.json            ← Claude Code config
├── public/
│   ├── index.html               ← NextBorder landing page (navy/gold)
│   ├── nextborder-landing.html  ← Conversion landing page (built, not published)
│   └── ielts-practice.html      ← IELTS practice app
├── src/
│   ├── components/
│   │   ├── nora-avatar.jsx       ← SVG animated face (free, blink/emotions)
│   │   ├── nora-did-avatar.jsx  ← D-ID WebRTC version (credit exhausted)
│   │   ├── nora-demo-widget.jsx ← Nora demo UI
│   │   └── ielts-speaking-widget.jsx
│   ├── services/
│   │   ├── anthropic.js         ← Claude API calls
│   │   ├── elevenlabs.js        ← TTS service
│   │   └── simli.js             ← Simli.ai lip sync (next integration)
│   └── utils/
│       ├── speech.js            ← Web Speech API STT
│       └── scoring.js           ← IELTS band calculation
├── netlify/
│   └── functions/
│       ├── claude-proxy.js      ← Claude API serverless proxy
│       ├── gemini-proxy.js      ← Gemini API proxy
│       └── chatgpt-proxy.js     ← GPT-4 proxy
├── agents/
│   ├── ielts-tutor.yml          ← IELTS tutor agent definition
│   ├── visa-advisor.yml         ← Visa consultancy agent
│   └── nora-avatar.yml          ← Nora MetaHuman agent
├── skills/
│   ├── ielts-scoring/
│   │   └── SKILL.md
│   ├── visa-guidance/
│   │   └── SKILL.md
│   └── bangla-translation/
│       └── SKILL.md
├── .env.example                 ← Environment variable template
├── netlify.toml                 ← Netlify build config
└── README.md
```

---

## ⚙️ Coding Conventions & Rules

### General Rules
- ✅ Always write tests before code (where possible)
- ✅ Use conventional commits: `feat:`, `fix:`, `chore:`, `docs:`
- ✅ Never commit directly to `main` — use feature branches
- ✅ Run lint + typecheck before PR
- ✅ No secrets in code or logs — always use environment variables
- ✅ Validate all user inputs on both frontend and backend
- ✅ Use parameterized queries only (no raw SQL string concatenation)

### React / JSX
- Functional components only — no class components
- Default export for all page-level components
- Props with default values — no required props in widget components
- Use `useState`, `useEffect`, `useRef` hooks
- Tailwind core utilities only (no custom compiler needed)

### JavaScript
- ES6+ syntax always
- `async/await` over `.then()` chains
- Always handle errors with `try/catch`
- No `var` — use `const` and `let`

### API Calls
- All AI API calls go through Netlify serverless functions (never expose keys in frontend)
- Always include `max_tokens: 1000` for Claude API calls
- Model: `claude-sonnet-4-20250514` (always use Sonnet 4)

### Naming
- Files: `kebab-case.js` / `PascalCase.jsx` for React components
- Variables: `camelCase`
- Constants: `UPPER_SNAKE_CASE`
- CSS classes: `kebab-case`

---

## 🚀 Deployment Instructions (Netlify)

### ⚠️ CRITICAL NETLIFY RULES
```
❌ NEVER create a new Netlify site — namespace conflicts will occur
✅ ALWAYS deploy to one of these two existing sites:
   - student-bd.netlify.app      (Site ID: 4cc4de83) → Nora / StudentBridge
   - cosmic-licorice-867c51.netlify.app (Site ID: 4df6aad9) → IELTS / NextBorder
```

### Environment Variables
```bash
# When setting env vars via Netlify MCP — always use:
upsertEnvVar: True
newVarScopes: ['all']
```

### Required Environment Variables
```env
ANTHROPIC_API_KEY=sk-ant-...
ELEVENLABS_API_KEY=...
SIMLI_API_KEY=...                  # Get from simli.ai (free tier)
GEMINI_API_KEY=...
OPENAI_API_KEY=...
GITHUB_TOKEN=...
```

### Netlify Build Config (netlify.toml)
```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/api/*"
  to = "/.netlify/functions/:splat"
  status = 200
```

### Deploy Checklist
- [ ] Environment variables set in Netlify dashboard
- [ ] `netlify.toml` present in root
- [ ] No hardcoded API keys in any file
- [ ] Functions in `netlify/functions/` directory
- [ ] Test locally with `netlify dev` before pushing

---

## 🤖 AI Agents / Skills Structure

### Active Agents

#### 1. IELTS Tutor Agent (`ielts-tutor.yml`)
- **Persona:** Priya — Bengali female AI tutor
- **Voice:** edge-tts `en-US-AriaNeural`
- **Features:** 8-minute countdown timer, STT via Web Speech API, band scoring
- **Avatar:** Multi-sprite animated states (idle, speaking, listening, celebrating)
- **Lead capture:** WhatsApp result delivery, `leads.json` storage

#### 2. Visa Advisor Agent (`visa-advisor.yml`)
- **Scope:** USA I-20, Canada student visa, UK Tier 4, Serbia work visa (fraud detection)
- **Output:** Consultation booking, document checklist, fraud alert reports

#### 3. Nora MetaHuman Agent (`nora-avatar.yml`)
- **Persona:** Nora — South Asian female AI English teacher
- **Pipeline:** Claude API → ElevenLabs TTS → Simli.ai lip sync → React frontend
- **Status:** nora-avatar.jsx (SVG, FREE) ✅ | nora-did-avatar.jsx (D-ID, credits exhausted) ⚠️
- **Next:** Integrate Simli.ai (free real-time lip sync)

### Skills Directory

| Skill | Purpose | Auto-activates on |
|-------|---------|-------------------|
| `ielts-scoring/` | IELTS band calculation logic | "score", "band", "IELTS" |
| `visa-guidance/` | Visa document checklists | "visa", "I-20", "student permit" |
| `bangla-translation/` | Bangla ↔ English translation | "Bangla", "বাংলা" |

---

## 🧠 Nora MetaHuman Pipeline

```
User Speech (Mic)
       ↓
Web Speech API (STT)
       ↓
Claude API (claude-sonnet-4-20250514)
       ↓
ElevenLabs TTS (Audio Stream)
       ↓
Simli.ai (Real-time Lip Sync) ← NEXT INTEGRATION
       ↓
React Frontend (nora-avatar.jsx)
       ↓ [Optional advanced path]
WebSocket Bridge (nora-complete.js)
       ↓
Unreal Engine 5 (MetaHuman lip sync)
```

### Nora Integration Status
| Component | Status | Notes |
|-----------|--------|-------|
| `nora-avatar.jsx` | ✅ Complete | SVG face, blink, emotions, mouth |
| `nora-did-avatar.jsx` | ⚠️ Paused | D-ID credits exhausted |
| Simli.ai integration | 🔜 Next | Free tier, real-time lip sync |
| UE5 MetaHuman bridge | 🔜 Future | WebSocket + nora-complete.js |

### Simli.ai Setup (Next Step)
1. Go to [simli.ai](https://simli.ai) → Sign up → Get free API key
2. Share API key → Claude will build `simli-integration.jsx`
3. Deploy to `student-bd.netlify.app`

---

## 🔒 Security & Compliance

- No secrets in code or logs
- All API keys in Netlify environment variables
- Validate all user inputs (especially visa document uploads)
- Use parameterized queries only
- Never expose raw API responses to frontend
- Fraud detection: Serbia work visa document checker built and distributed

---

## 📊 Context Management

| Context Level | Action |
|--------------|--------|
| 0–60% | Work normally |
| 50–70% | Monitor usage |
| 70–80% | Run `/compact` |
| 80%+ | Clear mandatory |

---

## 🎯 Current Sprint Goals

- [ ] Publish `nextborder-landing.html` (navy/gold conversion page — BUILT, not live)
- [ ] Integrate Simli.ai into Nora avatar (free real-time lip sync)
- [ ] Activate Luna AI English teacher persona (bilingual Bangla/English)
- [ ] Deploy NextBorder IELTS app improvements to `cosmic-licorice-867c51.netlify.app`
- [ ] 1,000 student registrations in 3 months

---

## 📞 Key Links

| Resource | URL |
|----------|-----|
| StudentBridge / Nora | https://student-bd.netlify.app |
| IELTS Practice App | https://cosmic-licorice-867c51.netlify.app |
| GitHub (StudentBridge) | https://github.com/dawantradelink-byte/studentbridge |
| GitHub (NextBorder) | https://github.com/dawantradelink-byte/next-border |
| Simli.ai (next integration) | https://simli.ai |

---

*Last updated: March 2026 | Owner: Ishakdawan | Location: Dhaka, Bangladesh*
