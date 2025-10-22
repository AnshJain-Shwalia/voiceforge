# AI-Powered Static Website Builder - Project Requirements

## Project Overview
A web application that allows users to create static websites through conversational AI (text + voice). Users interact with an AI assistant to build and refine their websites, with live preview and deployment to custom subdomains.

**Status**: MVP / Proof of Work

---

## 1. User Management & Authentication

### Authentication
- **Sign-in Method**: Google OAuth only
- **User Profile**:
  - Username (used for subdomain)
  - Email
  - Created date

### Dashboard
- List of all user's projects displaying:
  - Project name
  - Last modified date
  - Deployment status (draft/deployed)
  - Quick actions (open, delete, deploy)
- "Create New Project" button

---

## 2. Project Creation & Management

### New Project Flow
1. User clicks "Create New Project" from dashboard
2. Enters project name (must be URL-safe, unique per user)
3. Project initializes with blank canvas (optional: simple "Hello, World!" in h1 tags)
4. Opens in full preview mode

### Project Storage
- **File Structure**: Separate files (HTML, CSS, JS)
- **Asset Storage**: Images uploaded to server
- **No Version History**: Single current state only (MVP limitation)

---

## 3. Main Interface (Website Builder)

### Layout
- **Preview Pane**: Full-screen view of the website as end users would see it
- **Drawer Toggle**: Button/icon to slide in AI interface from the side

### Drawer Contents
- Text chat interface
- Voice interface (speak/listen controls)
- Code viewer (read-only, syntax highlighted)
  - Tabs or sections for HTML, CSS, JS
  - Collapsible/expandable

---

## 4. AI Interaction System

### Architecture
- **Backend LLM**: Claude or GPT-5 for decision-making and code generation
- **Voice Layer**: ElevenLabs for speech-to-text and text-to-speech
  - Voice agent acts purely as intermediary between user and LLM
  - No decision-making by voice agent

### Conversation Features
- **Multi-turn conversations** with full context memory
- AI remembers previous changes and references
- Text and voice available simultaneously
- User can switch between modes mid-conversation

### Voice Behavior
- When user speaks, transcribed text appears in chat
- AI voice response **only if user initiated with voice**
- If user types, AI responds with text only

### Rate Limiting
- No specific limits for MVP
- Consider adding in production version

---

## 5. Code Generation & Constraints

### Supported Technologies
- **Pure static HTML/CSS/JS only**
- No frameworks (no React, Vue, Tailwind, etc.)
- No external libraries via CDN (for MVP)

### Assets
- **Images**: Users can upload images to server
- **Forms**: Not supported (no form submissions)

### Safety & Moderation
- No content restrictions for MVP
- Consider adding for production

### Error Handling
- If AI generates broken code, user can request fixes through chat/voice
- Auto-save project state after each successful change

---

## 6. Deployment System

### Subdomain Structure
- **Format**: `projectname.username.myname.dev`
  - `projectname`: User-defined project name
  - `username`: User's account username
  - `myname.dev`: Your domain

### Deployment Strategy (Simple)
- **Storage**: Cloudflare R2 (S3-compatible)
- **Serving**: Fetch project files from R2 and serve to user
- **No HTTPS for MVP** (HTTP only to keep it simple)
- **No CDN/caching for MVP**

### Deployment Actions
- **Deploy**: Takes draft project and makes it live on subdomain
- **Redeploy**: Updates live site with latest changes
- **Unpublish**: Take site offline (optional for MVP)

### Limits
- **Multiple projects**: Users can deploy multiple projects simultaneously
- **No storage limits** for MVP
- **No bandwidth limits** for MVP
- **Subdomains only**: No custom domains

---

## 7. MVP Feature Checklist

### Must-Have (MVP)
- [x] Google OAuth authentication
- [x] User dashboard with project list
- [x] Create/delete projects
- [x] Full-screen preview pane
- [x] Drawer interface (toggle)
- [x] Text chat with AI (multi-turn, context-aware)
- [x] **Voice chat with ElevenLabs** (key feature)
- [x] AI generates HTML/CSS/JS via Claude/GPT
- [x] Code viewer (read-only, separate files)
- [x] Image upload support
- [x] Deploy to subdomain (projectname.username.myname.dev)
- [x] Redeploy updates

### Explicitly Out of Scope (MVP)
- Starter templates (blank canvas only)
- Version history/undo
- External libraries/frameworks
- Forms and form submission
- HTTPS/SSL certificates
- Custom domains
- Content moderation
- Rate limiting
- Collaboration/sharing
- Analytics
- Export as ZIP
- CDN/caching

### Nice-to-Have (Future Versions)
- Starter templates (landing page, portfolio, etc.)
- Version history with undo/redo
- Framework support (Tailwind, Bootstrap via CDN)
- Custom domains
- HTTPS/SSL
- Analytics on deployed sites
- Export project as ZIP
- Collaboration features
- Content moderation
- Rate limiting and resource quotas

---

## 8. Technical Architecture (High-Level)

### Frontend
- Technology: TBD (React/Vue/Svelte recommended)
- Components:
  - Dashboard
  - Website builder (preview + drawer)
  - Chat interface
  - Voice controls
  - Code viewer
  - Project settings

### Backend
- Technology: TBD (Node.js/Python recommended)
- Responsibilities:
  - User authentication (Google OAuth)
  - Project CRUD operations
  - AI request handling (proxy to Claude/GPT)
  - Voice processing (proxy to ElevenLabs)
  - Image upload handling
  - Deployment orchestration
  - Static file serving from R2

### Storage
- **User Data**: Database (PostgreSQL/MySQL/MongoDB)
- **Project Files**: Cloudflare R2
- **Images**: Cloudflare R2 or dedicated asset bucket

### External Services
- Google OAuth
- Claude API or OpenAI GPT API
- ElevenLabs API (speech-to-text + text-to-speech)
- Cloudflare R2

---

## 9. Open Questions & Future Decisions

1. **Auto-save frequency**: After every AI change, or on user action?
2. **Session timeout**: How long before inactive session expires?
3. **Project name validation**: Character limits, restricted words?
4. **Concurrent editing**: What happens if user makes request while AI is processing?
5. **Image optimization**: Auto-compress uploaded images?
6. **Deployment DNS**: Wildcard DNS setup for *.username.myname.dev?
7. **Error recovery**: How to handle failed deployments?

---

## 10. Success Metrics (Future)

For post-MVP evaluation:
- Number of projects created
- Number of successful deployments
- Voice vs text usage ratio
- Average conversation length
- User retention rate
- Most common AI requests

---

## Notes

- This is an **MVP/Proof of Work** - prioritize core functionality over polish
- Voice interaction is the **key differentiating feature**
- Keep deployment simple (HTTP + R2) to ship faster
- Focus on the conversational UX - the AI should feel natural and helpful
- Backend developer comfort: Strong / Frontend: Needs support
