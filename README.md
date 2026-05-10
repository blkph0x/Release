# Solace Brochure Studio

**Solace Brochure Studio** is a Qt/C++ desktop design tool for building polished brochure pages, visual layouts, print-ready documents, SVG/PNG/PDF exports, and editable vector motifs from reference artwork. It combines a canvas editor, page management, rich text styling, image placement, procedural textures, vector tracing, preflight checks, project packaging, and CLI automation into one focused creative workflow.

> This README is written as both a GitHub landing page and a practical user manual. It explains what the app does, how to build it, how to use the major tools, what keyboard shortcuts exist, how the lasso/vector workflow works, how to export, and how to automate projects from JSON.

---

## Table of contents

1. [Overview](#overview)
2. [Core features](#core-features)
3. [Screens and workspace layout](#screens-and-workspace-layout)
4. [Building from source](#building-from-source)
5. [Running the app](#running-the-app)
6. [Command-line flags](#command-line-flags)
7. [Quick start workflow](#quick-start-workflow)
8. [Pages and project structure](#pages-and-project-structure)
9. [Text tools](#text-tools)
10. [Image and reference-image tools](#image-and-reference-image-tools)
11. [Lasso-to-vector and motif tracing](#lasso-to-vector-and-motif-tracing)
12. [Vector editing](#vector-editing)
13. [Layout, alignment, and smart design tools](#layout-alignment-and-smart-design-tools)
14. [Textures, styling, and visual effects](#textures-styling-and-visual-effects)
15. [Layers panel](#layers-panel)
16. [Preflight and print checks](#preflight-and-print-checks)
17. [Saving, loading, packaging, and recovery](#saving-loading-packaging-and-recovery)
18. [Exporting](#exporting)
19. [Keyboard and mouse controls](#keyboard-and-mouse-controls)
20. [CLI automation](#cli-automation)
21. [Project file notes](#project-file-notes)
22. [Developer architecture notes](#developer-architecture-notes)
23. [Troubleshooting](#troubleshooting)
24. [Recommended future roadmap](#recommended-future-roadmap)

---

## Overview

Solace Brochure Studio is designed for creating brochure-style layouts with a strong focus on:

- multi-page brochure design,
- rich styled typography,
- reference image tracing,
- editable vector motif creation,
- print and web export,
- deterministic project saves,
- autosave and recovery,
- CLI automation for repeatable design generation.

The app is built with Qt and C++ and uses a `QGraphicsView`-style canvas workflow. Pages contain user-editable items such as text boxes, images, divider lines, and vector motifs. Items can be selected, moved, styled, arranged, aligned, exported, and serialized into project files.

The current codebase includes multiple hardening improvements:

- atomic save paths,
- project backup generation,
- autosave/recovery support,
- schema validation,
- stable item UUIDs,
- relative asset paths,
- project package saving/loading,
- missing image placeholders,
- safer graphics-item geometry updates,
- improved lasso/vector optimization,
- command-based undo scaffolding,
- layer-model groundwork.

---

## Core features

### Document and page design

- Create cover pages and inside pages.
- Duplicate and delete pages.
- Apply cover and inside templates.
- Configure page size, margins, columns, gutters, bleed, and background color.
- Link page styling through the project DNA system.
- Sync shared design properties between linked pages.

### Text editing

- Add editable text boxes.
- Change font family, size, weight, italic, color, alignment, opacity, tracking, and line spacing.
- Apply named text styles and style presets.
- Add shadows, glows, strokes, and texture fills to text.
- Resize text boxes visually.
- Fit large titles to content.
- Prevent unwanted large-title wrapping through text layout modes.
- Detect text overflow during preflight.

### Image placement

- Add image items to the canvas.
- Move, scale, resize, and adjust opacity.
- Load reference images for tracing.
- Clear or bake reference images into the page.
- Use centralized image loading with EXIF auto-transform support.
- Preserve moved projects with relative asset paths.
- Show placeholders instead of silently dropping missing images.

### Vector and motif tools

- Draw normal lines.
- Draw vector lines.
- Trace filled motifs from a reference image with a lasso.
- Trace centerline artwork from reference linework.
- Smooth selected motifs.
- Edit vector nodes and handles.
- Insert, move, reset, and delete vector nodes.
- Recolor vector fill and stroke.
- Export selected vector motifs as SVG.
- Use advanced lasso optimization to reduce noisy point clouds into cleaner editable contours.

### Layout and arrangement

- Align selected items left, center, right, top, middle, and bottom.
- Distribute selected items horizontally or vertically.
- Auto-center selections.
- Auto-space selections.
- Fit selections to page margins.
- Raise or lower selected items in stacking order.
- Nudge selected items with arrow keys.

### Export

- Export the current page as PDF.
- Export the full brochure/project as PDF.
- Export high-resolution brochure PDF.
- Export current page as PNG.
- Export web PNG.
- Export current page as SVG.
- Export project SVG package.
- Export selected vector motif as SVG.
- Run preflight before print/export.

### Automation

- Run JSON automation files from the command line.
- Create pages, add text, add images, trace references, apply styles, export files, run preflight, and more from scripts.
- Run built-in CLI smoke tests.
- Print supported CLI actions.

---

## Screens and workspace layout

The application uses a traditional design-app layout:

### Main canvas

The central area shows the currently selected brochure page. You can select, move, resize, trace, and edit items directly on this canvas.

### Toolbar

The toolbar contains high-frequency actions such as:

- Add Cover
- Add Page
- Duplicate Page
- Delete Page
- Undo / Redo
- DNA Link
- Sync DNA
- Outro Layout
- Phi Mutate
- Saliency
- Clear Heatmap
- Ghost UI
- Add Text
- Line Tool
- Vector Line
- Add Image
- Reference Image
- Clear Reference
- Bake Reference
- Trace Motif
- Trace Centerline
- Smooth Motif
- Edit Nodes
- Recolor Motif
- Export Motif SVG
- Cover Template
- Inside Template
- Align / distribute tools
- Save / Load
- Zoom controls
- Preflight
- Preview
- PDF / PNG / SVG export tools

### Properties panel

The properties panel changes depending on the selected item. It is used for editing text, image, vector, line, and page-level settings.

### Layers panel

The layers panel provides the foundation for arranging selected items and future layer visibility/locking behavior. Current layer groundwork includes stable layer IDs and default layer serialization.

### Menus

The app includes these top-level menus:

- File
- Edit
- View
- Insert
- Arrange
- Design
- Tools

---

## Building from source

### Requirements

- CMake 3.21 or newer
- C++20 compiler
- Qt 6 with these components:
  - Widgets
  - PrintSupport
  - Svg
  - OpenGLWidgets if your local CMake file still links it, though the current safe viewport path does not require forced OpenGL viewport usage
- Windows: Visual Studio 2022 recommended
- Optional: vcpkg for dependency management

### Windows / Visual Studio build

From the project root:

```bat
rmdir /s /q out
cmake -S . -B out/build/vs2022-x64
cmake --build out/build/vs2022-x64 --config Release
```

The executable will normally be generated under:

```text
out/build/vs2022-x64/Release/SolaceBrochureStudio.exe
```

### Generic CMake build

```bash
cmake -S . -B build
cmake --build build --config Release
```

### Important build note

When adding new `.cpp` files, ensure they are listed in `CMakeLists.txt` under `add_executable(...)`. If headers compile but the linker reports unresolved externals, the matching implementation file is probably missing from the target source list.

---

## Running the app

Normal startup:

```bash
SolaceBrochureStudio
```

Windows:

```bat
SolaceBrochureStudio.exe
```

By default, the app opens directly into the editor.

---

## Command-line flags

### Show help

```bash
SolaceBrochureStudio --help
```

### Run the editor normally

```bash
SolaceBrochureStudio
```

### Show the startup manifesto

The manifesto boot sequence is disabled by default. To show it explicitly:

```bash
SolaceBrochureStudio --menifesto
```

The correctly spelled alias is also supported:

```bash
SolaceBrochureStudio --manifesto
```

### List CLI automation actions

```bash
SolaceBrochureStudio --list-cli-actions
```

### Run automation JSON and exit

```bash
SolaceBrochureStudio --automation path/to/automation.json
```

### Run built-in self-test

```bash
SolaceBrochureStudio --self-test output/directory
```

### Important flag rule

Do not use `--automation` and `--self-test` together. They are mutually exclusive.

---

## Quick start workflow

### 1. Create pages

Use the toolbar or Insert menu:

- Add Cover
- Add Page
- Duplicate Page

Start with a cover page, then add one or more inside pages.

### 2. Add text

Click **Add Text**. A new text box appears on the current page. Select it and use the Properties panel to change font, size, color, alignment, tracking, line spacing, opacity, and effects.

For large titles, use fit-to-content or single-line behavior to avoid unwanted wrapping.

### 3. Add images

Click **Add Image** and choose a raster image. Move and resize it on the canvas. The app uses centralized image loading and supports EXIF orientation correction.

### 4. Add a reference image for tracing

Click **Reference Image** and choose an image. This image can be used as a tracing source for vector motifs.

### 5. Trace a motif

Click **Trace Motif**, draw a lasso around the image area to convert, then finish the lasso. The vectorizer creates an optimized editable vector motif.

### 6. Edit vector nodes

Select the vector motif and click **Edit Nodes**. You can move nodes and handles, insert nodes, delete nodes, or reset handles.

### 7. Run preflight

Before export, click **Preflight**. This checks for issues such as missing assets or text overflow.

### 8. Export

Use File or toolbar export commands:

- Page PDF
- Print PDF
- Book PDF
- Book 600
- Web PNG
- PNG
- SVG
- Book SVG

---

## Pages and project structure

A project is made of pages. Each page has:

- page settings,
- background color or background texture,
- margin/column/gutter/bleed settings,
- optional reference image,
- user items such as text, images, lines, and vector motifs.

### Page templates

The app includes cover and inside template actions:

- **Cover Template** applies a cover-page layout.
- **Inside Template** applies an inside-page layout.

Templates can speed up brochure creation by providing a starting design structure.

### Page DNA

The DNA system lets pages share design behavior. Use:

- **DNA Link** to enable or disable link behavior on the current page.
- **Sync DNA** to synchronize linked page design traits.

### Page settings

Common page settings include:

- size,
- background color,
- background texture,
- margins,
- columns,
- gutter,
- bleed.

---

## Text tools

Text boxes are one of the main design objects in the app.

### Adding text

Use:

```text
Toolbar → Add Text
```

or:

```text
Insert → Add Text Box
```

### Editing text content

Select a text box and edit its content directly. Depending on focus state, double-clicking or interacting with the text item places it in text-edit mode.

### Text properties

The Properties panel supports:

- font family,
- font size,
- bold,
- italic,
- alignment,
- tracking,
- line spacing,
- text color,
- opacity,
- style selection,
- shadow,
- glow,
- stroke,
- texture fill.

### Text layout modes

Text layout modes control how text fits its box:

- **Auto Wrap**: normal paragraph behavior.
- **Single Line**: keeps display/title text on one line where possible.
- **Fit Width**: expands or adjusts width based on content.
- **Fit Box**: intended for fitting text within a bounded area.
- **Auto Scale**: intended for future smart scaling behavior.

### Large title behavior

Large title text can wrap unexpectedly if the text box is too narrow. The hardened text system includes fit-to-content support and large-title single-line logic to reduce accidental wrapping.

### Text resize handles

When a text box is selected, resize handles appear. Use these to resize the text bounds visually.

### Text overflow

Text overflow is detected during preflight. If the text is wider than its box or appears clipped, preflight can warn you before export.

---

## Image and reference-image tools

### Add image

Use:

```text
Toolbar → Add Image
```

or:

```text
Insert → Add Image...
```

Image items can be positioned, scaled, resized, layered, and given opacity.

### Reference image

A reference image is used as a guide or source for tracing.

Use:

```text
Toolbar → Reference Image
```

or:

```text
Insert → Load Reference Image...
```

### Clear reference

Removes the reference image from the page.

### Bake reference

Turns the current reference image into a normal page image item.

### Reference fit modes in automation

CLI automation supports reference fit modes:

- `stretch`
- `contain`
- `cover`
- `manual`

### Missing images

If an image file is missing, the app should preserve the image item as a placeholder instead of silently deleting it. This allows the project to open and gives the user a chance to relink or repair assets.

---

## Lasso-to-vector and motif tracing

The lasso-to-vector system converts reference artwork into editable vector motifs.

There are two major modes:

### Trace Motif

Use this for filled shapes, silhouettes, logos, decorative motifs, and object-like forms.

```text
Toolbar → Trace Motif
```

or:

```text
Tools → Trace Motif
```

### Trace Centerline

Use this for linework, ink drawings, floral strokes, signatures, and thin stroke-like artwork.

```text
Toolbar → Trace Centerline
```

or:

```text
Tools → Trace Centerline
```

### How to use lasso tracing

1. Load a reference image.
2. Choose **Trace Motif** or **Trace Centerline**.
3. Draw a lasso around the artwork region.
4. Finish the lasso by double-clicking, right-clicking, pressing Enter, or pressing Space.
5. The app creates an editable vector motif.
6. Select the motif and use **Edit Nodes** for fine adjustments.

### Trace controls while drawing the lasso

| Action | Result |
|---|---|
| Draw with mouse | Adds lasso points |
| Enter | Finish lasso if enough points exist |
| Space | Finish lasso if enough points exist |
| Double-click | Finish lasso |
| Right-click | Finish or cancel depending on state |
| Delete / Backspace | Remove last lasso point |
| Escape | Cancel lasso tracing |

### Advanced vector optimization

The current lasso vector pipeline is designed to avoid primitive “thousands of single points” output.

The vectorizer performs:

1. lasso input cleanup,
2. jitter and duplicate-point removal,
3. binary mask / contour extraction,
4. contour simplification,
5. corner detection,
6. adaptive simplification,
7. editable node budgeting,
8. Bézier handle generation,
9. straight segment preservation,
10. vector motif construction.

### Why this matters

Raw tracing can produce huge point clouds. Huge point clouds are difficult to edit, slow to render, and ugly as production vectors. The upgraded optimizer aims to produce fewer, cleaner, more meaningful vector nodes while preserving important corners and curves.

### Trace Motif vs Trace Centerline

Use **Trace Motif** when the selected art is a filled region.

Examples:

- logo silhouettes,
- icons,
- floral shapes,
- filled decorative ornaments,
- solid object outlines.

Use **Trace Centerline** when the selected art is linework.

Examples:

- ink strokes,
- thin vines,
- signatures,
- sketch lines,
- hand-drawn curves.

### Tips for best trace quality

- Use high-resolution reference images.
- Increase contrast before importing if the source is faint.
- Lasso slightly outside the object boundary.
- Use motif tracing for filled shapes and centerline tracing for strokes.
- After tracing, use Smooth Motif only as needed; too much smoothing can remove intentional detail.
- Use Edit Nodes for final artistic cleanup.

---

## Vector editing

Vector motifs support node and handle editing.

### Enter node edit mode

Select a vector motif, then use:

```text
Toolbar → Edit Nodes
```

or:

```text
Tools → Edit Selected Vector Nodes
```

### Exit node edit mode

Use:

```text
Tools → Finish Vector Node Edit
```

or press:

```text
Escape
```

### Node edit keys

| Key | Result |
|---|---|
| Escape | Exit node edit mode |
| Delete / Backspace | Delete the selected vector node |
| R | Reset selected node handles |
| Backslash | Reset selected node handles |

### Vector node operations

The codebase supports or scaffolds operations for:

- moving a node,
- moving an in/out handle,
- inserting a node,
- deleting a node,
- resetting handles,
- smoothing with a brush,
- smoothing entire selected vector,
- recoloring selected vector,
- exporting selected vector SVG.

### Vector styling

Selected vectors can be edited through Properties:

- fill color,
- stroke color,
- stroke width,
- opacity,
- name.

### Smooth brush

The vector smooth brush allows local smoothing of vector contours. It is useful after tracing, especially when a section has too much local noise.

---

## Layout, alignment, and smart design tools

### Alignment tools

Use the toolbar or Arrange menu:

- Align Left
- Align Center
- Align Right
- Align Top
- Align Middle
- Align Bottom

These are useful when multiple items are selected.

### Distribution tools

Use:

- Distribute Horizontally
- Distribute Vertically

These spread selected items evenly along an axis.

### Auto layout helpers

- **Auto Center** centers selected content.
- **Auto Space** improves spacing between selected items.
- **Fit To Margins** fits selected content within page margins.
- **Flow Text** links or flows selected text boxes where supported.

### Golden ratio mutation

The **Phi Mutate** / **Golden Ratio Mutation** tool attempts a design variation based on golden-ratio spacing/layout ideas.

### Saliency overlay

The saliency overlay helps visualize visual attention or heat-map behavior.

Use:

- **Saliency** / **Show Saliency Overlay**
- **Clear Heatmap** / **Clear Saliency Overlay**

---

## Textures, styling, and visual effects

### Style presets

The toolbar includes a style preset dropdown. The Design menu also exposes Style Presets.

Style presets can quickly apply a coordinated look.

### Text styles

Text can use named styles such as headings or body styles depending on the available StyleManager configuration.

### Texture fills

Textures can be applied to:

- text,
- backgrounds,
- possibly other supported drawable items.

Texture inputs may include:

- texture preset,
- texture image path,
- texture recipe JSON.

### Advanced texture dialog

The advanced texture dialog is used to generate or modify procedural texture settings.

### Effects

Text supports visual effects such as:

- shadow,
- glow,
- stroke,
- opacity,
- texture overlay.

---

## Layers panel

The project includes a Layers panel and layer-model groundwork.

Current layer data supports:

- layer ID,
- layer name,
- visibility flag,
- lock flag,
- order,
- default layer support,
- item-level `layerId` persistence.

The panel currently connects common item actions such as raise, lower, and delete. The deeper visibility/locking/grouping layer workflow is intended as a future expansion.

---

## Preflight and print checks

Preflight is used before export to catch issues that may not be obvious while editing.

Use:

```text
Toolbar → Preflight
```

or:

```text
Tools → Preflight
```

Preflight can check for issues such as:

- missing assets,
- text overflow,
- image resolution problems,
- print-safety problems,
- page/export readiness issues.

CLI automation also supports preflight with options:

```json
{
  "action": "preflight",
  "path": "out/preflight.json",
  "minImageDpi": 200,
  "failOnErrors": true,
  "failOnWarnings": false
}
```

---

## Saving, loading, packaging, and recovery

### Save project

Use:

```text
File → Save Project...
```

Shortcut:

```text
Ctrl+S
```

The app uses atomic save behavior so an interrupted write is less likely to corrupt the project file.

### Load project

Use:

```text
File → Load Project...
```

Shortcut:

```text
Ctrl+O
```

Project loading is designed to be non-destructive. The project is parsed and validated before replacing the current open document.

### Backup files

When saving over an existing project, the app can preserve the previous project as a `.bak` file.

### Autosave and recovery

The recovery system writes autosave/recovery snapshots. On startup, the app can inspect recovery candidates and restore the best available recovery file.

### Project packages

The package system supports a self-contained directory-style package with project JSON and copied assets.

Package structure:

```text
project-package/
  project.json
  assets/
    ...copied assets...
```

Use packages when moving a project between machines or sharing with someone else.

### Relative asset paths

Normal project saves try to store asset paths relative to the project file location. This makes project folders more portable.

---

## Exporting

### Current page PDF

Use:

```text
File → Export Current Page PDF...
```

or toolbar:

```text
Page PDF
```

### Print-ready PDF

Use toolbar:

```text
Print PDF
```

This is intended for print-oriented output.

### Full brochure PDF

Use:

```text
File → Export Brochure PDF...
```

or toolbar:

```text
Book PDF
```

### High-resolution brochure PDF

Use:

```text
File → Export High-Resolution PDF...
```

or toolbar:

```text
Book 600
```

### PNG export

Use toolbar:

```text
PNG
```

or:

```text
Web PNG
```

### SVG export

Use:

```text
File → Export SVG...
```

or toolbar:

```text
SVG
```

### Project SVG package

Use:

```text
File → Export SVG Package...
```

or toolbar:

```text
Book SVG
```

This writes one SVG per page plus a package manifest.

### Selected motif SVG

Select a vector motif, then use:

```text
Toolbar → Export Motif SVG
```

or:

```text
Tools → Export Selected Motif SVG...
```

---

## Keyboard and mouse controls

### Global shortcuts

| Shortcut | Action |
|---|---|
| Ctrl+S | Save Project |
| Ctrl+O | Load Project |
| Ctrl+Z | Undo |
| Ctrl+Y / Ctrl+Shift+Z, depending on platform Qt mapping | Redo |
| Delete | Delete Selected |
| Ctrl++ | Zoom In |
| Ctrl+- | Zoom Out |
| F10 | Toggle Ghost UI |
| Escape | Restore normal chrome / exit certain modes |

### Canvas movement

| Key | Action |
|---|---|
| Left Arrow | Move selected item(s) 1 px left |
| Right Arrow | Move selected item(s) 1 px right |
| Up Arrow | Move selected item(s) 1 px up |
| Down Arrow | Move selected item(s) 1 px down |
| Shift + Arrow | Move selected item(s) 10 px |

### Zooming

| Control | Action |
|---|---|
| Ctrl + Mouse Wheel Up | Zoom In |
| Ctrl + Mouse Wheel Down | Zoom Out |
| View → Fit View | Fit current page in view |

### Lasso tracing mode

| Control | Action |
|---|---|
| Mouse drag/click | Add lasso points |
| Enter | Finish lasso |
| Space | Finish lasso |
| Double-click | Finish lasso |
| Delete / Backspace | Remove last lasso point |
| Escape | Cancel tracing |

### Vector node edit mode

| Control | Action |
|---|---|
| Escape | Finish/cancel node edit mode |
| Delete / Backspace | Delete selected node |
| R | Reset selected node handles |
| Backslash | Reset selected node handles |

### Line drawing mode

| Control | Action |
|---|---|
| Escape | Cancel line drawing |

---

## CLI automation

Solace Brochure Studio can run automation files to generate projects and exports without manually using the UI.

Run an automation file:

```bash
SolaceBrochureStudio --automation automation.json
```

List supported actions:

```bash
SolaceBrochureStudio --list-cli-actions
```

### Automation file structure

```json
{
  "resetProject": true,
  "operations": [
    { "action": "newProject" },
    { "action": "addPage", "template": "cover", "applyTemplate": true },
    { "action": "addText", "text": "Automated headline", "x": 220, "y": 900, "width": 1200, "style": "Heading", "textColor": "#202020" },
    { "action": "setBackground", "color": "#F7F3EA" },
    { "action": "saveProject", "path": "out/automation-project.json" },
    { "action": "export", "format": "pdf", "scope": "project", "preset": "highres", "path": "out/brochure-600.pdf" }
  ]
}
```

### Supported automation actions

The CLI supports actions including:

#### Project actions

- `newProject`
- `loadProject { path }`
- `saveProject { path }`
- `loadTemplate { path }`
- `saveTemplate { path }`

#### Page actions

- `addPage { template: cover|inside, applyTemplate?: bool }`
- `selectPage { index }`
- `duplicatePage`
- `applyTemplate { template: cover|inside }`
- `setPageSize { name, scaleContent?: bool }`
- `setMargin { value }`
- `setColumns { value }`
- `setGutter { value }`
- `setBleed { value }`
- `setPageDnaLink { enabled }`
- `syncPageDna`

#### Background and reference image actions

- `setBackground { color }`
- `setBackgroundTexture { texturePreset?|texturePath?|textureRecipe? }`
- `clearBackgroundTexture`
- `setReferenceImage { path, opacity?: 0-1, fitMode?: stretch|contain|cover|manual, x?, y?, width?, height? }`
- `clearReferenceImage`
- `bakeReferenceImage`

#### Selection and arrangement actions

- `clearSelection`
- `selectAllItems { type?: any|text|line|image|vector }`
- `selectItems { index?|indices?, additive?: bool, type?: any|text|line|image|vector }`
- `moveSelected { dx, dy }`
- `raiseSelected`
- `lowerSelected`
- `deleteSelected`
- `alignSelection { mode: left|centerX|right|top|centerY|bottom }`
- `distributeSelection { axis: horizontal|vertical }`
- `autoCenterSelection`
- `autoSpaceSelection`
- `fitSelectionToMargins`

#### Undo and redo

- `undo`
- `redo`

#### Text actions

- `addText { text|html, x, y, width, style?, fontFamily?, fontSize?, bold?, italic?, tracking?, lineSpacing?, alignment?, textColor?, opacity?, shadow?, glow?, stroke?, texturePreset?, texturePath?, textureRecipe? }`
- `updateSelectedText { text?|html?, x?, y?, width?, z?, style?, fontFamily?, fontSize?, bold?, italic?, tracking?, lineSpacing?, alignment?, textColor?, opacity?, shadow?, glow?, stroke?, clearTexture?, texturePreset?, texturePath?, textureRecipe? }`
- `applyTextStyle { name }`
- `flowSelectedText`

#### Image actions

- `addImage { path, x, y, width?, height?, opacity? }`
- `updateSelectedImage { x?, y?, width?, height?, opacity?: 0-1, z? }`

#### Line and vector actions

- `addLine { x1, y1, x2, y2, color?, width?, vector?:bool, name? }`
- `updateSelectedLine { color?, width? }`
- `traceReferenceMotif { points, threshold?, fillColor?, strokeColor?, strokeWidth?, name? }`
- `traceReferenceCenterline { points, threshold?, strokeColor?, strokeWidth?, name? }`
- `updateSelectedVector { fillColor?, strokeColor?, strokeWidth?, opacity?, name? }`
- `smoothSelectedVector { epsilon?, tension? }`
- `moveVectorNode { contour, node, x, y }`
- `moveVectorHandle { contour, node, handle: in|out, x, y, mirror? }`
- `insertVectorNode { x, y }`
- `deleteVectorNode { contour, node }`
- `resetVectorHandles { contour, node }`
- `smoothVectorBrush { x, y, radius?, strength?, contour? }`
- `exportSelectedMotifSvg { path }`

#### Design actions

- `applyStylePreset { name }`
- `applyOutroLayout`
- `mutateSelectionGoldenRatio { variant?: 0-9 }`
- `showSaliencyOverlay { opacity?: 0-1 }`
- `clearSaliencyOverlay`

#### Preflight and export actions

- `preflight { path?, minImageDpi?, failOnErrors?, failOnWarnings? }`
- `export { format: pdf|png|svg, scope: current|project, preset?: print|web|highres, path }`

For project SVG export, `format=svg` and `scope=project` writes one SVG per page plus `package-manifest.json` into the target folder.

---

## Project file notes

Project files are JSON-based and include metadata such as:

- format name,
- schema version,
- app version,
- pages,
- items,
- layers,
- relative asset paths,
- item UUIDs.

### Stable item IDs

Items are assigned stable UUIDs. These IDs are important for:

- serialization,
- future grouping,
- future layer locking/visibility,
- undo/redo reliability,
- plugin and scripting APIs.

### Schema validation

The project loader validates the root object and expected structures before replacing the live project. This reduces the chance of corrupt projects destroying the current document state.

### Forward compatibility

Future schema versions should be rejected safely rather than partially loaded incorrectly.

---

## Developer architecture notes

The codebase includes the following major components:

### `MainWindow`

Application-level UI orchestration, page commands, menu/toolbar wiring, export commands, and high-level editor actions.

### `CanvasView`

Canvas interaction layer. Handles selection, drag behavior, zoom, line drawing, lasso drawing, trace selection, and vector node edit mode.

### `TextBoxItem`

Rich editable text item with styling, effects, texture support, layout modes, fit-to-content behavior, and resize handles.

### `PlacedImageItem`

Canvas image item for placed raster assets.

### `VectorMotifItem`

Editable vector motif item with contours, nodes, handles, fill/stroke styling, smoothing, and SVG export support.

### `ReferenceVectorizer`

Converts reference image regions into vector contours.

### `AdvancedVectorOptimizer`

Optimizes raw traced contours into lower-node editable vector contours with corner preservation and Bézier handles.

### `ProjectSerializer`

Saves and loads project JSON.

### `ProjectValidator`

Validates project JSON/schema structure before project mutation.

### `AssetManager`

Centralized image and asset handling, including relative paths, EXIF-aware loading, hashing, atomic copy, and placeholders.

### `ProjectRecoveryManager`

Autosave and recovery snapshot management.

### `ProjectPackageService`

Self-contained package save/load support.

### `LayerModel`

Layer-data foundation, including layer IDs, names, visibility, lock state, order, and default layer support.

### `UndoCommands`

Scaffolding for command-based undo operations.

### Documentation files

The repository also includes developer documentation under `docs/`, including:

- `OWNERSHIP.md`
- `SERIALIZATION.md`
- `COORDINATES.md`
- `RENDERING.md`
- `THREADING.md`
- `UNDO.md`
- `LAYERS.md`
- `RECOVERY.md`
- `VECTORIZATION_PIPELINE.md`
- `STATE_OF_ART_LASSO_VECTORIZATION.md`
- `GRAPHICS_ITEM_AUDIT.md`

---

## Troubleshooting

### Linker errors for new classes

If you see errors such as unresolved externals for classes like `AssetManager`, `ProjectRecoveryManager`, `ProjectValidator`, or `AdvancedVectorOptimizer`, make sure the matching `.cpp` files are listed in `CMakeLists.txt`.

Example:

```cmake
src/AssetManager.cpp src/AssetManager.h
src/AdvancedVectorOptimizer.cpp src/AdvancedVectorOptimizer.h
src/ProjectRecoveryManager.cpp src/ProjectRecoveryManager.h
```

Then clean and regenerate the build folder.

### Qt header or component errors

Make sure Qt6 is installed with the required components:

```cmake
find_package(Qt6 REQUIRED COMPONENTS Widgets PrintSupport Svg OpenGLWidgets)
```

If you remove forced OpenGL viewport usage, the app can run with the standard QWidget viewport while still linking your configured Qt components.

### App opens with the manifesto but you do not want it

Normal startup should not show the manifesto. Use the manifesto only when explicitly requested:

```bash
SolaceBrochureStudio --manifesto
```

or:

```bash
SolaceBrochureStudio --menifesto
```

### Project loads but assets are missing

Use project packages when sharing projects. Normal projects may use relative paths; if the project folder structure changes, image references may need to be restored.

### Text wraps unexpectedly

Select the text box and use fit-to-content or a single-line layout mode for large titles. Also make sure the text box is wide enough for the selected font size.

### Lasso trace has too many points

Use Trace Motif for filled shapes and Trace Centerline for linework. Start with a clean, high-contrast reference image. Use Smooth Motif after tracing if needed.

### Export looks different from the canvas

Run preflight first. Check for missing assets, text overflow, unsupported effects, or page scaling differences.

---

## Recommended future roadmap

The current app is stable enough for practical design/testing workflows. The next major improvements should focus on deeper production-grade behavior:

### Short-term

- Finish wiring layer visibility and locking to the live canvas.
- Add user-facing relink UI for missing assets.
- Add lasso/vector trace preview with detail, smoothness, and corner-sensitivity sliders.
- Show raw point count vs optimized node count after tracing.
- Add more command-based undo coverage.
- Add deterministic export tests.

### Medium-term

- Make `DocumentModel` the single source of truth instead of relying primarily on live scene state.
- Add group/ungroup support.
- Add full layer ordering and item-to-layer assignment UI.
- Add render modes for screen, preview, print, export, and thumbnail.
- Move heavy vector and texture operations to worker threads using pure-data input/output.

### Long-term

- Add intelligent/magnetic lasso based on image edge following.
- Add true live vectorization preview.
- Add exact line/arc/circle detection for technical artwork.
- Add object-aware segmentation as an optional advanced tracing path.
- Add plugin/script APIs over safe document commands.
- Add cross-platform installers and CI release builds.

---

## License

Add your project license here.

Suggested options:

- MIT for permissive open source,
- GPL if derivative openness is required,
- commercial/proprietary license if this is private product software.

---

## Credits

Solace Brochure Studio is a C++/Qt design application focused on brochure production, vector motif creation, print export, and automation-driven layout workflows.
