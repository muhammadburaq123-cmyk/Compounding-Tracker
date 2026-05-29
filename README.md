# Compounding Tracker v7

A PWA (Progressive Web App) to track compounding trading goals per account. Works offline, installable on mobile and desktop.

## Files
```
index.html      — main app
manifest.json   — PWA manifest (install as app)
sw.js           — service worker (offline support)
icon-192.png    — app icon
icon-512.png    — app icon
```

## Deploy to GitHub Pages (free hosting + mobile install)

### Step 1 — Create repo
1. Go to github.com → New repository
2. Name it: `compounding-tracker`
3. Set to **Public**
4. Click **Create repository**

### Step 2 — Upload files
1. In the new repo click **Add file → Upload files**
2. Upload all 5 files: `index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`
3. Click **Commit changes**

### Step 3 — Enable GitHub Pages
1. Go to repo **Settings → Pages**
2. Under Source select **Deploy from a branch**
3. Branch: **main**, folder: **/ (root)**
4. Click **Save**
5. Wait ~1 min, your URL will be: `https://YOUR_USERNAME.github.io/compounding-tracker`

### Step 4 — Install on mobile
1. Open the URL in Chrome (Android) or Safari (iOS)
2. **Android:** tap the "Install" banner that appears, or browser menu → "Add to Home Screen"
3. **iOS Safari:** tap Share → "Add to Home Screen"
4. App icon appears on your home screen — launches fullscreen like a native app

### Step 5 — Google Sheets sync (already configured)
The SHEET_URL in index.html points to your Apps Script.
Make sure your GAS script has:
```javascript
function doGet(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('data');
  const val = sheet.getRange('A1').getValue();
  return ContentService.createTextOutput(val || '{}')
    .setMimeType(ContentService.MimeType.JSON);
}
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('data');
  sheet.getRange('A1').setValue(e.postData.contents);
  return ContentService.createTextOutput('ok');
}
```
Deploy as: **Execute as Me**, **Anyone** access.

## Bugs fixed in v7
- Weekly reset now uses Monday (ISO-aligned) — consistent between reset detection and PnL summing
- `softUpdate()` now updates pct-display, target labels, and goal route estimates live
- Goal routes update instantly when balance or % changes
- Color modal is now a fixed overlay (no layout overlap)
- `resetColors()` no longer double-opens the modal
- `addLog()` clears the input field after logging
- CORS handling: POST uses `no-cors` mode (fire-and-forget), GET attempts cors with silent fallback
- Confirm dialog before deleting an account
- PWA: manifest + service worker + install banner
- Responsive: 2-col cards on mobile, single col on very small screens
