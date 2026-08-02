# Manifest — storage box inventory with QR codes

A single-page app for tracking what's in your storage boxes. Print a QR code
for each box, stick it on the box, and scanning it opens that box's itemized
list. Everything is stored in one `.xlsx` file that lives in this GitHub repo
— no database, no server.

## How it works

- The whole app is one file: `index.html`. It runs entirely in your browser.
- Your inventory is read from and written to an `.xlsx` file in this repo,
  using the GitHub API and a personal access token you provide.
- Each box gets a QR code that links to `your-site-url/#box=BOXID`. Scanning
  it opens this same page, jumped straight to that box's contents.

## One-time setup

1. **Create a repo** (or use this one) and make sure `index.html` is in the
   root.
2. **Turn on GitHub Pages**: repo → Settings → Pages → set source to your
   main branch, root folder. GitHub will give you a URL like
   `https://yourname.github.io/your-repo/`.
3. **Create a personal access token**, scoped narrowly so it can only touch
   this repo:
   - Go to https://github.com/settings/personal-access-tokens/new
   - Resource owner: you. Repository access: **only select repositories** →
     pick this repo.
   - Permissions → Repository permissions → **Contents: Read and write**.
     Leave everything else as "No access."
   - Generate the token and copy it — you won't see it again.
4. Open your Pages URL. In the **Connect your repo** form, enter:
   - your GitHub username
   - the repo name
   - the file path (default `inventory.xlsx` is fine — you don't need to
     create this file yourself, the app will offer to create it)
   - the site URL from step 2
   - your token
5. Click **Connect**. First time, click **Create it** when prompted to set
   up the inventory file.

Optionally check "Remember this token on this device" so you don't have to
retype it every time you scan a box on your phone. Only do this on a device
only you use — anyone with the token can edit the file.

## Day to day

- **Add a box**: "+ New box" on the dashboard, then edit its name/location
  and add items.
- **Print a label**: open a box → "Print label" → print, cut, tape to the
  box.
- **Scan a box**: point your phone camera at the label. It opens the site
  and jumps straight to that box's item list.
- **Find something**: use the search bar on the dashboard — it searches
  both box names/locations and item names, so "where's my winter coat"
  works.
- **Save**: changes (new boxes, items, edits, deletes) stay local until you
  click **Save changes to GitHub**, which commits the updated `.xlsx` file.

## Data format

The inventory file has two sheets:

- **Boxes**: `ID`, `Name`, `Location`, `Notes`, `Created`
- **Items**: `BoxID`, `Item`, `Quantity`, `Notes`

You can open and edit it directly in Excel/Sheets any time — the app just
reads and rewrites these two sheets, so manual edits are safe as long as the
column headers stay the same.

## Notes / limitations

- This is a client-side-only app — your token lives in your browser, either
  in memory for the session or in local storage if you opted to remember it.
  It's never sent anywhere except to GitHub's API.
- Only one person should edit at a time — the app does a normal Git commit
  on save, so if two people save at once the second save will fail (GitHub
  will reject it because the file changed underneath it); just reload and
  redo the change.
- Works on any static host that serves plain HTML, not just GitHub Pages —
  GitHub Pages is just the free, obvious choice since the data already lives
  in GitHub.
