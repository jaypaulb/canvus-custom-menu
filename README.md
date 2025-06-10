# Custom Menu

A custom menu provides the capability to easily insert predefined custom content onto a canvas from the finger menu. You can do a particular action or insert PDFs, images, browsers, videos, and notes. Custom menu also supports sub-menus for easier categorisation. For example, you can have one sub-menu for PDFs and another one for videos, or you can have one sub-menu for each project you are working on.

## Adding Newlines in Note Text

When creating notes through the custom menu, you may want to include multiple lines of text. Based on testing, there are several effective ways to add newlines in the `text` parameter of a note:

### Method 1: YAML Block Scalar with Pipe Character (|)

The most reliable method is to use YAML's literal block scalar with the pipe character:

```yaml
text: |
  First line of text
  Second line of text
  Third line of text
```

This preserves all line breaks exactly as written and is the recommended approach.

### Method 2: YAML Literal Style with Dash (|-)

If you want to clip trailing newlines, use the literal style with a dash:

```yaml
text: |-
  First line of text
  Second line of text
  Third line of text
```

### Method 3: Double Quoted String with Explicit \n

You can also use double quotes with explicit newline characters:

```yaml
text: "First line of text\nSecond line of text\nThird line of text"
```

### Method 4: Indentation in Literal Block Scalar

You can preserve indentation in your text using the literal block scalar:

```yaml
text: |
  Main heading
    Indented point 1
    Indented point 2
  Back to main level
```

### Example in Context

Here's a complete example of a note creation with multiline text:

```yaml
- name: 'create'
  parameters:
    type: 'note'
    color: '#ffffff'
    coordinate-system: 'viewport'
    coordinate-offset: 'none'
    location: 50% 50%
    size: 20% 15%
    scale: 1
    text: |
      MEETING AGENDA
      
      1. Project Updates
      2. Budget Review
      3. Timeline Discussion
      4. Action Items
    pinned: true
```

### Important Notes

- The standard `\n` escape sequence in single quotes does not work reliably
- HTML tags like `<br>` are not interpreted as newlines
- Avoid using carriage returns (`\r`) or other special characters
- When using the literal block scalar (|), indentation is preserved in the output
- Make sure to maintain proper YAML indentation throughout your file

You can insert an item or a group of items with the tap of a button. It is possible to predefine the size, position, color and other attributes that are relevant to the items.

Custom menu
Custom menu in the finger menu.

Setting up a custom menu consists of two parts. The contents of the menu are specified in a separate configuration file in YAML format (see Defining a Custom Menu). This custom menu configuration file must also be specified in the Canvus client configuration file mt-canvus.ini (see Enabling a Custom Menu).

Enabling a Custom Menu
The custom menu is defined in a separate YAML that must be referenced from mt-canvus.ini.

[canvas]
; Custom menu file in YAML format used to display a user-defined menu
; structure on the finger menu.
; DEFAULT: empty
; NOTE: Backslashes in paths need special handling on Windows computers.
;       Replace each \ with \\ or /.
custom-menu=/path/to/menu.yml
Note

Canvus must be restarted after this setting has been modified.

Defining a Custom Menu
The contents of the custom menu are defined using a separate YAML file. See below for an example custom menu definition:

tooltip: 'Top-level menu'
icon: 'icons/custom.png'
items:

- tooltip: 'Open sub-menu 1'
    icon: 'icons/submenu.png'
    items:
  - tooltip: 'Note'
      icon: 'icons/note.png'
      actions:
    - name: 'create'
          parameters:
            type: note
            text: 'Note from sub menu1'
            color: 'DarkSeaGreen'
  - tooltip: 'Video'
      icon: 'icons/video.png'
      actions:
    - name: 'create'
          parameters:
            type: video
            source: 'docs/REDWOOD - Night Garden.mp4'
            title: 'Video from sub menu1'
Tip

To make editing the custom menu easier, any changes in the YAML file are automatically reloaded. It is not necessary to restart Canvus after making changes. Just remember to reopen the finger menu because Canvus fetches the content of the YAML file when the finger menu is opened.

The custom menu is specified in YAML format. The custom menu consists of menu-items which can be nested in a tree hierarchy.

Menu Items
A menu-item represents a single entry in the custom menu. When a menu-item is activated by tapping on it, one of the following can happen:

A sub-menu is opened

A sub-menu is closed

A list of actions are executed

Each menu-item can have the following attributes:

Menu-item attributes
Attribute

Description

icon

The icon displayed in the menu

icon-back

The icon that will be displayed for sub-menus to navigate backwards in the hiearchy

tooltip

Tooltip that will appear with a tap-and-hold gesture

items

List of sub-menu items that will appear when the menu-item is activated

actions

List of actions to perform when the menu-item is activated

The actions associated with a menu-item have additional parameters that depend on the type of the action. See Actions for more details.

Icon
To specify the icon that appears in the custom menu, use the icon attribute.

The icon attribute for a menu item is used to specify the location of an image file. Relative paths are relative to the custom menu YAML file itself.

If you don't specify an icon for a menu-item, Canvus will use a default icon and issue a warning.

Note

To appear correctly, each icon should be a transparent 937x937 PNG file, with the contents of the image occupying approximately the center 250x250 pixels.

Back Icon
When the user navigates inside a sub-menu, the parent menu-item will change into a back button that allows the user to return to the previous menu hierarchy. By default, the icon on the back button will not change. To specify a different icon to appear for the back button, use the back-icon attribute.

Custom menu back icon
Using the back-icon attribute to change the icon when going up the menu hierachy.

The back-icon attribute for a menu item is used to specify the location of an image file. Relative paths are relative to the custom menu YAML file itself.

Note

To appear correctly, each icon should be a transparent 937x937 PNG file, with the contents of the image occupying approximately the center 250x250 pixels.

Tooltip
To specify a tooltip that appears when the user performs a tap-and-hold gesture on a menu-item, use the tooltip attribute. The default value is empty.

The tooltip attribute should be a text string that will appear in a popup when the user performs a tap-and-gesture on the icon.

Custom menu with a tooltip
The tooltip is visible with tap-and-hold gesture on a menu-item.

Submenus
To make a menu-item trigger opening a sub-menu, specify a list of menu-items as the value of items attribute.

Tip

While each sub-menu can hold up to 24 items we don't recommend more than 18 items or things will get very crowded.

Actions
Actions are used to specify how content is created from a custom menu. Activating a menu-item can trigger one or more actions. The actions are specified using the actions attribute of a menu-item. The attribute can be used to define a list of actions to perform when the menu-item is activated.

The actions are specified by name and a list of parameters. The list of valid parameters for each action depends on the action itself. The names of valid actions are shown below:

Table of valid action names
Name

Description

create

Creates new content on the canvas

open-folder

Opens a folder widget to given folder

Create
The create action can be used to create a canvas item on the canvas.

- name: 'create'
    parameters:
      type: browser
      url: '<https://www.bbc.co.uk/weather>'
The following parameters are common for every create action:

type
The type of content to create from the action. The valid types are:

note - creates a new note

PDF - creates a new PDF

video - creates a new video

image - creates a new image

browser - creates a new browser

coordinate-system
Specifies the coordinate system in which size, scale, and location are defined in. Valid values are:

viewport (default) - the action is specified in the workspace's current viewport coordinates

canvas - the action is specified in absolute canvas coordinates

coordinate-offset
Specifies an offset for the location. Valid values are:

none - no offset

finger - the location of the finger activating the menu-item

menu (default) - the center location of the finger menu

origin
Specifies the origin of the created item. Specified as two decimal values for x and y coordinate respectively. An origin of 0 0 is the upper-left corner of the widget, 0.5 0.5 is the center of the widget and 1 1 is the bottom-right corner of the widget. The default is 0 0.

location
Location of the created item specified in coordinate-system offset by coordinate-offset. Specified as two values for x and y coordinate respectively. Affected by origin.

size
Size of the created item. Specified as two values for width and height. CSS units can be used, like 50% 50% or 100px 100px. Percentage values are relative to the specified coordinate-system.

scale
Scale of the item relative to the specified coordinate-system. Default value is 1.

pinned
Should the created item be pinned or not? Specified as boolean, true or false. The default value is false.

Additional parameters depend on the type of the action.

Creating Images
If the type of a create action is image, the following additional parameters are available:

source
Relative or absolute path to the image filename on the host computer. Relative paths are relative to the custom menu YAML file.

title
Title of the image.

Creating Videos
If the type of the create action is video, the following additional parameters are available:

source
Relative or absolute path to the video filename on the host computer. Relative paths are relative to the custom menu YAML file.

title
Title of the video.

Creating PDFs
If the type of the create action is pdf, the following additional parameters are available:

source
Relative or absolute path to the PDF filename on the host computer. Relative paths are relative to the custom menu YAML file.

title
Title of the PDF.

Creating Web Browsers
If the type of the create action is browser, the following additional parameters are available:

url
The URL for the web browser.

title
Title of the web browser.

transparent-mode
Accepts boolean values (true/false) as input. If set to true, transparency mode is turned on.

Creating Notes
If the type of the create action is note, the following additional parameters are available:

text
Text shown in the note. For multiline text, see the section ["Adding Newlines in Note Text"](#adding-newlines-in-note-text) at the beginning of this document for recommended approaches.

title
Title of the note.

color
Background color of the note.

Open Folder
To open a specific folder from the host computer, use the open-folder action.

- name: 'open-folder'
    parameters:
      source: '/home/multi/Big Video Files'
      title: 'My Videos'
The following parameters can be defined for the open-folder action:

source
Relative or absolute path to the folder to open on the host computer. Relative paths are relative to the custom menu YAML file.

title
Title of the folder to open.
