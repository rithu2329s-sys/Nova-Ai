# NOVA AI Ultimate Edition — Windows beginner guide

இந்த project உங்களுடைய `NOVA_AI_Starter.zip`-ஐ அடிப்படையாகக் கொண்டு மேம்படுத்தப்பட்டது. இது **Hybrid AI**: உங்கள் PC-யில் Ollama local model-ஐ முதலில் பயன்படுத்தும்; அது கிடைக்காவிட்டால், server-ல் configure செய்த OpenAI-compatible cloud model-க்கு fallback செய்யும்.

## இந்த build-ல் உள்ளவை
- Premium responsive NOVA interface; mobile sidebar; chat history browser-ல் சேமிப்பு.
- Local Ollama + optional cloud API routing (`auto`, `local`, `cloud`).
- API keys frontend-ல் இல்லை; server `.env`-ல் மட்டும்.
- Helmet security headers, JSON size limit, API rate limit, message validation, optional access token.
- Browser speech-to-text (browser support தேவை).
- PWA manifest + service worker: HTTPS deployment-ல் installable web app.
- Health endpoint: `/api/health`.

## 1) Install
1. Node.js 20 LTS அல்லது newer: https://nodejs.org/
2. Ollama for Windows: https://ollama.com/download
3. ZIP extract செய்யவும்; `NOVA_AI_Ultimate` folder open செய்யவும்.
4. Folder address bar-ல் `cmd` type செய்து Enter.
5. Run:
   ```bash
   npm install
   ollama pull qwen2.5:3b
   copy .env.example .env
   npm start
   ```
6. Browser: http://localhost:3000

16 GB RAM-க்கு `qwen2.5:3b` ஒரு starting model மட்டுமே. பெரிய model-கள் அதிக RAM/VRAM மற்றும் நேரம் தேவைப்படலாம்.

## 2) Hybrid mode settings
`.env` file-ஐ Notepad-ல் open செய்யவும்:
```env
AI_MODE=auto
OLLAMA_URL=http://127.0.0.1:11434
OLLAMA_MODEL=qwen2.5:3b
CLOUD_API_URL=
CLOUD_API_KEY=
CLOUD_MODEL=
```
- `auto`: local model ready என்றால் local; இல்லையெனில் cloud fallback.
- `local`: Ollama மட்டும்.
- `cloud`: cloud மட்டும்.
- Cloud use செய்ய provider account/API key தேவை; usage charge இருக்கலாம். Provider endpoint, model name, key ஆகியவை provider documentation-ல் சரிபார்க்கவும்.
- `.env` save செய்த பிறகு server restart செய்யவும் (`Ctrl+C`, பின்னர் `npm start`).

OpenAI-compatible endpoint உதாரணம்:
```env
CLOUD_API_URL=https://api.openai.com/v1/chat/completions
CLOUD_API_KEY=உங்கள்_API_KEY
CLOUD_MODEL=உங்கள்_கணக்கில்_கிடைக்கும்_model_name
```
API key-ஐ யாரிடமும் share செய்யாதீர்கள்; GitHub-ல் `.env` upload செய்யாதீர்கள்.

## 3) Optional access token
Public deployment-க்கு முன் `NOVA_ACCESS_TOKEN`-ஐ நீளமான random secret-ஆக set செய்யலாம். ஆனால் இந்த starter UI token-entry screen வழங்கவில்லை; அதை frontend-க்கு hardcode செய்யக்கூடாது. Production login/user system தனியாக உருவாக்க வேண்டும். Local-only development-ல் token blank-ஆக இருக்கட்டும்.

## 4) Public hosting / all platforms — முக்கியமான உண்மை
- **Local PC:** Ollama local model நேரடியாக இயங்கும்.
- **Public website:** Netlify static hosting மட்டும் Node backend/Ollama-ஐ இயக்காது. Backend-ஐ Node.js hosting/server-ல் deploy செய்ய வேண்டும். Public users-க்கு local Ollama உங்கள் PC-யில் இருப்பதால் தானாக access கிடைக்காது; cloud provider அல்லது பாதுகாப்பான hosted inference backend தேவை.
- **Android/iPhone:** HTTPS website-ஐ supported browser-ல் PWA ஆக install செய்யலாம். இது native APK/IPA அல்ல. Store distribution-க்கு பின்னர் Capacitor/native build, signing, store accounts, privacy disclosures தேவை.
- **Offline:** App shell cache ஆகும்; AI responses offline-ல் உருவாகாது.

## 5) Security boundaries
இந்த version-ல் practical baseline hardening உள்ளது; இது “100% secure” அல்லது “1000x secure” guarantee அல்ல. இது இன்னும் production-grade multi-user authentication, database encryption, account recovery, admin RBAC, audit pipeline, malware scanner, WAF, DDoS protection, automated backups, penetration testing ஆகியவற்றைக் கொண்டிருக்கவில்லை. PC commands/file access/remote-control சேர்க்கப்படவில்லை. Public-ஆக expose செய்வதற்கு முன் HTTPS reverse proxy, strong authentication, secret manager, provider quotas, monitoring, backups, dependency updates மற்றும் security review சேர்க்கவும்.

## 6) Troubleshooting
- `npm is not recognized`: Node.js install செய்து terminal reopen செய்யவும்.
- Local model unavailable: Ollama app running இருக்கட்டும்; `ollama list`; `ollama pull qwen2.5:3b`.
- Cloud fallback unavailable: `.env` URL/key/model மூன்றும் சரியா பார்க்கவும்; server restart செய்யவும்.
- Port busy: `.env`-ல் `PORT=3001` set செய்து restart.
- Mobile install: deployed site HTTPS-ல் இருக்க வேண்டும்; browser menu-ல் Install app / Add to Home Screen தேர்வு செய்யவும்.

## Folder structure
```text
NOVA_AI_Ultimate/
  server.js
  package.json
  .env.example
  README_TAMIL.md
  public/
    index.html
    styles.css
    app.js
    manifest.json
    icon.svg
    sw.js
```
