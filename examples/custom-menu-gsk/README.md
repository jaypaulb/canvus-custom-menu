# Custom Menu — GSK Example

This is a ready-to-use Canvus custom menu themed for GSK, including branded icons, sample content (images, PDFs, videos), and a fully configured `menu.yml`.

## Folder Contents

```
custom-menu-gsk/
├── menu.yml        ← The custom menu definition
├── icons/          ← Menu icons (937x937 transparent PNGs)
├── content/        ← Sample PDFs, images, and videos
└── README.md       ← This file
```

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

### Step 2: Extract the zip file

Extract the `custom-menu-gsk.zip` into a `menu/` folder alongside your `mt-canvus.ini`.

For example, if your `mt-canvus.ini` is at:

**Windows:**
```
%PROGRAMDATA%\MultiTaction\canvus\mt-canvus.ini
```

Extract to:
```
%PROGRAMDATA%\MultiTaction\canvus\menu\custom-menu-gsk\
```

**Linux:**
```
~/MultiTaction/canvus/mt-canvus.ini
```

Extract to:
```
~/MultiTaction/canvus/menu/custom-menu-gsk/
```

After extraction, the folder structure should look like this:

```
MultiTaction/canvus/
├── mt-canvus.ini
└── menu/
    └── custom-menu-gsk/
        ├── menu.yml
        ├── icons/
        └── content/
```

### Step 3: Update `mt-canvus.ini`

Open `mt-canvus.ini` in a text editor and add (or update) the `custom-menu` line under the `[canvas]` section:

**Windows:**
```ini
[canvas]
custom-menu=menu/custom-menu-gsk/menu.yml
```

**Linux:**
```ini
[canvas]
custom-menu=menu/custom-menu-gsk/menu.yml
```

> **Windows path note:** Use forward slashes (`/`) or double backslashes (`\\`) in the path. Single backslashes will not work.
>
> The path is **relative to the location of `mt-canvus.ini`**, so `menu/custom-menu-gsk/menu.yml` works as long as you extracted the zip into the `menu/` folder as described above.

### Step 4: Restart Canvus

**Canvus must be restarted** after modifying `mt-canvus.ini` for the change to take effect.

Once restarted, the custom menu icon will appear in the finger menu on the canvas.

## Making Changes

After the initial setup, you can edit `menu.yml` at any time — Canvus will pick up changes automatically when you next open the finger menu. **No restart is needed for menu.yml changes**, only for `mt-canvus.ini` changes.

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Custom menu icon doesn't appear | Check the `custom-menu` path in `mt-canvus.ini` is correct and the file exists at that path. Restart Canvus. |
| Icons show as default/missing | Verify the `icons/` folder is in the same directory as `menu.yml`. Icon paths in `menu.yml` are relative to `menu.yml` itself. |
| Content (PDFs, videos) won't load | Verify the `content/` folder is in the same directory as `menu.yml`. Check file permissions. |
| Path not working on Windows | Replace single backslashes (`\`) with forward slashes (`/`) or double backslashes (`\\`). |
