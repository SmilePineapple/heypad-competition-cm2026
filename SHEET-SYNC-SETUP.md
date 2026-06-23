# Setting up Google Sheet auto-sync for the prize wheel

The prize-draw wheel (`draw.html`) can now **auto-load entries directly from your Google Sheet** in real-time, so it always shows all entries regardless of which browser/device was used.

---

## Step-by-step setup (3 minutes)

### 1. Open your Google Form responses Sheet

1. Go to your form: https://docs.google.com/forms/d/e/1FAIpQLSc-ibGcyf7X2vGHS-qXTzYFqww0OwMeHCXjPR0-lgl9mJIiMQ/edit
2. Click the **"Responses"** tab.
3. Click the **green Sheets icon** (📊) to open the linked spreadsheet.

### 2. Publish the Sheet to the web as CSV

1. In the Google Sheet, go to **File → Share → Publish to web**.
2. In the dialog:
   - **"Entire Document"** should be selected (or select the specific sheet tab if you have multiple).
   - Change the format dropdown from **"Web page"** to **"Comma-separated values (.csv)"**.
3. Click **"Publish"**.
4. Click **"OK"** to confirm.
5. **Copy the published URL** — it will look like:
   ```
   https://docs.google.com/spreadsheets/d/e/2PACX-1vS.../pub?output=csv
   ```

### 3. Add the URL to draw.html

**Option A: I'll do it for you**
- Paste the CSV URL here in the chat and I'll update `draw.html` and push to GitHub immediately.

**Option B: Edit it yourself**
1. Open `draw.html` in a text editor.
2. Find line ~143: `const SHEET_CSV_URL = '';`
3. Paste your CSV URL between the quotes:
   ```js
   const SHEET_CSV_URL = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vS.../pub?output=csv';
   ```
4. Save the file.
5. Commit and push:
   ```bash
   git add draw.html
   git commit -m "Enable Google Sheet auto-sync for prize wheel"
   git push
   ```

### 4. Test it

1. Open the live wheel: https://smilepineapple.github.io/heypad-competition-cm2026/draw.html
2. Check the browser console (F12 → Console tab) — you should see:
   ```
   Loaded X entries from Google Sheet
   ```
3. The wheel should now show all entries from the Sheet, even if they were submitted on different devices.

---

## How it works

- The wheel **fetches the published CSV** every time the page loads.
- It parses the **first column (Full name)** from each row.
- If the Sheet URL is configured, it uses that; otherwise it falls back to `localStorage`.
- The Sheet updates in real-time as new entries arrive from the quiz.

---

## Privacy / security notes

- **Publishing the Sheet makes it publicly readable** (anyone with the CSV URL can see the names).
- The URL is not easily guessable, but it's not secret either.
- If you need to keep entries private, use the `localStorage` method instead (wheel only works on the same device as the quiz).
- You can **unpublish** the Sheet after the event: File → Share → Publish to web → "Published content & settings" → Stop publishing.

---

## Troubleshooting

**"No entries" even though the Sheet has data:**
- Check the CSV URL is correct (should end with `pub?output=csv`).
- Open the CSV URL directly in a browser — you should see raw CSV data starting with the column headers.
- Check the browser console (F12) for error messages.

**Entries are duplicated or wrong:**
- Make sure the **first column** in the Sheet is "Full name" (the wheel reads column 1).
- If you've reordered columns, the wheel might be reading the wrong data.

**Sheet not updating in the wheel:**
- The wheel fetches fresh data on every page load. **Refresh the page** (`Ctrl+R` or `Cmd+R`).
- If using a cached/offline version, clear browser cache.

---

## Next step

**Paste your published CSV URL here** (or in `draw.html` line 143) and the wheel will auto-sync with your Google Sheet.
