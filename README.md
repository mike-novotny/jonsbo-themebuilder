# Jonsbo 916 Theme Builder

A browser-based visual editor for creating and editing custom themes for the **Jonsbo 916** series LCD display panels. Design themes with a live preview, import existing themes to modify them, and export a ready-to-use ZIP with the correct folder structure.

![Theme Builder Screenshot](screenshot.png)

> **Browser requirement:** Chrome or Edge is strongly recommended. Firefox lacks support for folder import and system font loading. All other features work in any modern browser.

---

## Features

- **Visual canvas editor** — 462×1920 (vertical) or 1920×462 (horizontal) canvas with zoom and fit-to-window
- **Drag to place, move, and resize** every element — or use arrow keys to nudge (Shift+arrow = 10px)
- **Drag and drop** elements from the sidebar directly onto the canvas
- **Live preview** — all elements show realistic sample sensor values so you can see the result before deploying
- **All element types supported:**
  - Sensor text (live data with optional prefix and unit)
  - Static labels (fixed custom text)
  - Progress bars (horizontal or vertical, fully styled with animation)
  - Ring gauges (circular arc, RingProgressBar) with optional centered label
  - Decorative blocks (solid color panels and dividers)
  - Image layers (back.png, d1.png, d2.png, etc.)
- **Full sensor library** — every data binding discovered across all built-in themes
- **Font picker** — built-in theme fonts, system font loader (Chrome/Edge), custom font entry, and font file upload for export
- **Theme color palette** — save and reuse colors across all element types
- **Snap & Align** — center on canvas, snap to edges, snap-while-dragging guide lines, align elements to each other
- **Import themes** — load existing themes from a ZIP, a Setting.txt file, a theme folder, or the entire Programme folder
- **ZIP export** — downloads the complete theme folder structure including images, fonts, and auto-captured demo.png

---

## Importing an Existing Theme

There are four ways to open an existing theme for editing. All import options are in the toolbar.

### 📂 Import Theme — ZIP or Setting.txt

Click **Import Theme** to open a single file:

- **ZIP file** — a full theme package (e.g. exported from this builder). All images, the Setting.txt, and the theme name are loaded automatically.
- **Setting.txt** — just the layout file on its own. All elements are loaded. Image layers will appear as placeholders; use the **Background** and **Add Image Layer** buttons to load image files manually afterward.

### 📁 Import Folder — Single theme or entire Programme folder

Click **Import Folder** to point the browser at a folder on your machine. This works in two ways:

**Single theme folder** — navigate to a theme folder such as:
```
C:\Users\<YourUsername>\AppData\Local\JONSBO-AIO\Programme\MyTheme\
```
The builder reads `Setting.txt`, `back.png` (or video background), and all image layers (`d1.png`, `d2.png`, etc.) from both the root and `source\` subfolder, and loads the theme immediately.

**Programme folder** — navigate directly to:
```
C:\Users\<YourUsername>\AppData\Local\JONSBO-AIO\Programme\
```
The builder scans every subfolder, finds all valid themes (any folder containing a `Setting.txt`), and shows a **visual picker** with each theme's name, file count, and `demo.png` thumbnail. Click any theme to load it.

> **Finding the Programme folder quickly:** Press `Win + R`, type `%LocalAppData%\JONSBO-AIO\Programme` and press Enter. This opens the folder directly in Explorer. Then switch to the Theme Builder and click Import Folder.

> **Browser note:** Folder import uses the `webkitdirectory` API, which is supported in Chrome and Edge. Firefox may have limitations with deeply nested folders.

### What gets imported

| Element in Setting.txt | Imported as |
|------------------------|-------------|
| `Text:` (sensor data) | Sensor text element |
| `Text:` with `IsDefaultText@true` | Static label |
| `BorderLine:` with `data@` | Progress bar |
| `BorderLine:` without `data@` | Decorative block |
| `RingProgressBar:` | Ring gauge |
| `d1.png:`, `d2.png:` etc. | Image layers |
| `back.png` | Background image |
| `name:` field | Theme name (populates the name input) |
| `width:` field | Orientation (462 = Vertical, 1920 = Horizontal) |

Ring gauges and their centered text labels are automatically re-linked on import — if a `Text` element shares the same `data@` key as a `RingProgressBar` and has an adjacent z-value, they are treated as a linked pair and move/resize together in the editor.

---

## Creating a New Theme

1. Download `ThemeBuilder.html`
2. Open it in Chrome or Edge
3. Choose **▯ Vertical** or **▭ Horizontal** orientation
4. Click **💾 Export ZIP** when prompted — enter a theme name (no spaces or special characters)
5. Click **🖼 Background** to load your background file — PNG, JPG, GIF (animated), MP4, AVI, or WMV
6. Add elements by clicking items in the left sidebar, or drag them directly onto the canvas
7. Click an element to select it — drag to move, drag the gold corner handle to resize
8. Edit all properties in the right panel: position, size, colors, font, sensor binding, z-order
9. Use **Snap & Align** in the position section to center or align elements to each other
10. Click **💾 Export ZIP** when done — a clean `demo.png` preview is captured automatically

---

## Installing a Theme on the Device

> **Important:** Fully close the Jonsbo AIO application before copying theme files. Check the system tray (bottom-right taskbar, click the `^` arrow) and right-click the Jonsbo icon → **Exit**. Simply closing the main window leaves it running in the background.

1. Export your theme as a ZIP from the Theme Builder, or prepare your theme folder manually
2. Extract the ZIP (if applicable) — you will get a folder named after your theme
3. Copy the entire theme folder into the Jonsbo programme directory:

```
C:\Users\<YourUsername>\AppData\Local\JONSBO-AIO\Programme\
```

4. The final structure must look like this:

```
C:\Users\<YourUsername>\AppData\Local\JONSBO-AIO\Programme\
└── YourThemeName\
    ├── Setting.txt
    ├── YourThemeName.json
    ├── demo.png              ← thumbnail shown in the theme browser
    ├── back.*                ← background file in ROOT (back.png/gif/mp4/avi/wmv)
    ├── font\
    │   └── YourFont.ttf
    └── source\
        ├── back.*            ← copy of background file (same format as root)
        ├── d1.png
        └── d2.png
```

> **The background file must exist in both locations** — in the theme root folder and inside `source\`. The Jonsbo software checks both. The Theme Builder exports it to both automatically, preserving the original file extension.

5. Launch the Jonsbo AIO application
6. Navigate to **Screen Vertical** or **Screen Horizontal** depending on your theme orientation
7. Your new theme will appear in the list — select it and apply

> **demo.png** — This is the thumbnail shown in the theme browser. The Theme Builder captures it automatically during export. If you need to regenerate it, use the **📷 demo.png** button in the toolbar.

---

## Fonts

### Using the Font Picker

When a text element is selected, the Font section of the properties panel offers:

- **Dropdown** — all fonts found in the built-in Jonsbo themes, listed first. Populated with system fonts after you click Load System Fonts.
- **🔍 Load System Fonts** — queries all fonts installed on your Windows machine (Chrome/Edge only). A one-time permission popup appears — click Allow. Fonts are added to the dropdown immediately.
- **Custom text field** — type any font name directly. The dropdown updates to select it if it matches, or adds it as a custom entry.
- **📁 Add Font File** — pick a `.ttf` or `.otf` file from disk. The font is registered with the browser for live preview, and stored so it is automatically included in the `font\` folder when you export the ZIP. This is the most reliable way to ensure fonts are exported correctly.

### Font Export

When exporting a ZIP the builder tries two methods for including fonts:

1. **Explicitly uploaded files** (via Add Font File) — included directly, works in any browser
2. **System font API** (Chrome/Edge only) — attempts to extract the font file from the OS for any fonts not manually uploaded

A `FONTS_NOTE.txt` is always written to the `font\` folder listing which fonts are `[included]` and which are `[MISSING]`, so you always know exactly what needs to be added manually.

### Font Installation on the Device

Fonts must be installed in two places for a theme to display correctly:

- Inside the theme's `font\` folder (for portability and sharing)
- Installed on the Windows machine running the Jonsbo AIO software — right-click the `.ttf` file → **Install** or **Install for all users**

> The `FontFamily` name in `Setting.txt` must match the font's **internal registered name** in Windows, which is not always the same as the filename. The Theme Builder's font dropdown shows the correct internal names.

### Built-in Theme Fonts

| Font name | Style |
|-----------|-------|
| `HarmonyOS Sans SC` | Clean sans-serif in four weights: regular, Light, Medium, Black |
| `Square721 BT` | Monospace / tech display |
| `Tomorrow ExtraBold` | Geometric sans-serif, very bold |
| `Oswald Stencil` | Condensed stencil display |
| `Cinzel` | Classical serif |
| `站酷高端黑` | Chinese display, bold gothic |
| `站酷庆科黄油体` | Chinese display, rounded butter style |
| `猫啃忘形圆` | Chinese display, rounded |

---

## Snap & Align

Every element's property panel includes a **Snap & Align** section:

| Control | What it does |
|---------|-------------|
| ↔ H Center | Center element horizontally on the canvas |
| ↕ V Center | Center element vertically on the canvas |
| ⇤ L / ⇥ R / ⇡ T / ⇣ B | Snap element to a canvas edge |
| Align to dropdown | Pick any other element on the canvas |
| ⇤ L-edge / ⇥ R-edge | Align left or right edges to the target element |
| ⇡ T-edge / ⇣ B-edge | Align top or bottom edges to the target element |
| ↕ V-center | Align vertical midpoints with the target element |
| ↔ H-center | Align horizontal midpoints with the target element |
| Snap drag checkbox | When on, elements snap to the canvas center lines while dragging — a gold guide line flashes to confirm the snap |

---

## Theme Color Palette

Every color picker has a **+** button next to it that saves the current color to the palette. The palette appears as a row of colored swatches below each color picker on all element types (text, bars, rings, decorative blocks). Click any swatch to apply that color to the current element. The palette holds up to 16 colors and persists for the browser session.

---

## Preparing Image Assets

### Canvas Dimensions

| Orientation | Width | Height |
|-------------|-------|--------|
| Vertical | 462 px | 1920 px |
| Horizontal | 1920 px | 462 px |

All images must be exactly these dimensions.

### Image Files and Their Roles

| Filename | Role |
|----------|------|
| `back.png` / `back.gif` / `back.mp4` / `back.avi` / `back.wmv` | Background — loaded by the display software behind all layers. Must exist in both the theme root and `source\`. Animated GIFs and video files loop automatically. |
| `d1.png` | Layer 1 — usually the main background art or dark base. |
| `d2.png` | Layer 2 — foreground overlay (character art, UI chrome) on a transparent background. |
| `d3.png`… | Additional layers, named sequentially. |

### Workflow

1. Create a document at 462×1920px (vertical) or 1920×462px (horizontal) in Photoshop, GIMP, or similar
2. Design your background — save a flattened copy as `back.png`
3. For overlays (character art, panel borders), keep foreground elements on a **transparent background** and export as PNG-24 with transparency as `d1.png`, `d2.png`, etc.
4. Leave transparent areas where live data elements (text, bars, rings) will appear — the display software renders those on top
5. Load your background using the **🖼 Background** button — supports PNG, JPG, GIF (animated), MP4, AVI, and WMV. Image layers (d1.png etc.) use **+ Image Layer** and support PNG only.

### Z-Order

Image layers always sit behind data elements. The builder enforces this automatically on the canvas and at export time.

```
z = 1     d1.png   ← background art
z = 2     d2.png   ← foreground art overlay
z = 10+   Text, BorderLine, RingProgressBar
```

---

## Setting.txt Format Reference

### Header
```
name:ThemeName
width:462
height:1920
```

### Text element
```
Text:x@50,y@100,z@10,maxheight@80,maxwidth@500,FontSize@36,FontFamily@#Cinzel,Foreground@#FFFFFFFF,Title@,data@CpuUsage,unit@%
```

| Field | Description |
|-------|-------------|
| `x`, `y` | Top-left position in pixels |
| `z` | Layer order (higher = in front) |
| `maxheight`, `maxwidth` | Bounding box — text is clipped to this area |
| `FontSize` | Font size in pixels |
| `FontFamily` | Font internal name prefixed with `#` |
| `Foreground` | Text color in `#AARRGGBB` |
| `Title` | Static prefix shown before the live value |
| `data` | Sensor binding key |
| `unit` | Suffix appended after the value (e.g. `℃`, `%`) |
| `IsDefaultText@true` | Makes this a static label — `data` field is used as literal text |

### Progress bar (BorderLine)
```
BorderLine:x@40,y@500,z@8,maxheight@40,maxwidth@360,MaxNum@100,CornerRadius@0 0 0 0,Fill@#FF499CDE,IsAnimationEnabled@True,BorderThicknes@2,BorderFill@#FFFFFFFF,BackColor@#00000000,data@CpuUsage
```

| Field | Description |
|-------|-------------|
| `maxheight`, `maxwidth` | Bar dimensions |
| `MaxNum` | Value that equals 100% fill — typically `100` for percentages |
| `Fill` | Bar fill color |
| `BorderFill` | Border/outline color |
| `BackColor` | Background track color |
| `BorderThicknes` | Border width in pixels (note: single `s` — this is not a typo) |
| `CornerRadius` | Four radii: `TL TR BR BL` |
| `IsAnimationEnabled` | `True` or `False` |

A `BorderLine` **without** a `data@` field is a decorative block — static colored rectangle, no animation.

### Ring gauge (RingProgressBar)
```
RingProgressBar:x@105,y@142,z@1,RingWidth@12,Perimeter@250,MaxValue@100,IndexColor@#FFFF0000,BackColor@#FFFF9500,data@CPUTemp
```

| Field | Description |
|-------|-------------|
| `RingWidth` | Arc stroke thickness in pixels |
| `Perimeter` | **Diameter** of the ring circle in pixels (not circumference) |
| `MaxValue` | Value that represents a full ring |
| `IndexColor` | Arc fill color |
| `BackColor` | Track/background ring color |

To display a value inside the ring, add a `Text` element with the same `data@` key positioned at the same x/y with `maxwidth` and `maxheight` equal to the ring diameter. The builder handles this automatically via the "Show label" checkbox on ring gauges.

### Image layer
```
d1.png:x@0,y@0,z@1,height@1920,width@462
```

The filename is the element identifier. Files go in the `source\` folder.

### Color format

All colors use **`#AARRGGBB`** — alpha is the first byte, unlike standard CSS hex:

| Prefix | Meaning |
|--------|---------|
| `#FF` | Fully opaque |
| `#80` | ~50% transparent |
| `#00` | Fully transparent |

Example: `#FFFF0000` = solid red. `#80000000` = 50% transparent black.

---

## Sensor Bindings

### CPU
| Binding | Description |
|---------|-------------|
| `CpuUsage` | Utilization % |
| `CPUTemp` | Temperature °C |
| `CpuFrequency` | Clock speed MHz |
| `CpuTEC` | Power draw W |
| `CpuVoltage` | Voltage V |
| `CPUName` | Model name string |

### GPU
| Binding | Description |
|---------|-------------|
| `GpuUsage` | Utilization % |
| `GPUTemp` | Temperature °C |
| `GpuFrequency` | Clock speed MHz |
| `GpuTEC` | Power draw W |
| `GpuVoltage` | Voltage V |
| `GPUName` | Model name string |

### RAM
| Binding | Description |
|---------|-------------|
| `MemoryUseInt` | Usage % |
| `MemoryUsageSize` | Used GB |
| `MemoryCanUsageSize` | Available GB |
| `MemorySize` | Total GB |

### Storage
| Binding | Description |
|---------|-------------|
| `HardDiskUsed` | Usage % |
| `HardDiskUsedSize` | Used GB |
| `HardDiskFree` | Free GB |
| `HardDiskCapacity` | Total GB |
| `DiskTemp` | Temperature °C |

### Network
| Binding | Description |
|---------|-------------|
| `DownNetSpeed` | Download speed |
| `UpNetSpeed` | Upload speed |

### Time & Date
| Binding | Output format | Notes |
|---------|--------------|-------|
| `CurrentTimeShut` | `HH:MM` | Recommended — no seconds, 24h |
| `CurrentTime` | `HH:MM:SS` | Includes seconds, 24h |
| `CurrentDates` | `YYYY.MM.DD` | System locale date |
| `WeekDaysL3` | `Mon`, `Tue`… | Three-letter day abbreviation |

> Both time bindings output 24-hour format. There is no known 12-hour binding in the current firmware.

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| Arrow keys | Nudge selected element 1px |
| Shift + Arrow | Nudge selected element 10px |
| Delete | Remove selected element |
| Escape | Deselect |

---

## Version History

### v7
- Background supports PNG, JPG, GIF (animated), MP4, AVI, and WMV — file picker accepts all formats
- Video backgrounds loop and play automatically in the preview canvas
- Export preserves the original file extension (`back.mp4`, `back.gif`, etc.) in both root and `source\`
- JSON export sets `BackgroundVideoFile` for video and `BackgroundImage` for still images
- Import (ZIP and folder) detects and restores video backgrounds automatically

### v6
- **Import Folder** — point at a single theme folder or the entire `Programme\` directory; visual picker with thumbnails when multiple themes are found
- **Import Theme** — load from ZIP file or Setting.txt; ring+label pairs automatically re-linked on import
- All import methods reconstruct the full canvas including background, image layers, and all element types

### v5
- Scale-with-resize checkbox for text (defaults on, moved to top of Font section)
- Ring gauge built-in label with font, color, and manual size controls
- Image layers always placed below data assets on canvas and in exported Setting.txt
- Font file upload (Add Font File) for reliable font export
- Image preparation documentation

### v4
- Text bounding box auto-sizes to content on every change
- Align-to-element: align edges and centers to any other element
- Theme color palette on all color pickers (text, bars, rings, decorative)
- Export name prompt
- Snap & Align section with center snap and edge snap buttons
- Snap-while-dragging guide lines

### v3
- Font picker with system font loader and custom entry
- Fixed color pickers not saving
- Fixed ring gauge background track

### v2
- RingProgressBar element with live SVG preview
- Decorative BorderLine
- ZIP export with embedded images and fonts
- New sensors: DiskTemp, HardDiskFree, DownNetSpeed, UpNetSpeed, CPUName, GPUName

### v1
- Initial release — drag-and-drop canvas editor, core sensor bindings, Setting.txt/JSON export

---

## Contributing

Pull requests welcome. If you discover additional sensor bindings, element types, or JSON fields from other 916 themes, please open an issue or PR.

---

## Disclaimer

This is an independent community project, not affiliated with or endorsed by Jonsbo. The theme file format was reverse-engineered from the built-in themes included with the 916 display software.
