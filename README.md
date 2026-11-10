# Xmart Telecom Dashboard

One `index.html` (no build, no dependencies) + `data/snapshot.json` (the xlsx data as a fallback).
Five views: **Next Week Target**, Sales Overview, Branch Comparison, Products & Trends, Data Check.

## 1. Host it on GitHub Pages
1. Create a repo and upload `index.html` and the `data/` folder.
2. Settings → Pages → Branch `main` / root → Save.
3. Your dashboard will be at `https://<username>.github.io/<repo>/`.

## 2. Refresh the data (3 ways)
- **⟳ Refresh from Google Sheets** – reads the 5 branch tabs (`30002 MF`, `30003 DW`, `811001 DT`, `829001 FN`, `709001 TN`).
  The sheet must be shared as *Anyone with the link – Viewer*. The Sheet ID is already set in `CONFIG.SHEET_ID`.
  The dashboard also tries this automatically when opened.
- **📂 Upload .xlsx** – pick a new Excel file from your computer (same layout as `Telecom_Data_Analysis.xlsx`). Works offline.
- **Snapshot** – if both fail, it uses `data/snapshot.json` (or the copy embedded inside `index.html`).

To update the snapshot: replace `data/snapshot.json` (it is a JSON object `{ "<tab name>": [[row], [row], …] }`).

## 3. Next Week Target
- Compares **last week** vs **previous week** (sales, pcs, % change) per branch and per product.
- **Target = average of the chosen basis × (1 + growth %)**, rounded up per product.
  Basis: last week only / average of last 2 weeks / average of last 4 weeks. Growth default 10%.
- Shows next week's target in pcs, NT$ and per day. Pick one branch to see its own targets.

## Sheet layout expected (one tab per branch)
Columns: `Date Range | 商品編號 | 商品名稱 | … | 銷售數量 | 銷售金額 | 退貨數量 | … | 退貨金額 | 銷售淨數量 | 銷售淨額`.
One block per week (`2026/10/25 - 2026/10/31`), product rows, then a `合計` total row. All figures in the dashboard are **net** (after returns).

## CONFIG (top of the script in `index.html`)
- `STORES` – tab name → branch name. Add a row for any new branch. Daxue (DT) and Hunei (FN) were mapped from postal codes (811, 829) – please confirm.
- `FIX_OVERLAP` – a block that overlaps earlier weeks (e.g. `2026/10/01 - 2026/11/07`) is counted as starting the day after the previous block (→ 1–7 Nov). Set `false` to disable.
- `TREND_WEEKS` / `TREND_PCT` – rising/falling products: last N weeks vs the N before; ±25% per day.
- `AUTO_LIVE` – try Google Sheets automatically on open.

## Data notes
- Operator (Chunghwa, FarEasTone, Taiwan Mobile, Hello Indo), type (SIM / Top-up) and plan price are read from the product name.
- The first week (1–8 Aug) has 8 days; others have 7.
- **Known issue in the current file:** tabs DW, DT, FN and TN contain identical numbers (only MF differs). The dashboard warns about this; fix the source reports before trusting those branches' comparisons and targets.
