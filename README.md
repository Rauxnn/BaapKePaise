# BaapKePaise

Apple-inspired, mobile-first expense tracker with device-local storage and optional AI extraction from payment screenshots.

## What is included
- Responsive website UI and Android-ready Capacitor configuration.
- IndexedDB local storage for transactions and screenshot image data.
- Dashboard, history, category reports, delete, CSV export, JSON backup and restore.
- AI screenshot extraction endpoint using OpenAI vision. Extracted fields are editable and must be verified before saving.

## Important privacy details
- Expense records and saved screenshots are stored in the browser/app's local IndexedDB database.
- The AI scan feature sends only the selected screenshot to your configured server and AI provider for processing. Do not use it for screenshots you do not want to share with that provider.
- The server code does not write screenshots to disk, but requests may be processed/logged by the hosting provider and AI provider under their policies.
- Keep `OPENAI_API_KEY` only on the server. Never put it in frontend code or the Android APK.
- Clearing browser/app data or uninstalling the app may delete local records. Export backups regularly.
- The website and Android app have separate local databases. Use backup/restore to move records between them.

## Run locally
Requires Node.js 20+.

1. Install dependencies: `npm install`
2. Copy `.env.example` to `.env` and add your API key.
3. Start: `npm start`
4. Open `http://localhost:3000`

Without an API key the app UI and manual record entry work, but AI scanning returns a configuration error. For AI scanning, the app needs to be served through this Node server (or a compatible deployment of `server/index.js`).

## Deploy the website
Deploy the project to a Node.js host that supports Express. Set `OPENAI_API_KEY` as a server-side environment variable. Do not upload `.env` to GitHub. This is not a static-only site because screenshot scanning requires the server endpoint.

## Build Android app
You need Node.js and Android Studio with the Android SDK.

1. Install dependencies: `npm install`
2. Add Android wrapper: `npx cap add android`
3. Sync web assets: `npx cap sync android`
4. Open Android Studio: `npx cap open android`
5. In Android Studio, build a debug APK for testing or a signed APK/AAB for distribution.

For a production Android build, set the Capacitor server/origin strategy to your deployed HTTPS backend or bundle the web assets and configure a secure API base URL. The default `/api/scan-payment` uses same-origin requests, so when the Android app is served from its bundled assets you must update the frontend scan URL to your deployed HTTPS API endpoint before building. Do not hardcode any API secret in the app.

## Before public release
Add authentication if multiple people could access the hosted server, rate limiting, request-size limits at the host, a privacy notice, and a verified API deployment. The current app is a personal starter project, not a financial institution or bank integration.
