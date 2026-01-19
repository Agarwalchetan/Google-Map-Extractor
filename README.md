# Google Map Extractor — How to Use It

This project is a **Chrome Extension** that helps you search Google Maps for many keyword/location combinations and export the results (CSV). It also includes optional features such as **email extraction** and **latitude/longitude export**.

> Note: This extension uses **Manifest V2**.

---

## 1) Install the Extension (Load Unpacked)

1. Open Chrome (or Edge).
2. Go to:
   - `chrome://extensions/` (Chrome)
   - `edge://extensions/` (Edge)
3. Enable **Developer mode**.
4. Click **Load unpacked**.
5. Select the project folder:
   - `Google_Map_Extractor/`
6. Confirm the extension appears in your extensions list.
7. (Recommended) Click **Extensions** (puzzle icon) and **Pin** the extension.

---

## 2) Open the Extension UI

1. Click the extension icon.
2. The extension opens its UI in a popup window.

Internally, the UI entry is:
- `controllers/background.js` opens `views/index.html`

---

## 3) Basic Workflow (Keyword + Location → Extract → Download)

### Step A — Enter Keywords

1. In the **Keywords** textarea, add one keyword per line.

Example:
- `restaurants`
- `dentist`
- `car repair`

### Step B — Enter Locations

1. In the **Locations** textarea, add one location per line.

Example:
- `New York`
- `Los Angeles`
- `Chicago`

### Step C — (Optional) Configure Email Extraction

If you want the extension to try to find emails from business websites:

1. Enable **Extract Email**.
2. Set **Max seconds allowed for extracting emails per website**.
   - Example: `10`
3. (Optional) Add an **Email regex list** (blacklist) if you want to ignore certain patterns.

How it works (high level):
- The extension fetches the website, scans for emails, and may follow internal links for a limited time.

### Step D — (Optional) Enable Latitude/Longitude

If you want to export coordinates:

1. Enable **Extract Lat & Long**.

---

## 4) Start Extracting

1. Click **Start Extracting**.
2. The extension will open Google Maps searches for each `keyword + location` task.
3. Keep Chrome running while extraction is active.

You can monitor:
- Progress counter (completed/total)
- Results count

To stop at any time:
- Click **Stop Extracting**

---

## 5) Download Results (CSV)

When you have results:

1. Click **Download**.
2. A CSV file will be generated and downloaded.

Export columns include (may vary by settings):
- `keyword`, `location`, `company_name`, `category`, `website`, `phone`
- `email_1`, `email_2`, `email_3` (if email extraction enabled)
- `lat`, `long` (if lat/long enabled)
- `address`, `city`, `state`, `pincode`, `rating_count`, `review`, `cid`

---

## 6) Clear Collected Data

To reset the current dataset:

1. Click **Clear Data**.

---

## 7) License / Activation (If Applicable)

The UI may show a license/activation modal depending on your configuration and remote checks.

- Enter the **License key** when prompted.
- If you are using a demo mode, exports may be limited.

---

## 8) Important Notes (Correct Way to Run)

### Do NOT open `views/index.html` using a local web server for real usage

If you open this file like:
- `http://127.0.0.1:5500/views/index.html`

you may see raw Angular expressions like `{{local.collect.length}}` or missing extension behavior.

Correct way:
- Load the extension via **Load unpacked** and open it from the **extension icon**.

### Keep tabs/windows available

During extraction the extension may open/close Google Maps tabs as part of its automation.

---

## 9) Troubleshooting

### A) UI shows `{{ ... }}` instead of real values

Likely cause:
- You are opening the HTML directly in a browser tab/server instead of running it as an extension.

Fix:
- Load unpacked extension and open it from the extension icon.

### B) Popup is blank / buttons don’t work

Fix checklist:
- Reload the extension from `chrome://extensions/`.
- Open DevTools for the extension popup and check console errors.
- Ensure required scripts exist and paths are correct.

### C) Email extraction does not find emails

Possible causes:
- The business website blocks requests.
- CORS restrictions / redirects / anti-bot pages.
- The email isn’t present on the site.

Try:
- Increase **Max seconds**.
- Disable email extraction if you only need map data.

### D) Latitude/Longitude columns missing

Cause:
- **Extract Lat & Long** is disabled.

Fix:
- Enable **Extract Lat & Long** and export again.

---

## 10) Project Structure (Quick Reference)

- `manifest.json` — Extension configuration
- `controllers/background.js` — Opens the UI popup
- `views/index.html` — Main UI
- `controllers/index.js` — Main UI logic (Angular controller)
- `controllers/content.js` — Runs on Google pages (content script)
- `libs/chromeBox.js` — Helper wrapper used throughout (`$box`)
- `libs/foxCommon.js` — Utility functions (`FoxCommon`, `$fc`)

---

## 11) Disclaimer

Use responsibly and comply with:
- Google Maps Terms of Service
- Website terms and privacy policies
- Applicable laws/regulations in your region
