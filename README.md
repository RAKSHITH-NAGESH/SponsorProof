# SponsorProof

SponsorProof is a local sponsor and agency workspace for tracking public Twitch channels. Add a streamer by Twitch login, sample public live viewer counts, record a sponsored segment, and create a report from the samples collected.

## Requirements and setup

1. Install Node.js 22.13 or newer.
2. In PowerShell, from this project folder, run:

```powershell
Copy-Item backend\.env.example backend\.env
notepad backend\.env
```

3. Add your Twitch Client ID and Client Secret to `backend/.env`. Keep the secret private and do not commit or share the file.
4. Install and start the app:

```powershell
npm install
npm install --prefix backend
npm run dev
```

5. Open the local Vite URL printed in PowerShell and leave the terminal running. Do not open `index.html` directly. If the printed port is 5174 or 5175, open that port.

## Use SponsorProof

1. Add a public channel in **Streamer roster**.
2. SponsorProof checks live status and samples public viewer counts every 30 seconds.
3. For a full pre-segment baseline, track the streamer for five minutes before the sponsor placement.
4. In **Live monitor**, choose the streamer and sponsor, then start and end the segment at the actual placement times.
5. Wait five minutes after the segment for the recovery window, then open **Reports** to review, delete, or print/save the report as PDF.

Reports use only samples SponsorProof collected. Each window needs at least three real samples. Retention compares the active average to the pre-segment median; recovery compares the post-segment average to the same baseline. These measurements do not prove a sponsor caused audience changes.

## Limits

- This is a local workspace on one computer; it is not an online multi-user service.
- Twitch public viewer counts can be sampled without signing in as the broadcaster. Chat messages and private analytics require streamer authorization and are not collected.
- Historical viewer counts cannot be backfilled.
- OBS is a local preview, not a hosted browser source.

The backend stores workspace records in `backend/sponsorproof-agency.sqlite`.

## Sponsorship helper

The **Sponsorship helper** is an additional section alongside the existing Twitch monitor, analytics, roster and reports.

- Creators can save a profile with platform, channel handle, category, self-reported audience, starting rate, description and contact email.
- Small businesses can post an opportunity with a category, supported platforms, audience range, budget, brief and contact email.
- SponsorProof sorts creator/opportunity pairs using a transparent fit score: platform (30), category (30), audience range (25), and budget covering the creator's starting rate (15).
- Each match can produce an editable creator pitch and a prefilled business invitation email. The user reviews and sends it from their email app; SponsorProof does not send emails.

This helper works without Twitch credentials. Its profiles and opportunities are stored on this computer and are not visible to other people online until the app has secure accounts and hosting.

For the full feature, setup, workflow and limitation guide, see [SPONSORPROOF_COMPLETE_GUIDE.md](SPONSORPROOF_COMPLETE_GUIDE.md).

For a step-by-step illustrated walkthrough, see the [SponsorProof Complete Workflow Guide (PDF)](output/pdf/SponsorProof_Complete_Workflow_Guide.pdf).

## Optional: deploy a password-protected Render preview

A Render deployment template is included in `render.yaml`. It is configured as a **private pilot**, not an open public marketplace: one shared username and password protect the whole workspace. Anyone with that shared login can view, add, or delete workspace records. Do not use it with outside users who expect individual accounts or private profiles.

Render needs this project in a GitHub repository. The repository must not contain `backend/.env` or the SQLite database; both are excluded by `.gitignore`. In Render, create a new Blueprint from the repository and enter the requested `APP_USERNAME`, `APP_PASSWORD`, `TWITCH_CLIENT_ID` and `TWITCH_CLIENT_SECRET` values in the dashboard. Render will build and start the service using `render.yaml`.

This template uses a paid web service and a persistent disk so the SQLite database survives restarts and redeploys. Review Render's current plan and disk charges before creating the service. A public multi-user launch still needs individual accounts, ownership and privacy protections.
