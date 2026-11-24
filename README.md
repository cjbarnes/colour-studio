# Colour Studio

A self-contained, visually led colour generator for Apple-platform apps. No framework, build step, network calls, accounts or backend. Open `index.html` in a modern browser (or host it as a static site, e.g. GitHub Pages).

## Use

- **Palette:** starts with the twelve published Apple system colours. Open **Palette style…** to reveal the optional global controls (hidden by default). Use family, intensity, lightness, temperature and hue rotation to transform the palette. Below the four appearance previews, compare four single-line strips ordered independently by actual output OKLCH hue, and an interactive OKLCH plot with **Hue and chroma** (absolute chroma, radial scale fitted to the maximum in the selected appearance) and **Hue only** modes. The plot's appearance selector also controls the live colour similarity matrix below it. The matrix shows OKLab Euclidean distance × 100, including lightness, hue and chroma, with a fixed shading scale and the three closest pairs. Select a swatch, plot marker, legend entry or matrix header to edit a colour. These analyses update live and do not affect generation. Click any swatch to edit its definition, individual settings or manual overrides. Manage colours for duplication, ordering and deletion. Lock a colour to freeze its four outputs.
- **Single colour:** select a hue, or enter an exact or intermediate seed colour (HEX, RGB, HSL or OKLCH). Select the anchor appearance, or find the nearest reference. The Apple-like, Half intensity and Brown family presets set individual family/intensity to 0/100, 0/50 and 100/100 respectively, retaining other style settings, the seed and manual appearance overrides. Add the result to the palette. Inspect contrast and technical values on demand.
- **Neutrals:** retain system greys, coordinate a tint with palette style, or choose an independent hue/chroma. Optional near-white and near-black backgrounds. Click neutral swatches to copy.
- **Previews:** all four appearances by default; everyday light/dark pair or one appearance. Palette swatches, icons, lists and UI-element views. Contrast is informational only.
- **Save/Open:** portable editable project JSON. Local storage retains the current project where the browser permits it; always save a JSON file for reliable portability. Undo/redo applies to saved palette and neutral changes, not unsaved single-colour drafts.
- **Export:** all four appearances, optionally with neutrals. Xcode asset ZIP, final-colours JSON, CSS, SwiftUI asset references, CSV, plain text and static HTML palette sheet.

## Xcode

Unzip the asset export. Add `Colours.xcassets` to the app target, or copy individual `.colorset` directories into an existing asset catalogue. Use `Color("Blue")` (or the exported Swift file). Asset names are sanitized and disambiguated. Xcode selects appearance variants natively. The archive has been structurally verified; it has not been compiled with Xcode in this environment.

## Reference and verification

See `MODEL.md` for the agreed mathematical rules. The current Apple HIG publishes a shared numeric chromatic table rather than separate macOS numeric values; the iOS/iPadOS six-grey ramp is used for neutrals. Live system colours can differ from the documented design reference.

Click **About the model → Run numerical self-tests** to run the built-in checks. V1 was checked in Chromium with the page injected from the local HTML (browser navigation in the test environment is restricted), including 573 numerical checks, UI interactions, project serialization, ZIP integrity and a narrow viewport. V1.1 additionally passed the original 573 checks and Chromium interaction checks for presets, undo, sidebar visibility, hue sorting, wheel selection and a 390px viewport. V1.2 passed 578 numerical checks and JavaScript rendering/event tests using a synthetic DOM, including all four appearances, fixed plot radii, matrix cells, and empty/duplicate/achromatic palettes. V1.2.1 passed 580 numerical checks and synthetic-DOM checks of adaptive plot scaling in all four appearances, very low chroma, empty palettes and unchanged hue-only mode. V1.2 and v1.2.1 have not been visually tested in a browser. Safari and native Xcode compilation are not tested here.

## Hosting

For a GitHub repository, upload these files to the root and enable Pages from the main branch. `index.html` is the entire application. Keep project JSON files out of the public repository unless you deliberately want to publish them.
