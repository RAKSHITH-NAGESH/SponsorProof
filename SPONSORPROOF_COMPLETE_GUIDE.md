# SponsorProof — Complete Project Guide

SponsorProof combines two tools in one local application:

1. **Twitch campaign measurement** for tracking public live viewer counts before, during and after a sponsor segment.
2. **Sponsorship helper** for creators and small businesses to describe what they offer or need, see potential fits, and prepare an email introduction.

The two areas are available from the same sidebar. The sponsorship helper is an addition to SponsorProof; it does not replace the original analytics, live monitor, streamer roster, reports, OBS preview or settings.

## What SponsorProof does

### Twitch campaign measurement

- Looks up public Twitch channels by login.
- Checks whether tracked channels are live.
- Samples public concurrent viewer counts every 30 seconds while the backend is running.
- Lets you mark when a sponsor placement begins and ends.
- Calculates viewer retention and post-segment recovery from collected samples.
- Shows actual saved samples in analytics and reports.
- Lets you delete a segment from **Reports**.

Twitch credentials are needed for this area. You do not need to sign in as the broadcaster to look up public channels or public live viewer counts.

### Sponsorship helper

Creators can save a profile containing:

- Creator or channel name, platform and handle
- Content category
- Self-reported audience size
- Starting sponsorship rate in Indian rupees
- Description of the content
- Contact email

Small businesses can post an opportunity containing:

- Business name and contact email
- Campaign title and category
- Platforms they are interested in
- Minimum and maximum audience size
- Budget in Indian rupees
- Campaign brief and expectations

SponsorProof ranks creator and opportunity pairs with a transparent fit score:

| Match factor | Points |
| --- | ---: |
| Platform match | 30 |
| Category match | 30 |
| Audience range fit | 25 |
| Opportunity budget covers the creator’s starting rate | 15 |

The score is a sorting aid, not a guarantee that a partnership will succeed. Audience sizes, rates and briefs are entered by users; SponsorProof does not independently verify them.

For each match, users can prepare an editable creator pitch or a prefilled invitation email. SponsorProof opens an email draft in the user’s mail app; it does not send email automatically. The pitch text is a template filled from the profile and opportunity details, not an AI-generated recommendation.

## What the app does not do

- It does not fabricate creator profiles, audience counts, sponsor opportunities or campaign results. New lists start empty.
- It does not collect Twitch chat messages. Chat-based sponsor mention counts would require Twitch chat authorization from the streamer or an authorized bot/moderator setup.
- It cannot backfill viewer counts from before a channel was added and sampling began.
- It does not prove that a sponsor placement caused viewer changes.
- It does not automatically negotiate sponsorship terms, verify payment, or send emails.
- It is not currently an online public marketplace. Profiles and opportunities are local to the computer running the app.

## Requirements

- Windows PowerShell (the commands below are written for PowerShell)
- Node.js 22.13 or later
- Twitch Developer Application credentials for **Live measurement**, **Streamer roster**, Twitch viewer analytics and Twitch reports

The sponsorship helper itself does not require Twitch credentials.

## First-time setup

### 1. Open the project folder in PowerShell

Use the folder that contains `package.json`:

```powershell
cd "C:\Users\Rakshithn\Documents\Codex\2026-11-03\c\outputs\SponsorProof"
```

### 2. Create the backend environment file

```powershell
Copy-Item backend\.env.example backend\.env
notepad backend\.env
```

Add the Twitch Client ID and Client Secret to `backend/.env`. Keep the secret private. Do not paste it into public chats or commit the file to a repository.

### 3. Install dependencies and start both services

```powershell
npm install
npm install --prefix backend
npm run dev
```

Leave the PowerShell window open while using SponsorProof. The terminal prints the local web address. Open that exact address in your browser; Vite may choose `5173`, `5174` or another available port. Do not double-click `index.html` or use a `file:///` address.

To stop the services, focus the terminal and press **Ctrl+C**. Start them again with `npm run dev` from the project folder.

## Recommended workflow: measure a Twitch sponsor segment

1. Open **Streamer roster** and add a public Twitch channel login.
2. Keep SponsorProof running. It checks live status and samples viewer counts every 30 seconds.
3. For a complete pre-segment baseline, add the channel and let it collect for at least five minutes before the placement.
4. Open **Live monitor**, select the channel, enter the sponsor or brand, then select **Start sponsor segment** at the placement start.
5. Select **End segment** when the placement finishes.
6. Keep SponsorProof running for another five minutes to collect the post-segment recovery window.
7. Review the segment in **Analytics** or open **Reports** to print/save a PDF. Use **Delete segment** in Reports if you need to remove a segment record.

At least three samples are required in each measurement window. If the channel was added too late, the stream was offline, or SponsorProof was stopped, values may remain unavailable.

## How the viewer metrics are calculated

- **Baseline:** median viewer count in the five minutes before the segment starts.
- **Active average:** average viewer count while the segment is active.
- **Post average:** average viewer count in the five minutes after the segment ends.
- **Retention:** active average divided by baseline, expressed as a percentage and capped at 100%.
- **Recovery:** post average divided by baseline, expressed as a percentage.

These describe viewer-count changes in the collected samples. They do not establish causation or guarantee the number of people who noticed the sponsor.

## Recommended workflow: find sponsorship opportunities

1. Open **Sponsorship helper** in the sidebar. This section works without Twitch credentials.
2. In **For creators**, add a real profile with a clear description, audience size, starting rate and contact email.
3. In **For small businesses**, post an opportunity with an actual budget, audience range, platforms and campaign expectations.
4. Review **Potential partnerships** and the reasons shown for each match.
5. Select **Create creator pitch** to edit a creator’s pitch, copy it, or open it in an email draft. A business can also open a prefilled invitation email to the creator.
6. Review the details and send the email yourself if you choose.

Creator profiles and sponsor opportunities are saved in the local database. They will not be visible to other people on the internet until the app is deployed as a shared service with accounts and secure access controls.

## Data storage

The backend uses SQLite. Its local database file is:

```text
backend/sponsorproof-agency.sqlite
```

It stores the Twitch roster, viewer samples, sponsor segment records, creator profiles and sponsor opportunities. Keep a backup if you need to preserve local records. Do not delete the database file unless you intend to remove its stored data.

The app stores contact emails for sponsorship helper profiles so it can prepare `mailto:` drafts. This is a local prototype without accounts or profile ownership. Do not expose it as a public service until account security, profile ownership, contact privacy, abuse reporting and moderation are implemented.

## Troubleshooting

### `npm` says it cannot find `package.json`

PowerShell is in the wrong directory. Run `cd` into the SponsorProof project folder shown in the setup section, then run `npm run dev`.

### The browser page is blank or opened a file path

Do not open `index.html` directly. Run `npm run dev` and open the `http://localhost:...` address printed by Vite.

### Twitch says “API not configured”

Check that `backend/.env` contains the correct Twitch Client ID and Client Secret. Restart SponsorProof after editing the file.

### The live monitor says the channel is offline

Check the Twitch login and wait for the next refresh. SponsorProof checks status periodically; the creator must be live for a segment to start.

### A report says “Insufficient samples”

SponsorProof needs at least three real samples in the relevant window. Continue tracking before and during the segment, and keep the backend running after the segment for the recovery window.

### Port 5173 is already being used

Vite may print another port such as `5174`. Open the URL that Vite prints rather than assuming it will always use `5173`.

## Project files

- `src/main.jsx` — React pages and browser-side interactions
- `src/style.css` — application styling
- `backend/server.js` — Express API, Twitch integration and SQLite storage
- `backend/.env.example` — environment variable template
- `vite.config.js` — Vite development server and API proxy
- `README.md` — short setup and feature overview

## Future work before a public launch

A public, multi-user version needs deployment with HTTPS, user accounts, ownership rules for profiles and campaigns, private contact details, moderation and reporting, rate limiting, production email, backups and a process for handling fraud or misleading listings. Chat mention measurement needs a separate streamer-consent flow and should be designed to retain aggregate counts rather than raw chat content.

## Optional live deployment preview

A `render.yaml` Blueprint is included for a password-protected Render preview. It builds the Vite frontend and starts the Express backend as one web service. A persistent disk is mounted at `/var/data`; `DATABASE_PATH` stores the SQLite file there so the database survives deploys.

The deployment uses HTTP Basic Authentication with `APP_USERNAME` and `APP_PASSWORD` in production. Render's `/api/health` check remains accessible without a login. The same shared credentials protect the whole app; they do not create individual creator or business accounts. Everyone with the password shares the workspace and can manage its records. Use this only for a private preview with people you trust.

To deploy, first push the project to a GitHub repository without `backend/.env` or any database file. Then create a Render Blueprint from that repository, review the plan and persistent-disk charges, and provide the four prompted secrets: `APP_USERNAME`, `APP_PASSWORD`, `TWITCH_CLIENT_ID`, and `TWITCH_CLIENT_SECRET`. Check Render's current docs for [Express deployment](https://render.com/docs/deploy-node-express-app), [environment secrets](https://render.com/docs/configure-environment-variables), and [persistent disks](https://render.com/docs/disks).

A true public marketplace requires a larger follow-up: separate accounts, profile ownership, private contact access, deletion permissions, abuse reporting and moderation. The shared-password preview is not a replacement for those controls.
