# Custom Menu — Vodafone Example

A ready-to-use Canvus custom menu themed for Vodafone, with one-tap browser shortcuts for the most-used Vodafone Roaming, SAP Analytics Cloud, and Microsoft Power Platform dashboards plus a few utility items.

## Folder Contents

```
custom-menu-vodafone/
├── menu.yml        ← The custom menu definition
├── icons/          ← Menu icons (transparent PNGs)
├── content/        ← (optional) sample PDFs, images, videos
└── README.md       ← This file
```

## What's in the menu

| Section | Items |
|---|---|
| **Vodafone Roaming** | Roaming Dashboard — Inbound · Roaming Overview — VF Outbound (Kibana) · Inbound vs Outbound side-by-side |
| **SAP Analytics Cloud** | Savings Stories · SCM Control Centre 1 (Cockpit) · SCM Files folder |
| **Power Platform** | GenAI Roadmap — Procurement (Power Apps) · Inventory Overview — Ageing Wise (Power BI) · Global Network Logistics — Analytics (Power BI) · Inventory + Logistics side-by-side |
| **Other Tools** | Akiro Login · Google |
| **Browser Sizes** | Standard sizes (HD, 4K, Ultrawide, 5x1) · Quad-split of all four Vodafone dashboards |
| **Vodafone Notes** | Vodafone Red note · Plain note |

All bookmarks open inside a Canvus `browser` widget — they keep their original interactivity, so any authenticated session in the embedded browser flows through to the live dashboard.

## Deployment Instructions

### Step 1: Locate your `mt-canvus.ini`

Canvus searches for its configuration file in the following locations (in order):

**Windows:**

| Priority | Path |
|----------|------|
| 1st | `%APPDATA%\MultiTaction\canvus\mt-canvus.ini` |
| 2nd | `%PROGRAMDATA%\MultiTaction\canvus\mt-canvus.ini` |

> `%APPDATA%` is typically `C:\Users\<YourUser>\AppData\Roaming`
> `%PROGRAMDATA%` is typically `C:\ProgramData`

**Ubuntu/Linux:**

| Priority | Path |
|----------|------|
| 1st | `~/MultiTaction/canvus/mt-canvus.ini` |
| 2nd | `/etc/MultiTaction/canvus/mt-canvus.ini` |

> `~/` refers to the home folder of the user running Canvus.

Pick whichever `mt-canvus.ini` your system is using. If neither exists yet, create one at the first priority location.

### Step 2: Copy the folder into place

Copy the `custom-menu-vodafone/` folder into a `menu/` folder alongside your `mt-canvus.ini`.

For example, if your `mt-canvus.ini` is at:

**Windows:**
```
%PROGRAMDATA%\MultiTaction\canvus\mt-canvus.ini
```

Copy to:
```
%PROGRAMDATA%\MultiTaction\canvus\menu\custom-menu-vodafone\
```

**Linux:**
```
~/MultiTaction/canvus/mt-canvus.ini
```

Copy to:
```
~/MultiTaction/canvus/menu/custom-menu-vodafone/
```

After copying, the folder structure should look like this:

```
MultiTaction/canvus/
├── mt-canvus.ini
└── menu/
    └── custom-menu-vodafone/
        ├── menu.yml
        ├── icons/
        └── content/
```

### Step 3: Update `mt-canvus.ini`

Open `mt-canvus.ini` in a text editor and add (or update) the `custom-menu` line under the `[canvas]` section:

**Windows:**
```ini
[canvas]
custom-menu=menu/custom-menu-vodafone/menu.yml
```

**Linux:**
```ini
[canvas]
custom-menu=menu/custom-menu-vodafone/menu.yml
```

> **Windows path note:** Use forward slashes (`/`) or double backslashes (`\\`) in the path. Single backslashes will not work.
>
> The path is **relative to the location of `mt-canvus.ini`**, so `menu/custom-menu-vodafone/menu.yml` works as long as you placed the folder under `menu/` as described above.

### Step 4: Restart Canvus

**Canvus must be restarted** after modifying `mt-canvus.ini` for the change to take effect.

Once restarted, the custom menu icon will appear in the finger menu on the canvas.

## Making Changes

After the initial setup, you can edit `menu.yml` at any time — Canvus picks up changes automatically when you next open the finger menu. **No restart is needed for menu.yml changes**, only for `mt-canvus.ini` changes.

To add a new bookmark, copy any of the existing browser blocks in `menu.yml` and swap in your new tooltip and URL:

```yaml
- tooltip: 'My New Dashboard'
  icon: 'icons/browser.png'
  actions:
    - name: 'create'
      parameters:
        type: 'browser'
        url: 'https://example.vodafone.com/...'
        size: '1920px, 1080px'
        scale: 0.33
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Custom menu icon doesn't appear | Check the `custom-menu` path in `mt-canvus.ini` is correct and the file exists at that path. Restart Canvus. |
| Icons show as default/missing | Verify the `icons/` folder is in the same directory as `menu.yml`. Icon paths in `menu.yml` are relative to `menu.yml` itself. |
| Embedded dashboard won't load / asks to log in | The Canvus browser widget is a fresh session — sign in once and the cookie persists for the lifetime of the widget. Some Vodafone SSO flows may require opening the URL in a normal browser first. |
| Path not working on Windows | Replace single backslashes (`\`) with forward slashes (`/`) or double backslashes (`\\`). |
