# Google Apps Script — Booking Requests Sheet

Bound to the "Genesis Pool and Spa - Booking Requests" Google Sheet. Not stored anywhere
in this git repo — Apps Script projects live in Google Drive, bound to the spreadsheet —
so this file is the source of truth for what the deployed script should contain. Paste it
into the Sheet's **Extensions → Apps Script** editor (`Code.gs`) and redeploy any time it
changes here.

Receives `doPost` submissions from both site forms (see [`plan.md`](plan.md)'s "Contact
Preference Popup — Lead Routing Plan" section for the client-side wiring):
- `Genesis-Pool-and-Spa-main/schedule.html` (`schedForm`, via `navigator.sendBeacon`)
- `Genesis-Pool-and-Spa-main/index.html` `#contact` (`contactForm`, via `fetch(...,
  {mode:'no-cors'})`)

Both forms append a `Form Source` field (`'Schedule Form'` / `'Contact Form'`) that the
script uses to route the row to the correct tab.

## Change from the original single-sheet design

The first version of this script (described in `plan.md`) logged **both** forms into one
sheet/tab, with a `Form Source` column distinguishing which form a row came from. That's
now been split into **two separate tabs**, per direct instruction:

- **First tab in the spreadsheet — `Booking Requests`**: free-cleaning / schedule
  requests only (`schedule.html`). This is what's visible by default when the Google
  Sheet is opened.
- **Second tab — `Contact Form Submissions`**: `index.html` `#contact` "Send Us a
  Message" submissions only, including the `Service Interested In` column.

Each tab gets its own header row (self-healing — see `ensureHeaders_` below), rather than
one shared 15-column header with a `Form Source` tag. `Form Source` is still read from
the incoming payload to decide *which* tab a row goes to, but it's no longer stored as a
column, since the tab itself now carries that meaning.

**Bundle option**: `index.html`'s contact form service dropdown includes a `bundle`
`<option>` (`Bundle (Signup) — Cleaning + Balancing + Free Tile Cleaning`) — see
`js/main.js`, which reads the *selected option's visible text* (not its `value`) into
`Service Interested In` before posting:

```js
const serviceLabel = serviceSelect.options[serviceSelect.selectedIndex].text;
payload.append('Service Interested In', serviceLabel);
```

No script-side change was actually needed to "support" bundle specifically — `Service
Interested In` is logged as free text, so whatever the dropdown sends (including the
bundle label) is captured automatically. What *was* missing is fixed below: the script
now always writes `Service Interested In` into the `Contact Form Submissions` tab's
column of that name, so a bundle signup is never silently dropped or misfiled.

## `Code.gs`

```javascript
/**
 * Genesis Pool and Spa — lead logging Web App.
 * Routes schedule.html and index.html #contact submissions to separate tabs.
 */

var BOOKING_SHEET_NAME = 'Booking Requests';
var CONTACT_SHEET_NAME = 'Contact Form Submissions';

var BOOKING_HEADERS = [
  'Timestamp',
  'First Name',
  'Last Name',
  'Phone',
  'Email',
  'Property Address',
  'Property Type',
  'Preferred Date',
  'Preferred Time',
  'Confirmation Method',
  'Alternate Date',
  'Notes'
];

var CONTACT_HEADERS = [
  'Timestamp',
  'First Name',
  'Last Name',
  'Phone',
  'Email',
  'Service Interested In',
  'Notes'
];

function doPost(e) {
  var params = (e && e.parameter) || {};
  var formSource = params['Form Source'];

  var sheetName = formSource === 'Contact Form' ? CONTACT_SHEET_NAME : BOOKING_SHEET_NAME;
  var headers = formSource === 'Contact Form' ? CONTACT_HEADERS : BOOKING_HEADERS;

  var sheet = getOrCreateSheet_(sheetName, sheetName === BOOKING_SHEET_NAME ? 0 : 1);
  ensureHeaders_(sheet, headers);

  var row = headers.map(function (col) {
    if (col === 'Timestamp') return new Date();
    return params[col] || '';
  });

  sheet.appendRow(row);

  return ContentService.createTextOutput('OK');
}

/** Gets the named tab, creating it at the given index (0 = first) if missing. */
function getOrCreateSheet_(name, index) {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet = ss.getSheetByName(name);
  if (!sheet) {
    sheet = ss.insertSheet(name, index);
  }
  return sheet;
}

/** Self-heals row 1 to match the expected headers for this tab. */
function ensureHeaders_(sheet, headers) {
  var range = sheet.getRange(1, 1, 1, headers.length);
  var current = sheet.getLastRow() > 0 ? range.getValues()[0] : [];
  var matches = headers.every(function (h, i) { return current[i] === h; });
  if (!matches) {
    range.setValues([headers]);
  }
}
```

## Deployment

1. Open the "Genesis Pool and Spa - Booking Requests" Sheet → **Extensions → Apps
   Script**.
2. Replace `Code.gs` with the script above.
3. **Deploy → New deployment → Web app.**
   - Execute as: **Me**
   - Who has access: **Anyone** (the site posts to this anonymously, with `no-cors` /
     `sendBeacon` — there's no auth token in either client)
4. Copy the deployed `/exec` URL into `window.APPS_SCRIPT_URL` in
   `Genesis-Pool-and-Spa-main/js/main.js` (currently a
   `'REPLACE_WITH_DEPLOYED_APPS_SCRIPT_URL'` placeholder — both forms' logging calls are
   guarded no-ops until this is set).
5. On first run, `getOrCreateSheet_` / `ensureHeaders_` will create both tabs (or rename
   an existing single combined tab manually beforehand if migrating old data — the script
   itself doesn't migrate existing rows).
6. Re-deploy (**Deploy → Manage deployments → Edit → New version**) any time `Code.gs`
   changes — editing the script alone doesn't update the live `/exec` URL's behavior.

## Open items

- Existing rows already logged under the old single-sheet/`Form Source`-column design (if
  any were captured before this split) aren't auto-migrated — split them into the two new
  tabs manually if that history needs to be kept.
- Per [`twilio.md`](twilio.md), the Sheet is also the trigger source for the Twilio/Zapier
  "reply YES → notify the boss" flow (Zap 1 watches for new booking rows) — that Zap's
  trigger sheet/tab reference should be repointed at `Booking Requests` specifically now
  that it's a named tab rather than the whole spreadsheet's one sheet.

## Raw `Code.gs` (copy-paste)

```javascript
/**
 * Genesis Pool and Spa — lead logging Web App.
 * Routes schedule.html and index.html #contact submissions to separate tabs.
 */

var BOOKING_SHEET_NAME = 'Booking Requests';
var CONTACT_SHEET_NAME = 'Contact Form Submissions';

var BOOKING_HEADERS = [
  'Timestamp',
  'First Name',
  'Last Name',
  'Phone',
  'Email',
  'Property Address',
  'Property Type',
  'Preferred Date',
  'Preferred Time',
  'Confirmation Method',
  'Alternate Date',
  'Notes'
];

var CONTACT_HEADERS = [
  'Timestamp',
  'First Name',
  'Last Name',
  'Phone',
  'Email',
  'Service Interested In',
  'Notes'
];

function doPost(e) {
  var params = (e && e.parameter) || {};
  var formSource = params['Form Source'];

  var sheetName = formSource === 'Contact Form' ? CONTACT_SHEET_NAME : BOOKING_SHEET_NAME;
  var headers = formSource === 'Contact Form' ? CONTACT_HEADERS : BOOKING_HEADERS;

  var sheet = getOrCreateSheet_(sheetName, sheetName === BOOKING_SHEET_NAME ? 0 : 1);
  ensureHeaders_(sheet, headers);

  var row = headers.map(function (col) {
    if (col === 'Timestamp') return new Date();
    return params[col] || '';
  });

  sheet.appendRow(row);

  return ContentService.createTextOutput('OK');
}

/** Gets the named tab, creating it at the given index (0 = first) if missing. */
function getOrCreateSheet_(name, index) {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet = ss.getSheetByName(name);
  if (!sheet) {
    sheet = ss.insertSheet(name, index);
  }
  return sheet;
}

/** Self-heals row 1 to match the expected headers for this tab. */
function ensureHeaders_(sheet, headers) {
  var range = sheet.getRange(1, 1, 1, headers.length);
  var current = sheet.getLastRow() > 0 ? range.getValues()[0] : [];
  var matches = headers.every(function (h, i) { return current[i] === h; });
  if (!matches) {
    range.setValues([headers]);
  }
}
```
