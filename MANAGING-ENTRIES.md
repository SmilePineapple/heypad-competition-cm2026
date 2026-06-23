# Managing Competition Entries

## Viewing all entries

### Option 1: Google Sheet (recommended)
All quiz entries submit to your Google Form, which feeds into a **linked Google Sheet** in real-time.

**To access:**
1. Open your form: https://docs.google.com/forms/d/e/1FAIpQLSevb6foLJfzrgZP3VHBFY58HySTGvkSCINVzBulJBjS7RJSeg/edit
2. Click the **"Responses"** tab.
3. Click the **green Sheets icon** (📊) to open the spreadsheet.

**In the spreadsheet you can:**
- View all entries with timestamps, names, emails, scores, etc.
- **Delete duplicate rows** (right-click row number → Delete row).
- **Edit cells** if someone made a typo.
- **Sort/filter** by any column (email, name, timestamp, score).
- **Export to CSV** for backup or offline analysis.
- **Share** with colleagues (File → Share).

The sheet updates live as new entries arrive.

### Option 2: Delete from Form UI
In the **Responses** tab of the Google Form:
- Click **⋮ (three dots)** next to any response.
- Select **"Delete response"**.

---

## Duplicate prevention

### Client-side check (now active)
The quiz page now checks `localStorage` before allowing submission:
- **Blocks duplicate emails** (case-insensitive).
- **Blocks duplicate names** (case-insensitive).
- Shows error: *"This email address has already been entered. Each person can only enter once."*

**Limitation:** Only works on the same browser/device. If someone uses a different browser or clears their cache, they could re-enter.

### Manual cleanup in Google Sheet
If duplicates slip through (different browsers, typos, etc.):
1. Open the linked Google Sheet.
2. **Sort by email** (click column header → Sort A→Z).
3. Scan for duplicate emails and delete extra rows.
4. Repeat for the **name** column if needed.

### Advanced: Conditional formatting to highlight duplicates
In the Google Sheet:
1. Select the **email column** (e.g., column B).
2. **Format → Conditional formatting**.
3. Format rule: **Custom formula is** → `=COUNTIF(B:B,B1)>1`
4. Choose a highlight colour (e.g., red).
5. Click **Done**.

Duplicate emails will now be highlighted automatically.

---

## Prize-draw wheel entry source

The **draw.html** wheel loads entries from the **browser's `localStorage`** (same store the quiz uses).

**Important:** The wheel only sees entries submitted **on that specific browser**. If you want the wheel to show all entries from the Google Sheet:

### Option A: Manual entry list
Before the draw, open `draw.html` in the browser console and run:
```js
const allNames = ["Alice Smith", "Bob Jones", "Charlie Brown", ...]; // paste from Sheet
localStorage.setItem('heypad_entries', JSON.stringify(allNames.map(name => ({name}))));
location.reload();
```

### Option B: Export from Sheet and import
1. In the Google Sheet, copy all names (column A).
2. Open browser console on `draw.html`.
3. Paste and run:
```js
const names = `Alice Smith
Bob Jones
Charlie Brown`.split('\n').filter(n=>n.trim());
localStorage.setItem('heypad_entries', JSON.stringify(names.map(name => ({name: name.trim()}))));
location.reload();
```

### Option C: Fetch from Sheet API (requires setup)
If you want the wheel to auto-sync with the Google Sheet, I can add a script that fetches entries via the Google Sheets API. This requires:
- Publishing the Sheet to the web (File → Share → Publish to web), or
- Setting up a Google Cloud project with Sheets API access.

Let me know if you want this — it's a 10-minute addition.

---

## At the event: recommended workflow

1. **Set up a tablet/laptop** at the Jabbla stand with the quiz page open.
2. Attendees complete the quiz and enter their details.
3. **Periodically check the Google Sheet** on your phone/laptop to monitor entries and remove any obvious duplicates.
4. **At draw time:**
   - Open `draw.html` on the same device (or sync entries via console if using a different device).
   - Project the wheel on a screen if available.
   - Click **"Spin the wheel!"** in front of attendees.
   - Announce the winner and verify they're present.
5. **After the event:** Export the Google Sheet for records and follow-up.

---

## Questions?

- **"Someone entered twice by mistake"** → Delete the duplicate row in the Google Sheet.
- **"The wheel doesn't show all entries"** → Entries are browser-local. Use Option B above to import from the Sheet.
- **"Can I edit an entry after submission?"** → Yes, directly in the Google Sheet (edit any cell).
- **"How do I backup entries?"** → In the Sheet: File → Download → CSV or Excel.

Need help setting up auto-sync between the Sheet and the wheel? Just ask.
