# SponsorProof --- Project Handoff & Codex Build Brief

> Purpose: This file is the working handoff for building and improving
> SponsorProof in VS Code with OpenAI Codex. Keep it in the project root
> as `SPONSORPROOF_PROJECT_BRIEF.md`.
>
> Product direction: Build a credible, polished SaaS product. The
> interface must feel intentionally designed by a professional product
> team---not like a generic AI-generated dashboard.

------------------------------------------------------------------------

## 1. Project summary

**SponsorProof** is a Twitch sponsorship analytics platform. It helps
streamers, agencies and sponsors understand how sponsored segments
affect an audience and communicate campaign performance with clear
reports.

The core product idea is to measure audience behaviour around a
sponsorship segment:

1.  A streamer or moderator starts a sponsor segment.
2.  SponsorProof records the segment start and end.
3.  The platform compares audience measurements before, during and after
    the segment.
4.  It calculates retention and recovery metrics.
5.  It presents campaign analytics and a sponsor-ready report.

The first release is Twitch-only. Do not expand the product into other
streaming platforms unless the project owner explicitly changes the
scope.

## 2. Main users

-   **Streamer:** connects a Twitch account, starts and ends sponsor
    segments, and reviews performance.
-   **Agency:** compares performance across a small roster of streamers.
-   **Sponsor / brand:** reviews a concise report showing how a
    sponsored segment performed.

## 3. Core features and priority

### Must-have product features

1.  Twitch account connection (OAuth).
2.  Sponsor segment start/end tracking.
3.  Viewer measurement around the segment.
4.  Retention Score.
5.  Recovery metric.
6.  Chat-active retention.
7.  Brand-safety status indicator.
8.  One-page sponsor PDF report.

### Secondary features

-   Agency streamer roster and comparison.
-   OBS browser-source overlay.
-   Tracked campaign links/clicks, if time allows.

### Scope boundaries

Do not spend time on these for the first version:

-   Mobile applications.
-   Other streaming platforms.
-   Billing and subscription management.
-   Advanced AI sentiment analysis.
-   Automatic sponsor-segment detection.
-   Claims of real-time measurement when the data is simulated or
    sampled.

If time is short, prioritize in this order: segment tracking and
retention calculations, sponsor report, brand-safety indicator, roster,
then tracked links.

## 4. Measurement model

A sponsor segment is measured in three phases:

-   **Pre-segment:** five minutes before the sponsorship begins. This is
    the baseline.
-   **Active segment:** while the sponsorship is running.
-   **Post-segment:** five minutes after the sponsorship ends.

### Retention Score

Use the project definition:

`Retention Score = average audience during sponsorship / median pre-segment baseline × 100`

Cap the displayed score at 100% if that is the agreed product rule. Do
not silently change the calculation.

### Recovery

`Recovery = post-segment average audience / pre-segment baseline × 100`

### Chat-active retention

Compare active chatters before and during/after the segment according to
the final agreed product definition. The exact formula must be confirmed
against the project requirements before implementation. Do not invent a
formula and present it as approved.

### Brand safety

The first version should use a simple, explainable status (Safe / Watch
/ Risk) based on basic chat-rate and flagged-language signals. It is not
advanced AI moderation and must not be marketed as such.

### Data integrity

-   Never label mock or generated values as real Twitch measurements.
-   Label demo data clearly in the UI.
-   Show the measurement window and methodology in reports.
-   Handle missing or insufficient samples explicitly.
-   Do not manufacture precision or claim statistical certainty.

## 5. Recommended implementation stack

These are implementation recommendations for the current build, not
claims that the original requirements mandate every package.

### Frontend

-   React
-   Vite
-   Tailwind CSS
-   React Router
-   Recharts for data visualisation
-   Lucide icons (use consistent outline icons; do not use emoji as
    interface icons)

### Backend

-   Node.js
-   Express
-   SQLite for the initial database
-   Twitch OAuth
-   Twitch EventSub for chat events
-   Twitch Helix API for viewer-count polling, subject to Twitch API
    availability and limits

### Other

-   WebSocket or Server-Sent Events for live UI updates if needed.
-   Print stylesheet or a PDF-generation approach for one-page reports.
-   OBS browser source route/page for the overlay.

Keep the architecture simple. Do not add packages without a concrete
need.

## 6. Proposed application navigation

Use a persistent left sidebar on desktop and a considered responsive
navigation pattern on smaller screens.

1.  Dashboard
2.  Live Monitor
3.  Analytics
4.  Streamer Roster
5.  Reports
6.  OBS Overlay (may be a separate route or a dedicated preview)
7.  Settings

### Page requirements

#### Login / connect page

-   Clear SponsorProof identity and value proposition.
-   One primary action to connect Twitch.
-   Explain why Twitch access is needed.
-   Do not imply that OAuth is connected when it is not.
-   For a prototype, provide a clearly labelled demo-entry option.

#### Dashboard

-   Retention Score summary.
-   Brand-safety status.
-   Segment count.
-   Campaign clicks only if that feature is implemented and data is
    available.
-   Performance trend chart.
-   Recent sponsor segments.
-   Useful quick actions.

#### Live Monitor

-   Stream and connection status.
-   Current viewer count, with data timestamp/source.
-   Sponsor/brand field.
-   Start segment and end segment actions.
-   Segment elapsed time.
-   Brand-safety status and reason.
-   Pre-segment, active-segment and post-segment phase indicators.
-   If live Twitch data is unavailable, show a clearly labelled demo
    state.

#### Campaign Analytics

-   Retention Score.
-   Recovery.
-   Chat-active retention.
-   Audience flow chart across pre-segment, active segment and
    post-segment windows.
-   Summary of findings.
-   Methodology and sample limitations.
-   Campaign selector.

#### Streamer Roster

-   Streamer cards or a compact table.
-   Consistent comparison metrics.
-   Clear distinction between real connected accounts and sample/demo
    profiles.
-   Add/remove actions only if they work.

#### Reports

-   Select campaign/segment.
-   Optional sponsor contact and notes.
-   One-page report preview.
-   Export/print to PDF.
-   Include measurement definitions, date range, source and limitations.
-   Do not call a browser print action a server-generated PDF.

#### OBS Overlay

-   Minimal browser-source presentation.
-   Sponsor name and segment state.
-   Brand-safety status.
-   High contrast and transparent-background option where practical.
-   Explain whether the page is a local preview or a deployed URL.

#### Settings

-   Account and workspace details.
-   Twitch connection status.
-   Integration status.
-   Demo/production mode clearly visible.
-   Avoid decorative settings that do not work.

## 7. Visual direction based on the supplied reference image

The reference image shows a dark sponsorship analytics product with
purple accents. Treat it as visual direction, not a requirement to copy
every detail.

### Desired design language

-   Dark navy / near-black foundation.
-   Restrained violet accent, used for primary actions and selected
    navigation.
-   Green, amber and red only for meaningful status indicators.
-   Subtle borders and carefully controlled shadows.
-   Strong typographic hierarchy.
-   Spacious layouts and consistent alignment.
-   Charts with readable axes, labels, legends and tooltips.
-   Consistent card padding and border radius.
-   Responsive layouts that remain usable on laptop and mobile widths.

### Make it feel like a real product

-   Use a consistent spacing scale and component system.
-   Use a restrained palette; avoid excessive gradients and glow
    effects.
-   Prefer real typography hierarchy over oversized headings everywhere.
-   Use meaningful empty, loading, error, disconnected and success
    states.
-   Keep data dense but readable.
-   Align card edges and grid columns precisely.
-   Use realistic, concise product copy.
-   Keep navigation labels and terminology consistent.
-   Use believable demo data only when clearly marked.
-   Avoid unnecessary decoration, random badges and repeated cards that
    add no information.

### Explicitly avoid

-   No emoji anywhere in the interface, navigation, buttons, charts or
    status indicators.
-   Do not use emoji as substitutes for icons. Use Lucide icons or
    simple CSS/SVG icons.
-   No generic AI-looking gradients on every card.
-   No excessive glassmorphism, neon glow or oversized rounded pills.
-   No arbitrary random numbers that change on every render.
-   No fake live indicators when no live source is connected.
-   No fake Twitch connection success.
-   No placeholder buttons that do nothing.
-   No lorem ipsum.
-   No claims that demo data is actual audience analytics.
-   Do not add an AI chatbot or AI features unless requested.

## 8. Existing prototype

A standalone `index.html` prototype was created during the conversation.
It includes:

-   Login/demo entry.
-   Dashboard with sample KPI cards and chart.
-   Live Monitor with simulated segment start/end and timer.
-   Analytics page with illustrative audience metrics.
-   Roster with sample streamer profiles and add-streamer interaction.
-   Reports page with print-ready report output.
-   OBS overlay preview.
-   Settings page.

Important limitations of that prototype:

-   It is a frontend-only mock-up.
-   It does not connect to Twitch.
-   It does not have a backend or database.
-   Metrics and segment activity are demo data.
-   The report uses the browser print dialog; user can choose Save as
    PDF.
-   The overlay is a local preview, not a hosted OBS URL.
-   Browser state is not persistent.

Use the prototype as a visual and interaction reference. Do not assume
its code or metrics are production-ready. Improve the information
architecture, visual polish, accessibility and responsive behaviour
rather than blindly copying it.

## 9. Current setup instructions (Windows / VS Code)

If the project has not already been set up, install:

-   VS Code: https://code.visualstudio.com/
-   Node.js LTS: https://nodejs.org/en/download
-   Git: https://git-scm.com/downloads

In the VS Code terminal, verify:

``` powershell
node -v
npm -v
git --version
```

Suggested project layout:

``` text
SponsorProof/
  SPONSORPROOF_PROJECT_BRIEF.md
  frontend/
  backend/
```

Create the Vite React frontend:

``` powershell
npm create vite@latest frontend -- --template react
cd frontend
npm install
npm install react-router-dom recharts lucide-react
```

For Tailwind CSS v4 with Vite:

``` powershell
npm install tailwindcss @tailwindcss/vite
```

Configure `vite.config.js`:

``` js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'

export default defineConfig({
  plugins: [react(), tailwindcss()],
})
```

In `src/index.css`:

``` css
@import "tailwindcss";
```

Create the backend:

``` powershell
cd ..
mkdir backend
cd backend
npm init -y
npm install express cors dotenv
npm install -D nodemon
npm pkg set type=module
npm pkg set scripts.dev="nodemon server.js"
```

Create `backend/server.js`:

``` js
import express from 'express'
import cors from 'cors'
import dotenv from 'dotenv'

dotenv.config()

const app = express()
app.use(cors())
app.use(express.json())

app.get('/api/health', (req, res) => {
  res.json({ status: 'OK', project: 'SponsorProof' })
})

const PORT = process.env.PORT || 5000
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

Create `backend/.env`:

``` env
PORT=5000
```

Create `backend/.gitignore`:

``` gitignore
node_modules/
.env
```

Run frontend:

``` powershell
cd frontend
npm run dev
```

Run backend in a second terminal:

``` powershell
cd backend
npm run dev
```

Expected local URLs:

-   Frontend: `http://localhost:5173/`
-   Backend health check: `http://localhost:5000/api/health`

Do not recreate these files if they already exist and work. Inspect
first.

## 10. How to work with Codex

The project owner is a beginner programmer. Explain changes in plain
language, work in small milestones and avoid overwhelming them.

Codex must:

1.  Inspect the existing repository before editing.
2.  Preserve working code.
3.  Avoid overwriting files without understanding them.
4.  Explain which files it intends to change.
5.  Make one coherent milestone at a time.
6.  Run available checks/build commands after changes.
7.  Fix errors introduced by its own changes.
8.  Tell the owner exactly how to run and test the result.
9.  Avoid unnecessary dependencies.
10. Never claim a feature is implemented unless it works.

### Initial Codex prompt

Copy this into Codex after opening the SponsorProof project folder:

``` text
Read SPONSORPROOF_PROJECT_BRIEF.md completely before making changes.

You are helping me build SponsorProof, a polished Twitch sponsorship analytics SaaS product. I am a beginner, so explain your work in clear, simple language.

First inspect the entire existing project structure and the current frontend/backend code. Do not immediately rewrite anything. Tell me:
1. What currently exists.
2. What works.
3. What is incomplete or broken.
4. Which files you recommend changing first.

The visual direction is a premium, restrained dark SaaS dashboard with a near-black/navy background, subtle violet accent, clean typography, consistent spacing, precise alignment and accessible contrast.

Critical design requirements:
- Absolutely no emoji in the UI.
- Use Lucide icons consistently.
- Avoid generic AI-generated dashboard styling, excessive gradients, neon glows and decorative clutter.
- Make it look like a professionally designed real SaaS product.
- Use realistic copy and consistent labels.
- Make every visible button functional or clearly disabled with an explanation.
- Include responsive layouts, hover/focus states, empty states and loading/error states where relevant.

Product requirements and feature priorities are in SPONSORPROOF_PROJECT_BRIEF.md. Keep demo data clearly labelled. Never pretend demo data is real Twitch data. Do not fake a Twitch connection.

Do not connect to Twitch or add backend integrations in this first inspection step. Do not add packages yet. Do not overwrite working files. Wait for my approval before making the first implementation changes.
```

### UI refinement prompt

Once Codex has inspected the project and the owner approves
implementation, use this prompt:

``` text
Now improve the SponsorProof frontend to a professional, production-quality visual standard using the project brief as the source of truth.

Work on the existing project rather than creating a second app. Keep all routes and interactions that already work. Do not use emoji anywhere. Use the installed Lucide icon package for icons.

Priorities:
1. Establish a consistent design system: colours, typography, spacing, borders, radii, shadows and reusable components.
2. Refine the sidebar, top navigation, page headers and responsive layout.
3. Make Dashboard, Live Monitor, Analytics, Roster, Reports, OBS Overlay and Settings feel like one coherent product.
4. Improve charts, data hierarchy, tables, empty states, status indicators and form controls.
5. Ensure all controls have meaningful behaviour and clear feedback.
6. Clearly label all mock metrics and demo interactions.
7. Keep the design restrained and credible—no excessive gradients, glow, decorative icons or AI-looking visual patterns.

Before editing, list the files you plan to change and the reason for each. Then implement in manageable steps. Run the frontend build and fix any errors. At the end, summarize what changed and how I can test it.
```

## 11. Suggested build milestones

Build in this order. Do not attempt every integration at once.

### Milestone 1 --- Stable frontend shell

-   App layout and navigation.
-   Design tokens and reusable components.
-   Responsive behaviour.
-   Route/page structure.
-   Demo state and clear demo labels.

### Milestone 2 --- Working demo interactions

-   Start/end sponsor segment.
-   Segment timer.
-   Sample audience series.
-   Metric calculations from a fixed sample dataset.
-   Roster add/remove.
-   Report preview and PDF/print flow.
-   Overlay preview.

### Milestone 3 --- Backend foundation

-   Express API.
-   SQLite schema and persistence.
-   Streamer, campaign and segment records.
-   Server-side metric calculations.
-   Validation and error handling.

### Milestone 4 --- Twitch integration

-   OAuth flow and secure token handling.
-   EventSub chat events.
-   Helix viewer sampling, with rate limits and timestamps.
-   Reconnection and missing-data handling.
-   Clear connected/disconnected state.

### Milestone 5 --- Reports and production readiness

-   Reproducible one-page PDF.
-   Methodology and limitations.
-   Tests for metric calculations.
-   Accessibility and responsive review.
-   Environment variable and secret handling.
-   Demo rehearsal.

## 12. Acceptance checklist

### Visual quality

-   [ ] No emoji in the interface.
-   [ ] Icons use one consistent icon family.
-   [ ] Typography and spacing are consistent.
-   [ ] Dashboard looks professional and not template-like.
-   [ ] All pages share the same visual system.
-   [ ] Works at desktop and mobile widths.
-   [ ] Focus states and readable contrast are present.

### Functionality

-   [ ] Navigation works.
-   [ ] Start/end segment actions work.
-   [ ] Segment timer works and resets correctly.
-   [ ] Analytics update from selected demo data.
-   [ ] Roster add/remove works if shown.
-   [ ] Report can be printed/exported.
-   [ ] Overlay preview reflects segment state.
-   [ ] Buttons do not falsely claim unsupported integrations.

### Data honesty

-   [ ] Demo data is clearly labelled.
-   [ ] Twitch connection state is accurate.
-   [ ] Metric definitions are documented.
-   [ ] Missing data is not silently replaced with fabricated values.
-   [ ] Reports disclose sample windows and limitations.

------------------------------------------------------------------------

## 13. Conversation decisions captured

The project owner asked whether SponsorProof could be completed in two
hours. The agreed guidance was:

-   A demo-ready MVP may be achievable in two hours if the development
    environment is already set up.
-   A complete production system with real Twitch OAuth, EventSub,
    viewer polling, database, analytics and PDF generation is not a
    realistic two-hour commitment.
-   Focus on a working, honest demonstration rather than pretending all
    production integrations exist.

The project owner then supplied a visual reference image showing six
principal product views: Login, Dashboard, Live Stream Monitoring,
Campaign Analytics, Streamer Roster and Reports, plus an About panel.
The standalone prototype expanded this to include Settings and an OBS
Overlay page.

The owner requested a single `index.html` prototype to inspect the look
and feel. That prototype was created, with the limitations documented
above.

The owner's latest design direction is explicit: **make the UI more
professional, remove all emoji, and avoid an artificial or obviously
AI-generated appearance.**

## 14. Product principle

SponsorProof should feel trustworthy because it is clear about what it
measures, how it measures it, and what the data can and cannot prove.

A smaller number of polished, genuinely working features is better than
a large interface full of fake controls and invented analytics.
