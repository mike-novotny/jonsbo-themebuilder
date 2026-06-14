# Jonsbo 916 Theme Builder

A browser-based visual editor for creating custom themes for the **Jonsbo 916** series LCD display panels. Design your theme with a live preview, position elements by dragging, and export a ready-to-use ZIP with the correct folder structure.

![Theme Builder Screenshot](screenshot.png)

---

## Features

- **Visual canvas editor** — 462×1920 (vertical) or 1920×462 (horizontal) canvas scaled to fit your screen
- **Drag to place, drag to move, drag corner to resize** every element
- **Live preview** — sensor values shown with realistic sample data so you can see how it'll look
- **All element types supported:**
  - Text (live sensor data with optional prefix and unit)
  - Progress bars (horizontal or vertical, fully styled)
  - Ring gauges (circular arc progress, RingProgressBar)
  - Static labels (fixed text)
  - Decorative blocks (solid color panels, dividers)
  - Image layers (d1.png, d2.png, etc.)
- **Full sensor library** — every data binding found across all built-in themes
- **ZIP export** — downloads a `.zip` with the exact folder structure the display expects, including your image assets
- **Layer panel** — see z-order, toggle visibility, delete elements
- **Keyboard shortcuts** — arrow keys nudge (Shift+arrow = 10px), Delete removes selected element

---

## Getting Started

1. Download `ThemeBuilder.html`
2. Open it in any modern browser (Chrome, Edge, Firefox) — no install required
3. Choose **Vertical** or **Horizontal** orientation
4. Enter a theme name (no spaces)
5. Click **🖼 Background** to load your `back.png`
6. Click elements in the left sidebar to add them to the canvas
7. Click an element to select it, drag to move, drag the gold corner handle to resize
8. Edit properties in the right panel
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

Extract the ZIP and drop the folder into the Jonsbo theme directory on your display device.

> **demo.png** — The app can't auto-capture a screenshot, so take one of the canvas area yourself and save it as `demo.png` inside the theme folder. This is what the device shows in its theme browser.

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

A `BorderLine` **without** a `data@` field is a decorative block (no animation, just a solid colored rectangle).

### Ring gauge (RingProgressBar)
```
RingProgressBar:x@150,y@50,z@12,RingWidth@12,Perimeter@127,MaxValue@200,IndexColor@#FF00B3EA,BackColor@#00000000,data@CPUTemp
```

| Field | Description |
|-------|-------------|
| `RingWidth` | Stroke thickness of the arc |
| `Perimeter` | Circumference of the ring in pixels (controls size: `Perimeter / (2π) = radius`) |
| `MaxValue` | Value that represents a full ring |
| `IndexColor` | Arc color (`#AARRGGBB`) |
| `BackColor` | Track/background ring color |
| `data` | Sensor binding key |

### Image layer
```
d1.png:x@0,y@0,z@1,height@1920,width@462
```

The filename is the element type. Files go in the `source/` folder. Multiple layers use `d1.png`, `d2.png`, `d3.png`, etc. A `back.png` is also supported as a separate background outside the layer stack.

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

## Fonts

Font files must be placed in the `font/` subfolder of your theme. The font name used in `FontFamily@#` must match the font's internal name (not necessarily the filename).

Fonts seen in the built-in themes:

| Font name | Style |
|-----------|-------|
| `站酷高端黑` | Chinese display, bold gothic |
| `站酷庆科黄油体` | Chinese display, rounded |
| `猫啃忘形圆` | Chinese display, rounded |
| `HarmonyOS Sans SC` | Huawei system sans-serif (multiple weights: Light, Medium, Black) |
| `Square721 BT` | Monospace / tech |
| `Tomorrow ExtraBold` | Geometric sans, very bold |
| `Oswald Stencil` | Condensed stencil |
| `Cinzel` | Classical serif |

Any `.ttf` or `.otf` font installed on the display device can be referenced by name.

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
