# Trading Journal Dashboard

A one-page website that reads `trades.xlsx` and shows your performance: equity curve in R, drawdown, rolling and monthly win rate vs. your breakeven win rate, monthly and daily R, breakdowns by setup / session / market / direction / weekday / hour, and a sortable trade log.

There's no server and no database. The page downloads `trades.xlsx` from the same folder each time it opens (and every 5 minutes while open), so updating the site = updating the Excel file on GitHub.

## Files

| File | What it is |
|---|---|
| `index.html` | The dashboard. Settings are in the `CONFIG` block near the bottom. |
| `trades.xlsx` | Your journal. Log trades on the **Trade Log** sheet. |
| `README.md` | This guide. |

## One-time setup (about 10 minutes)

1. **Create the repository.** On github.com click **New repository**, name it e.g. `trading-journal`.
   - **Public** works on the free plan.
   - **Private** requires GitHub Pro for Pages. Either way, the published *site* is reachable by anyone with the link (the page is set to `noindex`, so search engines won't list it, but it isn't password-protected).
2. **Install GitHub Desktop** (desktop.github.com) and sign in. Choose **File → Clone repository** and clone `trading-journal` to a folder on your computer.
3. **Copy the three files** (`index.html`, `trades.xlsx`, `README.md`) into that folder.
4. In GitHub Desktop, type a summary like "Initial dashboard", click **Commit to main**, then **Push origin**.
5. **Turn on Pages.** On github.com open the repo → **Settings → Pages** → under *Build and deployment* choose **Deploy from a branch**, branch **main**, folder **/ (root)** → **Save**.
6. After a minute, the page shows your URL, something like `https://YOUR-USERNAME.github.io/trading-journal/`. Bookmark it.

## Daily workflow

1. Open `trades.xlsx` **from the cloned folder** and log your trades.
2. Save and close Excel.
3. In GitHub Desktop: **Commit to main → Push origin**.
4. Within about a minute the site reflects the new trades (click **↻ Refresh** if the page is already open).

Tip: keep the file closed in Excel when you commit; Excel's temporary `~$trades.xlsx` lock file shouldn't be committed. Add a `.gitignore` containing `~$*` to be safe.

## Logging trades

The **Trade Log** sheet has exactly these columns: Trade No, Date, Time, Market, Direction, Session, Setup, Result, Notes, Screenshot.

- **Required:** Date and Result. Rows without both are ignored.
- **Result:** TP, SL or BE. The dashboard converts it to R: TP = +6R, SL = −1R, BE = 0 (change `rValues` in the settings if your target changes).
- **Win rate** = TP ÷ (TP + SL). Breakevens are left out of win rate and streaks.
- **Screenshot:** paste a TradingView/image link (or insert a hyperlink) and the trade log shows a "View ↗" link.
- **Time:** optional; powers the Hour breakdown.
- Rows 1–2 (title and instructions) can stay; the site finds the header row automatically. Keep the header names as they are.
- The data is an Excel table, so new rows pick up the formatting and dropdowns automatically. To add a market or session: select the column → **Data → Data Validation** → add it to the list.

## Previewing without pushing

Open the live site, click **Open file…** (or drag your `trades.xlsx` onto the page) to see it with your local, unpushed file. Nothing is uploaded; it's read in your browser only. Click **Back to published data** to return.

Opening `index.html` by double-clicking won't load data automatically (browsers block that for local files), but **Open file…** still works there.

## Settings

In `index.html`, near the bottom:

```js
const CONFIG = {
  file: "trades.xlsx",
  sheet: null,                        // null = first sheet
  rValues: { TP: 6, SL: -1, BE: 0 },  // R per result
  riskPerTrade: null,                 // e.g. 100 → also shows $ amounts
  currency: "$",
  refreshMinutes: 5
};
```

## Troubleshooting

- **"Data didn't load (HTTP 404)"** – `trades.xlsx` isn't in the repo root next to `index.html`, or the name differs (it's case-sensitive).
- **"Couldn't find a header row with Date and Result"** – one of those headers was renamed. Restore it.
- **Site shows old data** – GitHub Pages takes 30–90 seconds to redeploy. Check the repo's **Actions** tab for the "pages build and deployment" run.
