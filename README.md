# 5S Management System — Offline-first PWA prototype

## What this prototype does
- Runs as a responsive web application on desktop and mobile.
- Uses IndexedDB for local device storage.
- Provides zone registration, 5S audit capture, findings/action tracking, ideal-condition photo storage, performance summaries and printable board reports.
- Exports/imports a complete JSON backup and exports audit CSV.
- Caches the application shell using a service worker for offline reopening after the first successful online visit.

## Critical limitations
This is a working local-first prototype, not yet a multi-user centralised production system.
- Each browser/device has its own local database.
- There is no automatic cross-device synchronisation or central corporate database.
- JSON backup/import is a controlled manual transfer mechanism.
- Browser storage can be cleared by users, device management or browser cleanup. Export backups regularly.
- Offline capability requires the app to be served from HTTPS (or localhost during development). Opening index.html directly from a file manager is not sufficient for PWA service workers.
- SharePoint embedding depends on tenant settings, iframe/embed policy, authentication and allowed-domain configuration. A site owner cannot override tenant restrictions.
- OneDrive sharing links are not static web hosting and should not be relied upon for service-worker/PWA deployment.

## Quick local test
1. Install Python 3 on a computer.
2. Open a terminal in this folder.
3. Run: `python -m http.server 8080`
4. On that computer open `http://localhost:8080`.
5. For phone testing on the same Wi-Fi, use the computer's LAN IP and port. Service-worker installation generally requires HTTPS on phones; use a proper HTTPS host for real installation.
6. Create a zone, run an audit, add actions and an ideal-condition photo.
7. Test export and import from Settings & Data.

## Deploying
Upload the files to a static HTTPS host that supports serving the files unchanged, including `sw.js` at the application root. Suitable options include GitHub Pages or another approved static web host. Configure the hosting path correctly if deploying under a project subpath.

## Embedding in SharePoint later
1. Deploy the app to HTTPS.
2. Test that its URL opens directly and that the service worker registers.
3. In SharePoint, try the Embed web part using the app URL.
4. If embedding is blocked, the tenant administrator must allow the domain / iframe scenario. This cannot be bypassed by uploading files to OneDrive.
5. The app remains usable as a standalone PWA even if embedding is unavailable.

## Production design required for multi-site operation
Before corporate rollout, add:
- Authenticated central API or a properly secured Microsoft 365 integration.
- Central database with unique audit/action IDs and conflict handling.
- Per-user identity and role-based access control.
- Reliable offline sync queue, retry logic, duplicate prevention and sync status.
- Server-side audit trail and immutable submitted audit records.
- Backup, retention, privacy and device-loss procedures.
- Versioned checklist configuration and migration handling.
- Image upload/compression and central storage.
- Corporate reporting data model.

Do not store confidential personnel or sensitive operational data in an unauthenticated public static application.
