# Canvus Custom Menu Manual

## Table of Contents

1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [Menu Structure](#menu-structure)
4. [Menu Items](#menu-items)
5. [Actions](#actions)
6. [Content Types](#content-types)
7. [Positioning and Layout](#positioning-and-layout)
8. [Advanced Features](#advanced-features)
9. [Best Practices](#best-practices)
10. [Examples](#examples)
11. [Troubleshooting](#troubleshooting)

---

## Introduction

### What is a Custom Menu?

A custom menu provides the capability to easily insert predefined custom content onto a canvas from the finger menu. You can perform specific actions or insert PDFs, images, browsers, videos, and notes with a single tap. Custom menus also support sub-menus for easier categorization, allowing you to organize content by project, content type, or any other logical grouping.

### Key Benefits

- **Quick Access**: Insert frequently used content with a single tap
- **Consistency**: Predefine size, position, color, and other attributes
- **Organization**: Use sub-menus to categorize content logically
- **Efficiency**: Reduce repetitive tasks and streamline workflows
- **Customization**: Tailor menus to specific projects, teams, or use cases

### Use Cases

- **Facilitation**: Quick access to templates for SWOT analysis, stakeholder mapping, process mapping, etc.
- **Asset Management**: Organize and quickly insert company logos, documents, videos
- **Project Templates**: Create project-specific menus with relevant content
- **Brand Consistency**: Ensure brand assets are easily accessible and consistently used
- **Training Materials**: Quick access to training documents and resources

---

## Getting Started

### Enabling a Custom Menu

The custom menu is defined in a separate YAML file that must be referenced from the Canvus client configuration file `mt-canvus.ini`.

#### Step 1: Locate the Configuration File

Find the `mt-canvus.ini` file on your system. This is typically located in the Canvus installation directory or user configuration folder.

#### Step 2: Configure the Custom Menu Path

Add or modify the `custom-menu` setting in the `[canvas]` section:

```ini
[canvas]
; Custom menu file in YAML format used to display a user-defined menu
; structure on the finger menu.
; DEFAULT: empty
; NOTE: Backslashes in paths need special handling on Windows computers.
;       Replace each \ with \\ or /.
custom-menu=/path/to/menu.yml
```

**Important Notes:**
- Use forward slashes (`/`) or double backslashes (`\\`) in Windows paths
- The path can be absolute or relative to the configuration file
- Ensure the path is correct and the file exists

#### Step 3: Restart Canvus

**Canvus must be restarted after modifying the `mt-canvus.ini` file** for the changes to take effect.

### Creating Your First Menu File

1. Create a new YAML file (e.g., `menu.yml`)
2. Use the basic structure shown below
3. Save the file and reference it in `mt-canvus.ini`
4. Restart Canvus

**Basic Menu Structure:**

```yaml
tooltip: 'Custom Menu'
icon: 'icons/custom-menu.png'
items:
  - tooltip: 'My First Item'
    icon: 'icons/note.png'
    actions:
      - name: 'create'
        parameters:
          type: 'note'
          text: 'Hello, Canvus!'
          color: '#ffffff'
```

### Hot Reloading

**Good News**: Once the menu is enabled, you don't need to restart Canvus after making changes to the YAML file. The menu is automatically reloaded when you:

1. Make changes to the YAML file
2. Close the finger menu (if open)
3. Reopen the finger menu

Canvus fetches the content of the YAML file when the finger menu is opened, so just remember to reopen the finger menu to see your changes.

---

## Menu Structure

### Top-Level Structure

Every custom menu file must have the following top-level structure:

```yaml
tooltip: 'Menu Tooltip Text'
icon: 'path/to/icon.png'
items:
  # List of menu items here
```

#### Top-Level Attributes

| Attribute | Required | Description |
|-----------|----------|-------------|
| `tooltip` | Yes | Text displayed when hovering over the menu icon |
| `icon` | Yes | Path to the icon image file (relative to YAML file) |
| `items` | Yes | List of menu items (can be empty) |

### Hierarchical Structure

Custom menus support unlimited nesting levels. Each menu item can contain:

- **Sub-items** (`items`): Creates a sub-menu
- **Actions** (`actions`): Performs operations when clicked
- **Both**: Can have sub-items and actions (sub-items take precedence)

**Example Structure:**

```
Custom Menu (root)
├── Sub-Menu 1
│   ├── Item 1.1 (action)
│   ├── Item 1.2 (action)
│   └── Sub-Menu 1.3
│       ├── Item 1.3.1 (action)
│       └── Item 1.3.2 (action)
├── Item 2 (action)
└── Sub-Menu 3
    └── Item 3.1 (action)
```

---

## Menu Items

### Menu Item Attributes

Each menu item can have the following attributes:

| Attribute | Required | Type | Description |
|-----------|----------|------|-------------|
| `tooltip` | Yes | String | Text shown on tap-and-hold gesture |
| `icon` | Recommended | String | Path to icon image (relative to YAML file) |
| `icon-back` | No | String | Icon for back navigation in sub-menus |
| `items` | No* | List | Sub-menu items (creates sub-menu) |
| `actions` | No* | List | Actions to perform when clicked |

\* Either `items` or `actions` (or both) must be present

### Icon Requirements

**Icon Specifications:**
- **Format**: PNG with transparency
- **Size**: 937x937 pixels
- **Content Area**: Image content should occupy approximately the center 250x250 pixels
- **Background**: Transparent

**Icon Paths:**
- Relative paths are relative to the YAML file location
- Use forward slashes (`/`) in paths for cross-platform compatibility
- If no icon is specified, Canvus uses a default icon and issues a warning

**Example:**

```yaml
- tooltip: 'My Menu Item'
  icon: 'icons/my-icon.png'  # Relative to menu.yml location
```

### Tooltips

Tooltips appear when users perform a tap-and-hold gesture on a menu item. They provide additional context about what the item does.

```yaml
- tooltip: 'Create a new note'
  icon: 'icons/note.png'
  actions:
    # ...
```

### Sub-Menus

To create a sub-menu, use the `items` attribute with a list of menu items:

```yaml
- tooltip: 'Documents'
  icon: 'icons/folder.png'
  items:
    - tooltip: 'User Manual'
      icon: 'icons/document.png'
      actions:
        - name: 'create'
          parameters:
            type: 'pdf'
            source: 'content/manual.pdf'
    - tooltip: 'Installation Guide'
      icon: 'icons/document.png'
      actions:
        - name: 'create'
          parameters:
            type: 'pdf'
            source: 'content/install.pdf'
```

**Sub-Menu Best Practices:**
- Keep sub-menus to 18 items or fewer (maximum 24, but becomes crowded)
- Use logical groupings (by content type, project, function, etc.)
- Consider depth vs. breadth (fewer levels with more items vs. more levels with fewer items)

### Back Icon

When navigating into a sub-menu, the parent menu item becomes a back button. By default, it keeps the same icon. To specify a different icon for the back button:

```yaml
- tooltip: 'Documents'
  icon: 'icons/folder.png'
  icon-back: 'icons/back.png'  # Custom back icon
  items:
    # ...
```

---

## Actions

### Action Types

Actions define what happens when a menu item is activated. There are two types of actions:

1. **`create`**: Creates new content on the canvas
2. **`open-folder`**: Opens a folder widget to a specified folder

### Create Action

The `create` action is used to create canvas items. It supports multiple content types:

- `note` - Creates a new note
- `pdf` - Creates a new PDF document
- `video` - Creates a new video player
- `image` - Creates a new image
- `browser` - Creates a new web browser

**Basic Structure:**

```yaml
actions:
  - name: 'create'
    parameters:
      type: 'note'  # or 'pdf', 'video', 'image', 'browser'
      # Additional parameters based on type
```

### Common Create Parameters

These parameters apply to all `create` actions:

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `type` | Yes | - | Content type: `note`, `pdf`, `video`, `image`, `browser` |
| `coordinate-system` | No | `viewport` | Coordinate system: `viewport` or `canvas` |
| `coordinate-offset` | No | `menu` | Offset type: `none`, `finger`, or `menu` |
| `origin` | No | `0 0` | Origin point: `0 0` (top-left), `0.5 0.5` (center), `1 1` (bottom-right) |
| `location` | No | - | Position as `x y` (affected by origin and offset) |
| `size` | No | - | Size as `width height` (supports `%` or `px` units) |
| `scale` | No | `1` | Scale factor relative to coordinate system |
| `pinned` | No | `false` | Whether item is pinned (`true` or `false`) |

### Coordinate Systems

**`viewport` (default)**: Coordinates are relative to the current viewport
- Useful for positioning relative to what's currently visible
- Changes as you pan/zoom the canvas

**`canvas`**: Coordinates are absolute on the canvas
- Useful for fixed positions regardless of viewport
- Maintains position even when panning/zooming

### Coordinate Offsets

**`menu` (default)**: Location is offset from the center of the finger menu
- Items appear near where the menu was opened

**`finger`**: Location is offset from the finger position that activated the menu
- Items appear at or near where you tapped

**`none`**: No offset, uses absolute coordinates
- Items appear at exact specified coordinates

### Origin

The origin point determines which part of the created item is positioned at the specified location:

- `0 0`: Top-left corner (default)
- `0.5 0.5`: Center of the item
- `1 1`: Bottom-right corner

**Example:**

```yaml
origin: 0.5, 0.5  # Center the item at the location
location: 50% 50%  # Item center will be at viewport center
```

### Size

Size can be specified in:
- **Percentage**: `50% 50%` (relative to coordinate system)
- **Pixels**: `800px 600px` (absolute size)
- **Mixed**: `50% 300px` (width as percentage, height in pixels)

**Examples:**

```yaml
size: 50% 50%        # Half of viewport width and height
size: 800px 600px    # Fixed 800x600 pixels
size: 100% 300px     # Full width, 300px height
```

### Scale

Scale factor applied to the item. Default is `1` (100% size).

```yaml
scale: 0.5   # 50% of normal size
scale: 1.5   # 150% of normal size
scale: 0.33  # 33% of normal size (common for thumbnails)
```

### Pinned

When `pinned: true`, the item cannot be moved or deleted easily, providing a fixed reference point.

```yaml
pinned: true   # Item is pinned
pinned: false  # Item can be moved (default)
```

---

## Content Types

### Creating Notes

Notes are text-based items with customizable colors and content.

**Required Parameters:**
- `type: 'note'`
- `text`: The note content

**Optional Parameters:**
- `color`: Background color (hex code, color name, or `transparent`)
- `title`: Title of the note

**Example:**

```yaml
- name: 'create'
  parameters:
    type: 'note'
    text: 'This is a simple note'
    color: '#ffffff'
    location: 50% 50%
    size: 20% 15%
```

**Multiline Text:**

For multiline text, use YAML's literal block scalar with the pipe character (`|`):

```yaml
text: |
  First line of text
  Second line of text
  Third line of text
```

**Color Options:**
- Hex codes: `#ffffff`, `#ff0000`, `#e6b3b31a` (with transparency)
- Color names: `red`, `blue`, `green`, `transparent`
- Named colors: `DarkSeaGreen`, `LightBlue`, etc.

**Note:** The `1a` suffix in hex codes (e.g., `#e6b3b31a`) represents 85% transparency.

### Creating PDFs

PDFs display document content on the canvas.

**Required Parameters:**
- `type: 'pdf'`
- `source`: Path to PDF file (relative to YAML file or absolute)

**Optional Parameters:**
- `title`: Title of the PDF
- `scale`: Scale factor (commonly `0.33` for thumbnails)

**Example:**

```yaml
- name: 'create'
  parameters:
    type: 'pdf'
    source: 'content/user-manual.pdf'
    title: 'User Manual'
    scale: 0.33
```

### Creating Videos

Videos create a video player on the canvas.

**Required Parameters:**
- `type: 'video'`
- `source`: Path to video file (relative to YAML file or absolute)

**Optional Parameters:**
- `title`: Title of the video
- `scale`: Scale factor

**Example:**

```yaml
- name: 'create'
  parameters:
    type: 'video'
    source: 'content/demo-video.mp4'
    title: 'Product Demo'
    scale: 0.5
```

**Supported Formats:**
- MP4 (recommended)
- Other formats supported by the system's media player

### Creating Images

Images display image files on the canvas.

**Required Parameters:**
- `type: 'image'`
- `source`: Path to image file (relative to YAML file or absolute)

**Optional Parameters:**
- `title`: Title of the image
- `scale`: Scale factor

**Example:**

```yaml
- name: 'create'
  parameters:
    type: 'image'
    source: 'content/logo.png'
    title: 'Company Logo'
    scale: 0.33
```

**Supported Formats:**
- PNG (recommended for transparency)
- JPEG
- Other formats supported by the system

### Creating Web Browsers

Browsers create a web browser widget on the canvas.

**Required Parameters:**
- `type: 'browser'`
- `url`: The URL to load

**Optional Parameters:**
- `title`: Title of the browser
- `size`: Browser window size (e.g., `1366px 768px`)
- `transparent-mode`: Enable transparency (`true` or `false`)
- `origin`: Origin point (commonly `0.5, 0.5` for centering)
- `scale`: Scale factor

**Example:**

```yaml
- name: 'create'
  parameters:
    type: 'browser'
    url: 'https://www.example.com'
    title: 'Example Website'
    origin: 0.5, 0.5
    size: 1366px, 768px
    scale: 0.5
    pinned: true
```

**Common Browser Sizes:**

```yaml
# Standard sizes
size: 800px, 600px      # 4:3
size: 1024px, 768px     # 4:3
size: 1280px, 720px     # 16:9
size: 1366px, 768px     # 16:9
size: 1920px, 1080px    # Full HD
size: 3840px, 2160px    # 4K

# Mobile sizes
size: 375px, 667px      # iPhone 8
size: 390px, 844px      # iPhone 12
size: 448px, 998px      # Pixel 8

# Ultrawide
size: 2560px, 1080px    # 21:9
size: 5120px, 2160px    # 21:9
```

### Open Folder Action

The `open-folder` action opens a folder widget to a specified directory.

**Required Parameters:**
- `name: 'open-folder'`
- `source`: Path to folder (relative to YAML file or absolute)
- `title`: Title of the folder

**Example:**

```yaml
- name: 'open-folder'
  parameters:
    source: '/home/user/Documents/Project Files'
    title: 'Project Files'
```

**Path Notes:**
- Use forward slashes (`/`) for cross-platform compatibility
- On Windows, you can use `/` or `\\` (double backslashes)
- Relative paths are relative to the YAML file location

---

## Positioning and Layout

### Understanding Coordinates

Positioning in Canvus uses a coordinate system where:
- **X-axis**: Left to right (0% = left edge, 100% = right edge)
- **Y-axis**: Top to bottom (0% = top edge, 100% = bottom edge)

### Viewport vs Canvas Coordinates

**Viewport Coordinates** (default):
- Relative to what's currently visible
- `50% 50%` = center of visible area
- Changes as you pan/zoom

**Canvas Coordinates**:
- Absolute position on the entire canvas
- `50% 50%` = center of entire canvas
- Fixed regardless of viewport position

### Common Positioning Patterns

**Center of Viewport:**
```yaml
coordinate-system: 'viewport'
coordinate-offset: 'none'
origin: 0.5, 0.5
location: 50% 50%
```

**At Finger Position:**
```yaml
coordinate-system: 'viewport'
coordinate-offset: 'finger'
origin: 0.5, 0.5
location: 0, 0
```

**Grid Layout:**
```yaml
# Top-left quadrant
location: 25% 25%
size: 45% 45%

# Top-right quadrant
location: 50% 25%
size: 45% 45%

# Bottom-left quadrant
location: 25% 50%
size: 45% 45%

# Bottom-right quadrant
location: 50% 50%
size: 45% 45%
```

### Creating Note Stacks

To create overlapping notes (stacks), use the same location with different `pinned` values:

```yaml
# Base note (pinned)
- name: 'create'
  parameters:
    type: 'note'
    color: '#3aaa34'
    location: 0% 10%
    size: 15% 30%
    pinned: true
    text: 'Base Note'

# Stacked notes (not pinned, can be moved)
- name: 'create'
  parameters:
    type: 'note'
    color: '#3aaa34'
    location: 0% 10%  # Same location
    size: 15% 30%
    pinned: false     # Can be moved
    text: ''
```

### Creating Templates

Templates often use multiple actions to create a complete layout:

```yaml
actions:
  # Title note
  - name: 'create'
    parameters:
      type: 'note'
      color: '#ffffff'
      location: 5% 0%
      size: 90% 10%
      text: 'TEMPLATE TITLE'
      pinned: true

  # Content area 1
  - name: 'create'
    parameters:
      type: 'note'
      color: '#e6b3b3'
      location: 15% 10%
      size: 35% 45%
      text: 'Section 1'
      pinned: true

  # Content area 2
  - name: 'create'
    parameters:
      type: 'note'
      color: '#b3d9c6'
      location: 50% 10%
      size: 35% 45%
      text: 'Section 2'
      pinned: true
```

---

## Advanced Features

### Multiple Actions

A single menu item can trigger multiple actions, creating multiple items at once:

```yaml
- tooltip: 'Create Template'
  icon: 'icons/template.png'
  actions:
    - name: 'create'
      parameters:
        type: 'note'
        # First note
    - name: 'create'
      parameters:
        type: 'note'
        # Second note
    - name: 'create'
      parameters:
        type: 'image'
        # Image
```

**Execution Order:** Actions are executed in the order they appear in the YAML file.

### Conditional Organization

Use comments and logical grouping to organize complex menus:

```yaml
items:
  # ======================================================================
  # Section: Facilitation Tools
  # ======================================================================
  - tooltip: 'SWOT Analysis'
    icon: 'icons/swot.png'
    actions:
      # ...

  # ======================================================================
  # Section: Assets
  # ======================================================================
  - tooltip: 'Logos'
    icon: 'icons/logos.png'
    items:
      # ...
```

### Color Transparency

Use hex codes with alpha channel for transparency:

```yaml
color: '#e6b3b31a'  # Light pink with 85% transparency
color: '#ffffff00'  # Fully transparent white
color: '#00000080'  # 50% transparent black
```

**Transparency Values:**
- `00` = Fully transparent
- `1a` = ~85% transparent (common for backgrounds)
- `80` = 50% transparent
- `ff` = Fully opaque

### Relative Paths

All file paths in the YAML are relative to the YAML file's location:

```
menu.yml
├── icons/
│   └── note.png
├── content/
│   ├── document.pdf
│   └── video.mp4
└── subfolder/
    └── menu.yml
```

**In menu.yml:**
```yaml
icon: 'icons/note.png'           # Relative to menu.yml
source: 'content/document.pdf'   # Relative to menu.yml
```

**In subfolder/menu.yml:**
```yaml
icon: '../icons/note.png'        # Go up one level
source: '../content/document.pdf'
```

---

## Best Practices

### Menu Organization

1. **Logical Grouping**: Group related items together
   - By content type (Documents, Videos, Images)
   - By project or client
   - By function (Facilitation, Assets, Templates)

2. **Limit Depth**: Avoid too many nested levels (3-4 levels maximum)

3. **Limit Items**: Keep sub-menus to 18 items or fewer

4. **Clear Naming**: Use descriptive tooltips and organize with comments

### Icon Management

1. **Consistent Style**: Use icons with consistent style and size
2. **Meaningful Icons**: Choose icons that clearly represent the action
3. **Organize Files**: Keep icons in a dedicated `icons/` folder
4. **Naming Convention**: Use descriptive, consistent file names

### Content Organization

1. **Dedicated Folders**: Organize content files in folders:
   ```
   menu.yml
   ├── icons/
   ├── content/
   │   ├── documents/
   │   ├── videos/
   │   ├── images/
   │   └── pdfs/
   ```

2. **Version Control**: Use version control for menu files (but not content files)

3. **Backup**: Keep backups of working menu configurations

### Performance

1. **File Sizes**: Keep media files reasonably sized
2. **Lazy Loading**: Don't load all content at once
3. **Efficient Paths**: Use relative paths for portability

### Maintenance

1. **Documentation**: Add comments explaining complex configurations
2. **Testing**: Test menus after changes
3. **Incremental Changes**: Make small, testable changes
4. **Versioning**: Keep versions of menu files for rollback

### Template Design

1. **Consistent Spacing**: Use consistent margins and spacing
2. **Color Coding**: Use colors consistently (e.g., green for strengths, red for weaknesses)
3. **Readable Text**: Ensure text is readable with appropriate contrast
4. **Pinned Elements**: Pin template elements that shouldn't be moved
5. **Boundary Awareness**: Keep content within viewport boundaries (use 2-5% margins)

---

## Examples

### Example 1: Simple Note Creation

```yaml
tooltip: 'Custom Menu'
icon: 'icons/custom-menu.png'
items:
  - tooltip: 'Create Note'
    icon: 'icons/note.png'
    actions:
      - name: 'create'
        parameters:
          type: 'note'
          text: 'Hello, Canvus!'
          color: '#ffffff'
          coordinate-offset: 'finger'
          origin: 0.5, 0.5
          location: 0, 0
```

### Example 2: Document Library

```yaml
tooltip: 'Custom Menu'
icon: 'icons/custom-menu.png'
items:
  - tooltip: 'Documents'
    icon: 'icons/folder.png'
    items:
      - tooltip: 'User Manual'
        icon: 'icons/document.png'
        actions:
          - name: 'create'
            parameters:
              type: 'pdf'
              source: 'content/user-manual.pdf'
              title: 'User Manual'
              scale: 0.33
      - tooltip: 'Installation Guide'
        icon: 'icons/document.png'
        actions:
          - name: 'create'
            parameters:
              type: 'pdf'
              source: 'content/install-guide.pdf'
              title: 'Installation Guide'
              scale: 0.33
```

### Example 3: Logo Collection

```yaml
tooltip: 'Custom Menu'
icon: 'icons/custom-menu.png'
items:
  - tooltip: 'Logos'
    icon: 'icons/logos.png'
    items:
      - tooltip: 'Company Logo'
        icon: 'icons/company.png'
        actions:
          - name: 'create'
            parameters:
              type: 'image'
              source: 'content/logo.png'
              title: 'Company Logo'
              scale: 0.33
      - tooltip: 'Partner Logos'
        icon: 'icons/partners.png'
        items:
          - tooltip: 'Partner A'
            icon: 'icons/partner-a.png'
            actions:
              - name: 'create'
                parameters:
                  type: 'image'
                  source: 'content/partner-a.png'
                  scale: 0.33
```

### Example 4: Browser Presets

```yaml
tooltip: 'Custom Menu'
icon: 'icons/custom-menu.png'
items:
  - tooltip: 'Web Browsers'
    icon: 'icons/browser.png'
    items:
      - tooltip: 'Standard Sizes'
        icon: 'icons/16-9.png'
        items:
          - tooltip: '1920x1080'
            icon: 'icons/4.png'
            actions:
              - name: 'create'
                parameters:
                  type: 'browser'
                  url: 'https://www.example.com'
                  origin: 0.5, 0.5
                  size: 1920px, 1080px
                  scale: 0.33
```

### Example 5: Template with Multiple Elements

```yaml
tooltip: 'Custom Menu'
icon: 'icons/custom-menu.png'
items:
  - tooltip: 'Templates'
    icon: 'icons/template.png'
    items:
      - tooltip: 'Meeting Template'
        icon: 'icons/meeting.png'
        actions:
          # Title
          - name: 'create'
            parameters:
              type: 'note'
              color: '#ffffff'
              location: 5% 0%
              size: 90% 10%
              text: |
                MEETING NOTES
                Date: [Date]
                Attendees: [List]
              pinned: true

          # Agenda
          - name: 'create'
            parameters:
              type: 'note'
              color: '#e6f3ff'
              location: 5% 15%
              size: 40% 30%
              text: |
                AGENDA
                1.
                2.
                3.
              pinned: true

          # Action Items
          - name: 'create'
            parameters:
              type: 'note'
              color: '#ffe6e6'
              location: 50% 15%
              size: 45% 30%
              text: |
                ACTION ITEMS
                - [ ]
                - [ ]
                - [ ]
              pinned: true
```

### Example 6: Complete Organization Menu

See the example menus in the `Example custom menus/` directory:
- `custom-menu - Deloitte/`
- `custom-menu - GSK/`
- `custom-menu - KPMG/`

These provide real-world examples of complete menu structures for different organizations.

---

## Troubleshooting

### Menu Not Appearing

**Problem**: Custom menu doesn't appear in finger menu.

**Solutions**:
1. Check that `custom-menu` path in `mt-canvus.ini` is correct
2. Verify the YAML file exists at the specified path
3. Ensure Canvus has been restarted after configuration change
4. Check YAML syntax for errors (use a YAML validator)
5. Verify file permissions allow Canvus to read the file

### Icons Not Displaying

**Problem**: Icons show as default or missing.

**Solutions**:
1. Verify icon file paths are correct (relative to YAML file)
2. Check that icon files exist
3. Ensure icons are PNG format, 937x937 pixels
4. Verify file permissions
5. Check for typos in icon paths (case-sensitive on some systems)

### Content Not Creating

**Problem**: Menu item doesn't create content when clicked.

**Solutions**:
1. Check YAML syntax (indentation, quotes, etc.)
2. Verify `actions` section is properly formatted
3. Check that `name: 'create'` is correct (not `action: 'create'`)
4. Verify file paths for content (PDFs, videos, images) are correct
5. Check Canvus console/logs for error messages

### Path Issues

**Problem**: Files not found (icons, content).

**Solutions**:
1. Use relative paths (relative to YAML file location)
2. On Windows, use `/` or `\\` (double backslashes)
3. Avoid spaces in paths (or quote them properly)
4. Check case sensitivity (Linux/Mac are case-sensitive)
5. Verify file extensions are correct

### YAML Syntax Errors

**Problem**: Menu fails to load, YAML errors.

**Common Issues**:
1. **Indentation**: YAML is sensitive to indentation (use spaces, not tabs)
2. **Quotes**: Use quotes for strings with special characters
3. **Colons**: Ensure colons have proper spacing
4. **Lists**: Use `-` for list items with proper indentation

**Example of Correct Syntax**:
```yaml
items:
  - tooltip: 'Item 1'    # Correct
    icon: 'icons/1.png'
    actions:
      - name: 'create'
        parameters:
          type: 'note'
```

**Example of Incorrect Syntax**:
```yaml
items:
- tooltip: 'Item 1'     # Wrong indentation
icon: 'icons/1.png'     # Wrong indentation
```

### Performance Issues

**Problem**: Menu is slow to load or respond.

**Solutions**:
1. Reduce number of items in sub-menus
2. Optimize icon file sizes
3. Avoid deeply nested menus
4. Check for large content files being loaded
5. Consider splitting into multiple menu files

### Changes Not Appearing

**Problem**: Changes to YAML file don't show up.

**Solutions**:
1. Close and reopen the finger menu (menu reloads on open)
2. Check that file was saved
3. Verify you're editing the correct file
4. Check file permissions
5. Restart Canvus if hot reload isn't working

### Coordinate Issues

**Problem**: Items appear in wrong location or off-screen.

**Solutions**:
1. Check `coordinate-system` setting (`viewport` vs `canvas`)
2. Verify `coordinate-offset` setting (`none`, `finger`, `menu`)
3. Check `origin` values (0-1 range)
4. Verify `location` values are percentages (0-100%) or pixels
5. Test with simple values first (e.g., `50% 50%`)

### Color Issues

**Problem**: Colors not displaying correctly.

**Solutions**:
1. Use hex codes for precise colors: `#ffffff`
2. For transparency, append alpha: `#ffffff1a`
3. Verify color names are valid (if using named colors)
4. Check that `transparent` is lowercase
5. Test with simple colors first

### Multiline Text Issues

**Problem**: Newlines not appearing in notes.

**Solutions**:
1. Use YAML literal block scalar with `|`:
   ```yaml
   text: |
     Line 1
     Line 2
   ```
2. Avoid single quotes with `\n` (doesn't work reliably)
3. Don't use HTML tags like `<br>`
4. Ensure proper indentation in YAML

---

## Additional Resources

### Example Menus

- `menu.yml` - Main example menu with comprehensive examples
- `Example custom menus/` - Organization-specific examples
- `other menus/` - Alternative menu configurations

### Related Documentation

- `README.md` - Basic custom menu documentation
- `tasks.md` - Detailed descriptions of facilitation tasks

### Getting Help

1. Check YAML syntax with online validators
2. Review example menus for patterns
3. Test with simple examples first
4. Check Canvus logs for error messages
5. Consult Canvus documentation for system-specific issues

---

## Quick Reference

### Menu Structure Template

```yaml
tooltip: 'Menu Name'
icon: 'icons/menu-icon.png'
items:
  - tooltip: 'Item Name'
    icon: 'icons/item-icon.png'
    actions:
      - name: 'create'
        parameters:
          type: 'note'  # or 'pdf', 'video', 'image', 'browser'
          # Add type-specific parameters
```

### Common Parameter Values

```yaml
coordinate-system: 'viewport'  # or 'canvas'
coordinate-offset: 'menu'      # or 'finger', 'none'
origin: 0.5, 0.5              # 0-1 range
location: 50% 50%             # percentages or pixels
size: 50% 50%                 # percentages or pixels
scale: 1                      # scale factor
pinned: false                  # true or false
```

### File Path Examples

```yaml
# Relative to YAML file
icon: 'icons/note.png'
source: 'content/document.pdf'

# Absolute path (Windows)
source: 'C:/Users/Name/Documents/file.pdf'

# Absolute path (Linux/Mac)
source: '/home/user/documents/file.pdf'
```

---

**Last Updated**: Based on Canvus 1.7 Custom Menu functionality

**Version**: 1.0

