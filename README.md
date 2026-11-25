# YouTube AutoPilot AI

A deployable starter SaaS for YouTube creators: account registration/login, channel OAuth connection, content project tracking, AI metadata generation (optional OpenAI key), video file upload, immediate YouTube publishing, scheduled publishing worker, and creator-credit wallet demo.

## Included
- Responsive dashboard, login/register, content studio, publishing queue, wallet and settings-style navigation.
- SQLite persistence for users, connected channel tokens, projects and demo credit ledger.
- Google OAuth 2.0 with YouTube upload/read-only scopes.
- YouTube Data API upload and scheduled publishing worker (runs every minute).
- OpenAI metadata generation if `OPENAI_API_KEY` is configured; otherwise a clearly labeled fallback template.
- Dockerfile and Render blueprint.

## Run locally

1. Install Node.js 20+.
2. Copy `.env.example` to `.env` and set a strong `SESSION_SECRET`.
3. `npm install`
4. `npm start`
5. Open `http://localhost:3000`.

## Enable YouTube connection

In Google Cloud Console create a project, enable **YouTube Data API v3**, configure the OAuth consent screen, create an OAuth Web client, and add the exact redirect URI:

`https://YOUR-DOMAIN/auth/youtube/callback`

Set `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `APP_URL`, and `GOOGLE_REDIRECT_URI` in your host's environment variables. OAuth app verification / testing-user restrictions may apply. Never commit `.env` or secrets to GitHub.

## Deploy

This app needs a persistent disk/volume for `data/` and `uploads/`, and a single active scheduled worker instance. Use Render or another Node host with persistent storage. `render.yaml` is a starting blueprint; set secrets in the host dashboard. GitHub Pages and static-only hosting will NOT run this backend. Vercel serverless is not a direct fit for local SQLite, large video uploads, or a persistent cron worker without external services.

## Current limits / honest scope

- Users upload a finished video file. This starter does not synthesize/edit full videos or AI voiceover itself; connect a video-generation/editing provider to extend it.
- Thumbnail generation is planned UI workflow; upload/thumbnail rendering and advanced editor are not yet implemented in this starter.
- Wallet has demo credits only. No real recharge/payment gateway is connected.
- Metadata endpoint creates title, hook, description and tags; users should review for accuracy, copyright and advertiser-friendly content.
- Scheduled upload uses a single worker and uploads at the scheduled time. YouTube API quota, channel permissions and OAuth verification apply.
- For production use, add CSRF protection, rate limits, secure session store, per-user upload quotas, malware scanning, token encryption at rest, job queue, retry/dead-letter handling, and billing before accepting customers.

## GitHub
Upload the project folder contents to a repository. Keep `.env` excluded. Then connect the repository to your Node hosting provider and configure the environment variables and persistent disk.
