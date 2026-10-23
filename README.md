# Fantasy NBA Optimiser (GitHub Pages prototype)

A mobile-friendly, static prototype for a Yahoo Fantasy NBA head-to-head 9-category decision dashboard.

## Run locally
Open `index.html` in a browser, or serve the folder with any static server.

## Deploy to GitHub Pages
1. Create a GitHub repository and upload `index.html` and this README.
2. In **Settings → Pages**, select deployment from the `main` branch and `/ (root)`.
3. Open the published Pages URL. No build step is required.

## What's implemented
- Command Centre with sample matchup/category view and approval queue
- Roster table and CSV export
- Weekly matchup category plan and review checklist
- Waiver Scout with category-fit filtering and proposal-to-review workflow
- League Intelligence placeholder for standings, category percentiles, opponent scouting and transaction trends
- Local display settings, JSON data import, decision export and browser-local persistence
- Approve/dismiss/undo recommendation workflow; approving only records a local decision and does **not** submit any transaction

## Important: Yahoo API / security
This is a front-end prototype with illustrative data; it is not connected to Yahoo and does not scrape Yahoo pages. Yahoo's official developer portal currently says Fantasy Sports API access is read-only by default and access applications are reviewed. See:
- https://sports.yahoo.com/developer/access/
- https://sports.yahoo.com/developer/docs/

GitHub Pages is static hosting. Do not put a Yahoo client secret, refresh token, or private credentials in browser code, repository files, or imported JSON. For a real connection, add a small server-side API (for example, a serverless function) that handles OAuth 2.0 token exchange and refresh securely, then calls `https://fantasysports.yahooapis.com/fantasy/v2`. The front end should call only your backend. Request access and confirm allowed scopes before building against the API.

## Suggested production architecture
- **Frontend:** this static GitHub Pages app
- **Backend:** serverless functions / small API service for OAuth callback, token refresh, Yahoo API proxy, data normalisation and rate limiting
- **Storage:** private database for token references, league snapshots, weekly category projections and decisions; never browser localStorage for OAuth tokens
- **Analytics:** category-aware projections that include volume and attempts for FG%/FT%, turnover direction (lower is better), remaining NBA schedule, lineup slots and transaction limits
- **Workflow:** recommendations are proposed, user reviews and approves; write actions should remain disabled unless Yahoo explicitly approves the required access and the implementation is verified
- **Monitoring:** handle expired tokens, API errors, stale data timestamps and incomplete stats visibly

## Data import shape
A JSON file can contain any of these arrays:
```json
{
  "roster": [],
  "waivers": [],
  "cats": [],
  "recommendations": []
}
```
The sample schema can be inspected in `index.html` (`defaultState`). Imported data is stored in the browser only.
