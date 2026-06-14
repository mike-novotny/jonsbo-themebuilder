# Jonsbo 916 Theme Builder

A browser-based visual editor for creating custom themes for the **Jonsbo 916** series LCD display panels. Design your theme with a live preview, position elements by dragging, and export a ready-to-use ZIP with the correct folder structure.

![Theme Builder Screenshot](screenshot.png)

---

## Features

- **Visual canvas editor** — 462×1920 (vertical) or 1920×462 (horizontal) canvas scaled to fit your screen
- **Drag to place, drag to move, drag corner to resize** every element
- **Live preview** — sensor values shown with realistic sample data so you can see how it will look
- **All element types supported:**
  - Text (live sensor data with optional prefix and unit)
  - Progress bars (horizontal or vertical, fully styled)
  - Ring gauges (circular arc progress, RingProgressBar)
  - Static labels (fixed text)
  - Decorative blocks (solid color panels, dividers)
  - Image layers (d1.png, d2.png, etc.)
- **Full sensor library** — every data binding found across all built-in themes
- **Font picker** — built-in theme fonts dropdown, system font loader (Chrome/Edge), and custom font entry
- **ZIP export** — downloads a `.zip` with the exact folder structure the display expects, including your image assets
- **Layer panel** — see z-order, toggle visibility, delete elements
- **Keyboard shortcuts** — arrow keys nudge (Shift+arrow = 10px), Delete removes selected element

---

## Installing a Theme on the Device

> **Important:** The Jonsbo AIO application must be **completely closed** before copying theme files. Check the system tray (bottom-right corner of the taskbar, click the `^` arrow to show hidden icons) and right-click the Jonsbo icon → **Exit** to fully close it. Simply closing the main window may leave it running in the tray.

1. Export your theme as a ZIP from the Theme Builder
2. Extract the ZIP — you will get a folder named after your theme
3. Copy the entire theme folder into the Jonsbo programme directory:

```
C:\Users\<YourUsername>\AppData\Local\JONSBO-AIO\Programme\
```

> **Finding AppData:** AppData is a hidden folder. The quickest way to reach it is to press `Win + R`, type `%LocalAppData%\JONSBO-AIO\Programme` and press Enter. This opens the folder directly.

4. The final structure should look like this:

```
C:\Users\<YourUsername>\AppData\Local\JONSBO-AIO\Programme\
└── YourThemeName\
    ├── Setting.txt
    ├── YourThemeName.json
    ├── demo.png
    ├── font\
    │   └── (your .ttf font files)
    └── source\
        ├── back.png
        ├── d1.png
        └── d2.png
```

5. Launch the Jonsbo AIO application
6. Navigate to **Screen Vertical** or **Screen Horizontal** depending on your theme orientation — your new theme will appear in the list there
7. Select your theme and apply it

> **demo.png** — This is the thumbnail shown in the theme browser. Take a screenshot of the canvas preview in the Theme Builder, crop it to the theme dimensions, and save it as `demo.png` inside the theme folder. Without it the theme will still work but will show a blank thumbnail.

### Font Installation

If your theme uses custom fonts, the font files must be installed in **two places**:

- Inside the theme's `font\` folder (so the theme package is self-contained for sharing)
- Installed on the Windows system running the Jonsbo software — right-click the `.ttf` file in Windows Explorer and select **Install** or **Install for all users**

The `FontFamily` name in `Setting.txt` must match the font's internal name as Windows recognises it, which is not always the same as the filename.

---

## Getting Started with the Builder

1. Download `ThemeBuilder.html`
2. Open it in any modern browser — Chrome or Edge recommended for full font support
3. Choose **Vertical** or **Horizontal** orientation
4. Enter a theme name (no spaces)
5. Click **🖼 Background** to load your `back.png`
6. Click elements in the left sidebar to add them to the canvas
7. Click an element to select it, drag to move, drag the gold corner handle to resize
8. Edit properties in the right panel — including font family via the font picker
9. Click **💾 Export ZIP** when done

---

## Export Structure

When you click Export ZIP, the download contains:

```
ThemeName/
├── Setting.txt          ← main layout file (auto-generated)
├── ThemeName.json       ← theme config (auto-generated)
├── demo_NOTE.txt        ← reminder to add a screenshot as demo.png
├── font/
│   └── (place your .ttf font files here)
└── source/
    ├── back.png         ← your background image (auto-included if loaded)
    ├── d1.png           ← image layers (auto-included if loaded)
    └── d2.png
```

---

## Fonts

### Using the Font Picker

The font property panel has three ways to choose a font:

- **Dropdown** — all fonts found in the built-in Jonsbo themes, pre-loaded and ready to use. Also populated with your system fonts after loading them.
- **Load System Fonts** button — queries all fonts currently installed on your Windows machine and adds them to the dropdown. Requires Chrome or Edge (not Firefox), and a one-time permission popup will appear asking for font access — click Allow.
- **Custom entry** — type any font name directly into the text field below the dropdown for fonts not in either list. The exact internal font name must be used.

### Loading and Installing New Fonts

The recommended workflow for using a custom font in your theme:

1. Download the font `.ttf` or `.otf` file
2. Right-click the font file in Windows Explorer and select **Install** (or **Install for all users**)
3. Open the Theme Builder in Chrome or Edge
4. Select a text element, then click **🔍 Load System Fonts** in the font section
5. Accept the font access permission when prompted
6. Your newly installed font will appear in the dropdown — select it
7. When you export your theme ZIP, copy the font file into the `font\` subfolder before installing the theme

> **Why install on Windows first?** The Theme Builder uses your browser's access to locally installed fonts. The font must be registered with Windows for it to appear in the system font list. Simply having the `.ttf` file sitting in a folder is not enough.

> **Font name vs filename:** The name you must use in `FontFamily@#` in `Setting.txt` is the font's **internal name** as Windows knows it — this is shown in the dropdown after loading system fonts. It is often different from the filename. For example, `TomorrowEB.ttf` might have the internal name `Tomorrow ExtraBold`.

### Font Files in Your Theme

Font files must be placed in the `font\` subfolder of your theme folder. The font must also be installed on the Windows system running the Jonsbo AIO software — the `font\` folder ensures the package is self-contained for sharing, but the font still needs to be installed on the target machine separately.

### Built-in Theme Fonts

Fonts found across the official Jonsbo 916 built-in themes:

| Font name | Style |
|-----------|-------|
| `HarmonyOS Sans SC` | Huawei system sans-serif — also available as `HarmonyOS Sans SC Light`, `HarmonyOS Sans SC Medium`, `HarmonyOS Sans SC Black` |
| `Square721 BT` | Monospace / tech display |
| `Tomorrow ExtraBold` | Geometric sans-serif, very bold |
| `Oswald Stencil` | Condensed stencil |
| `Cinzel` | Classical serif |
| `站酷高端黑` | Chinese display, bold gothic |
| `站酷庆科黄油体` | Chinese display, rounded butter style |
| `猫啃忘形圆` | Chinese display, rounded |

Any `.ttf` or `.otf` font installed on the display device can be referenced by name.

---

## Preparing Image Assets

Themes are built from layered PNG images combined with live data elements. Understanding the image dimensions and naming conventions is essential before you start designing.

### Canvas Dimensions

| Orientation | Width | Height |
|-------------|-------|--------|
| Vertical | 462 px | 1920 px |
| Horizontal | 1920 px | 462 px |

All images must be created at exactly these dimensions so they align perfectly with the canvas coordinates in `Setting.txt`.

### Image Files and Their Roles

| Filename | Role | Notes |
|----------|------|-------|
| `back.png` | Background image | Loaded separately by the display software behind all layers. Often a photo or illustration. |
| `d1.png` | Layer 1 (bottom) | First composited image layer. Usually the main background art or a dark base panel. |
| `d2.png` | Layer 2 | Overlay art — UI chrome, panel frames, decorative borders, character art with transparent background. |
| `d3.png`, `d4.png`… | Additional layers | Use as many as needed, named sequentially. |

All image layers go in the `source\` folder inside your theme directory.

### Creating the Images

**Recommended workflow in Photoshop, GIMP, or similar:**

1. Create a new document at **462 × 1920 px** (vertical) or **1920 × 462 px** (horizontal), 72 dpi, RGB
2. Design your background — photo, illustration, or solid/gradient. Save a flattened copy as `back.png`
3. For layered artwork (e.g. a robot character over a background), separate the layers:
   - `d1.png` — background scene, full canvas, no transparency needed
   - `d2.png` — foreground elements (character, UI frames, overlays) on a **transparent background** — export as PNG-24 with transparency
4. Leave blank/transparent areas in `d2.png` wherever live data text and bars will appear — those are rendered on top by the display software

**Background removal:** If your source image has a solid background you want to remove for `d2.png`, use Photoshop's "Remove Background" button, GIMP's fuzzy select + delete, or an online tool like remove.bg.

**Saving:** Always export as PNG (not JPG) to preserve transparency and avoid compression artefacts on text-adjacent areas.

### Z-Order and Layering

Image layers must have lower z-values than all data elements (text, bars, ring gauges) so they don't cover them. The Theme Builder enforces this automatically — when you add an image layer, it is placed beneath all existing data elements. At export, the `Setting.txt` z-values are also verified to ensure images never sit above data assets.

A typical z-order stack:
```
z = 1     d1.png   ← bottom background
z = 2     d2.png   ← foreground art overlay
z = 10+   Text, BorderLine, RingProgressBar elements
```

---

## Setting.txt Format Reference

The `Setting.txt` file is a plain-text layer stack. Each line declares one element with `@`-separated key-value pairs.

### Header
```
name:ThemeName
width:462
height:1920
```

### Text element
```
Text:x@50,y@100,z@10,maxheight@80,maxwidth@500,FontSize@36,FontFamily@#Arial,Foreground@#FFFFFFFF,Title@,data@CpuUsage,unit@%
```

| Field | Description |
|-------|-------------|
| `x`, `y` | Position in pixels from top-left |
| `z` | Layer order (higher = in front) |
| `maxheight`, `maxwidth` | Bounding box |
| `FontSize` | Font size in pixels |
| `FontFamily` | Font name prefixed with `#` |
| `Foreground` | Text color in `#AARRGGBB` format |
| `Title` | Static prefix text shown before the value |
| `data` | Sensor binding key (see table below) |
| `unit` | Suffix appended after the value (e.g. `℃`, `%`) |
| `IsDefaultText@true` | Marks this as a static label (data field used as literal text) |

### Progress bar (BorderLine)
```
BorderLine:x@40,y@500,z@8,maxheight@40,maxwidth@360,MaxNum@100,CornerRadius@0 0 0 0,Fill@#FF499CDE,IsAnimationEnabled@False,BorderThicknes@2,BorderFill@#FFFFFFFF,BackColor@#00000000,data@CpuUsage
```

| Field | Description |
|-------|-------------|
| `maxheight`, `maxwidth` | Bar dimensions |
| `MaxNum` | Value that represents 100% fill (use `100` for %, `200` for °C) |
| `Fill` | Bar fill color (`#AARRGGBB`) |
| `BorderFill` | Border/outline color |
| `BackColor` | Background track color |
| `BorderThicknes` | Border width in pixels (note: single `s`) |
| `CornerRadius` | Four corner radii: `TL TR BR BL` |
| `IsAnimationEnabled` | `True` or `False` |
| `data` | Sensor binding key |

A `BorderLine` **without** a `data@` field is a decorative block — no animation, just a solid colored rectangle.

### Ring gauge (RingProgressBar)
```
RingProgressBar:x@150,y@50,z@12,RingWidth@12,Perimeter@127,MaxValue@200,IndexColor@#FF00B3EA,BackColor@#00000000,data@CPUTemp
```

| Field | Description |
|-------|-------------|
| `RingWidth` | Stroke thickness of the arc |
| `Perimeter` | Circumference of the ring in pixels (`Perimeter ÷ 2π = radius`) |
| `MaxValue` | Value that represents a full ring |
| `IndexColor` | Arc fill color (`#AARRGGBB`) |
| `BackColor` | Track/background ring color |
| `data` | Sensor binding key |

### Image layer
```
d1.png:x@0,y@0,z@1,height@1920,width@462
```

The filename is the element type. Files go in the `source\` folder. Multiple layers use `d1.png`, `d2.png`, `d3.png`, etc. A `back.png` is also supported as a separate background outside the layer stack.

### Color format

All colors use `#AARRGGBB` — **alpha comes first**, unlike standard web hex colors:

| Value | Meaning |
|-------|---------|
| `#FF` | Fully opaque |
| `#80` | ~50% transparent |
| `#00` | Fully transparent |

Example: `#FF66BB17` = fully opaque green. `#80000000` = 50% transparent black.

---

## Sensor Bindings

All `data@` values discovered across the built-in 916 themes:

### CPU
| Binding | Description |
|---------|-------------|
| `CpuUsage` | CPU utilization % |
| `CPUTemp` | CPU temperature °C |
| `CpuFrequency` | CPU clock speed MHz |
| `CpuTEC` | CPU power draw W |
| `CpuVoltage` | CPU voltage V |
| `CPUName` | CPU model name string |

### GPU
| Binding | Description |
|---------|-------------|
| `GpuUsage` | GPU utilization % |
| `GPUTemp` | GPU temperature °C |
| `GpuFrequency` | GPU clock speed MHz |
| `GpuTEC` | GPU power draw W |
| `GpuVoltage` | GPU voltage V |
| `GPUName` | GPU model name string |

### RAM
| Binding | Description |
|---------|-------------|
| `MemoryUseInt` | RAM usage % |
| `MemoryUsageSize` | RAM used GB |
| `MemoryCanUsageSize` | RAM available GB |
| `MemorySize` | RAM total GB |

### Storage
| Binding | Description |
|---------|-------------|
| `HardDiskUsed` | SSD/HDD usage % |
| `HardDiskUsedSize` | Used storage GB |
| `HardDiskFree` | Free storage GB |
| `HardDiskCapacity` | Total storage GB |
| `DiskTemp` | Drive temperature °C |

### Network
| Binding | Description |
|---------|-------------|
| `DownNetSpeed` | Download speed |
| `UpNetSpeed` | Upload speed |

### Time & Date
| Binding | Description |
|---------|-------------|
| `CurrentTime` | Current time |
| `CurrentTimeShut` | Current time (also triggers shutdown threshold) |
| `CurrentDates` | Current date |
| `WeekDaysL3` | Day of week abbreviation (Mon, Tue…) |

---

## Tips

**Layering artwork** — The display system composites layers in z-order. A typical stack looks like:

```
z=1   d1.png  ← background art (full canvas)
z=2   d2.png  ← foreground overlay / UI chrome (transparent PNG)
z=3+  Text and BorderLine elements (live data)
```

Design your `d1.png` background and `d2.png` overlay in an image editor (Photoshop, GIMP, etc.) at exactly 462×1920px, then position live data elements on top in the Theme Builder.

**Gauge trick** — The `RingProgressBar` only updates the arc fill. To make it look like a speedometer, place a large font `Text` element centered inside the ring to show the numeric value.

**Vertical bars** — Make a `BorderLine` taller than it is wide (e.g. `maxheight@168, maxwidth@23`) for a vertical fill bar. The fill direction may differ by firmware version.

**Alpha on borders** — Setting `BorderFill@#FFFFFF44` gives a subtle semi-transparent white border, common in the built-in themes.

---

## Version History

### v5
- "Scale w/resize" checkbox now defaults to on and moved to top of Font section
- Ring gauge label renders value and unit on a single line (e.g. 46°C) using SVG tspan, sized to fit inside the ring
- Ring label "Manual size" checkbox: label size input is disabled until checked; unchecking returns to auto-size
- Image layers are automatically placed below all data assets in both the canvas and exported Setting.txt
- Font files are auto-exported into the theme ZIP `font/` folder when system fonts are loaded in Chrome/Edge
- Image preparation documentation added to README with canvas dimensions, file naming, and layering guide

### v4
- Text elements have a "Scale w/resize" checkbox — when enabled, font size scales proportionally as you drag to resize
- Ring gauges have a built-in label option: "Show label" checkbox displays the sensor value and unit centered inside the ring, auto-sized to the ring diameter
- Ring label has its own color, font, and manual size override controls
- Ring label and gauge resize together as one unit — label size scales with the ring
- Ring gauge resize is constrained to square (equal width and height) and syncs the Diameter field live

### v3
- Font picker with built-in theme fonts dropdown, system font loader (Chrome/Edge), and custom font entry
- Fixed color picker not saving or updating preview
- Fixed ring gauge background track not rendering
- Fixed element position drift on repeated re-renders

### v2
- RingProgressBar element with live SVG arc preview
- Decorative BorderLine (solid block, no data binding)
- ZIP export with correct folder structure and embedded image assets
- New sensors: `DiskTemp`, `HardDiskFree`, `HardDiskUsedSize`, `DownNetSpeed`, `UpNetSpeed`, `CPUName`, `GPUName`
- Bold text toggle
- `IsAnimationEnabled` flag on progress bars
- Improved layer panel and property editor

### v1
- Initial release
- Drag-and-drop canvas editor
- Text, progress bar, and image layer elements
- Vertical/horizontal orientation
- Core sensor bindings
- Setting.txt and JSON export

---

## Contributing

Pull requests welcome. If you discover additional sensor bindings, element types, or JSON fields from other 916 themes, please open an issue or PR.

---

## Disclaimer

This tool is an independent community project and is not affiliated with or endorsed by Jonsbo. Theme file formats were reverse-engineered from the built-in theme files included with the 916 display software.
