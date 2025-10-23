# Phase 1: Core Foundation

## Overview
Phase 1 focuses on building the essential foundation of the AI-powered website builder, validating the core value proposition before adding advanced features like voice interaction and deployment.

## Scope

### Included in Phase 1
- **Authentication**: Google OAuth sign-in
- **User Management**: User profiles with username and email
- **Dashboard**: List all user projects with basic info (name, last modified, status)
- **Project Management**:
  - Create new projects
  - Delete projects
  - Open/view projects
- **Chat Interface**: Text-based AI interaction using Claude/GPT
  - Multi-turn conversations with context memory
  - AI generates HTML/CSS/JS based on user requests
  - Iterative refinement (user can ask for changes)
- **LLM Rate Limiting**: Implement rate limiting for AI requests to control costs and prevent abuse
- **Project Preview**: Full-screen preview pane showing live rendering of generated code
- **Code Viewer**: Read-only display of generated HTML, CSS, and JS files

### Explicitly Deferred (Future Phases)
- ElevenLabs voice integration (speech-to-text, text-to-speech)
- Deployment system (Cloudflare R2, subdomain hosting)
- Image upload functionality
- Advanced drawer UI (can use simpler layout for Phase 1)

## Technical Implementation Notes

### Frontend
- Preview pane with iframe/sandboxed rendering
- Simple chat interface (can be side-by-side with preview or simpler drawer)
- Code viewer with syntax highlighting (optional but nice)
- Dashboard with project cards

### Backend
- Google OAuth integration
- Database schema:
  - Users table (id, email, username, created_at)
  - Projects table (id, user_id, name, html, css, js, created_at, updated_at)
- API endpoints:
  - Auth: `/auth/google`, `/auth/callback`, `/auth/logout`
  - Projects: `/api/projects` (CRUD)
  - AI: `/api/chat` (proxy to Claude/GPT with project context)
- AI integration:
  - Proxy requests to Claude/GPT API
  - Maintain conversation context per project
  - Parse AI responses to extract code changes
  - Rate limiting implementation:
    - In-memory TokenUsageTracker class/struct that maps user ID to token usage
    - Per-user limits (e.g., X tokens per hour/day)
    - Return appropriate error messages when limits exceeded
    - Implementation details (reset windows, cleanup, etc.) can be refined later

### Storage (Phase 1)
- Store HTML/CSS/JS as text fields in database (simple, no R2 yet)
- Store conversation history in database for context

## Success Criteria

Phase 1 is complete when:
1. User can sign in with Google
2. User can create and manage multiple projects
3. User can chat with AI to generate/modify website code
4. Generated code displays correctly in preview pane
5. User can see the generated HTML, CSS, and JS code
6. Conversation maintains context across multiple turns

## Future Phases (Preview)

### Phase 2: Voice & Rich Interactions
- ElevenLabs integration (speech-to-text, text-to-speech)
- Improved drawer UI
- Image upload support

### Phase 3: Deployment & Hosting
- Cloudflare R2 integration
- Subdomain system (projectname.username.myname.dev)
- Deploy/redeploy/unpublish functionality

## Notes
- Focus on getting the AI interaction right - this is the core value
- Keep UI simple but functional in Phase 1
- Ensure conversation context works well (AI remembers previous changes)
- Test with various website generation scenarios (landing page, portfolio, etc.)
