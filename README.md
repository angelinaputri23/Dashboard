# Telecom POS Weekly Sales Dashboard

A static GitHub Pages dashboard for weekly telecom POS sales across five stores.

## Included
- `index.html` — dashboard UI, filters, charts, target recommendations, CSV export, and Google Sheets refresh.
- `data/snapshot.json` — fallback snapshot built from `Telecom Data Analysis.xlsx`.

## Store mapping
| Sheet tab | Code | Store |
|---|---|---|
| 30002 MF | MF | Minghu |
| 30003 DW | DW | Dewei |
| 709001 TN | TN | Tainan |
| 811001 DT | DT | Daxue |
| 829001 FN | FN | Hunei |

## Deploy to GitHub Pages
1. Create a GitHub repository, for example `Telecom-POS-Weekly-Dashboard`.
2. Upload `index.html` and the entire `data` folder (keep `data/snapshot.json` inside it).
3. Open **Settings → Pages**.
4. Select **Deploy from a branch**, branch `main`, folder `/ (root)`, then Save.
5. Open the published Pages URL.

## Refresh from Google Sheets
The refresh button is configured with the spreadsheet ID supplied in the conversation. It expects five tabs with these exact names:
- `30002 MF`
- `30003 DW`
- `709001 TN`
- `811001 DT`
- `829001 FN`

Each tab must retain the source column headings:
`Date Range`, `商品編號`, `商品名稱`, `銷售數量`, `銷售金額`, `退貨數量`, `退貨金額`, `銷售淨數量`, `銷售淨額`.

1. Make sure the spreadsheet contains those five POS tabs. If the uploaded Excel is not yet in that Google spreadsheet, import it into Google Sheets and preserve the tab names.
2. Set **Share → General access → Anyone with the link → Viewer** (or use an access arrangement that allows the published dashboard to read it).
3. Open the dashboard and click **Refresh from Google Sheets**.

If refresh fails, the dashboard keeps displaying its embedded snapshot. Check the sheet ID, tab names, column headings, and sharing permission. GitHub Pages is static hosting; the dashboard reads the published Google Sheet when the user clicks Refresh.

## Analysis rules
- This is **POS only**. There is no App dataset, so App vs POS comparisons are intentionally not shown.
- Weekly totals use `銷售淨數量` (net pcs = sold qty less returns).
- The 2026/10/01–2026/11/07 range is a 38-day overlapping aggregate, not a weekly period, and is excluded from the weekly trend to avoid double counting.
- Minghu has no row for 2026/08/23–2026/08/29 in the uploaded workbook; it remains blank/not counted, not treated as zero sales.
- The uploaded DW, TN, DT, and FN sheets contain identical row-level data. The dashboard preserves the source as supplied; please verify the source tabs if those stores should have different sales.

## Target method
- Suggested weekly target = average of the latest 4 available weekly totals for that store.
- Minimum benchmark = 80% of that 4-week average, rounded up.
- Targets recalculate based on selected product filter. These are planning benchmarks, not guarantees.
