# Colour Studio — immutable colour model (v1)

## Purpose

Generate native-like custom colour families with all four Apple appearance variants, using reproducible numerical rules. The colour is the star; expose technical information only on demand. sRGB only; no gradients, custom reference-model editor, or automatic contrast correction.

## Reference

Apple Human Interface Guidelines, [Color](https://developer.apple.com/design/human-interface-guidelines/color), published system colour table (June 2025 update; consulted November 2026), plus the six numbered iOS/iPadOS greys. All 48 chromatic and 24 grey entries are stored as exact 8-bit triplets in `index.html`. The HIG currently supplies one shared chromatic table, not an independent macOS numeric palette; both platform labels refer to that same snapshot. Do not invent distinct macOS data. These are design values, not guarantees of live OS resolution.

Appearance order: default light, default dark, increased contrast light, increased contrast dark.

## Engine

1. Convert sRGB through its standard piecewise transfer function into linear RGB, then OKLab using Björn Ottosson's matrices; express as OKLCH. Hue is circular. Achromatic colours have undefined hue, represented internally as zero.
2. Relative chroma Q = C / Cmax(L,H). Cmax is the first sRGB gamut boundary along a constant-lightness, constant-hue ray, calculated numerically by bracketing then bisection. The final output is gamut-limited in chroma and rounded to 8-bit sRGB.
3. **Vivid profile:** use 11 non-brown reference colours. Each anchor is its default light hue. At each anchor store L, Q and signed circular hue difference from default light for every appearance. Circular piecewise-linear interpolation across the anchors defines the profile at every base hue.
4. **Complex profile:** transplant brown's four measured L, Q and hue offsets to any base hue. It is an extrapolated model, not a second Apple palette. Family linearly blends the vivid and complex profile parameters. Original system brown stays complex unless individually overridden.
5. For hue-only inputs, use the selected profile. For a seed input, find the base hue whose anchor appearance hue matches the input hue (numerically minimize circular difference); intermediate uses the mean of the two default profiles. Transfer the seed's lightness offset and relative-chroma ratio to every appearance. For an exact anchor, preserve the original seed when other style transformations are neutral. Automatic anchor selection chooses the smallest approximate OKLab distance to a matching reference profile.
6. Apply the style transformations: intensity multiplies Q (clamped 0–1), lightness adds an offset (clamped 0–1), hue rotation adds degrees, warmth adds w × sin((60° − H) in radians). Warmth is a defined stylistic distortion, not a physical temperature measurement. Recalculate C from Q and Cmax at transformed L and H. Manual appearance overrides replace generated outputs; a locked row returns its frozen values. Exact original Apple anchors at neutral settings reproduce the source HEX values.
7. Neutrals preserve the source lightness ladder per appearance. Independent tint uses chosen absolute C and H, not Q. Coordinated tint uses the source grey's chroma scaled by the neutral tint control, and applies the palette's temperature and hue rotation to the reference grey hue. Optional near-white/near-black backgrounds use fixed L values with a fraction of the tint; no replacement of semantic native backgrounds is implied.
8. Contrast is the WCAG relative luminance ratio of actual quantized RGB values. Report against black and white; previews may additionally report the actual background. Reference thresholds are 4.5:1 and 7:1. Never alter or block colour output on this basis.

## Invariants

All colours always have four variants, independent of preview selection. Preserve exact seed or source values at their defined anchors. Never clip RGB as an aesthetic adjustment; limit chroma to the gamut before conversion. Preserve source definitions when applying global changes. Keep the tool fully offline and dependency-free. Do not silently change the model during UI-only work.

## Tests

The built-in `selfTests()` exercises source round-trips, exact original palette reproduction, grey round-trips, generated hue/family grid, four seed anchors, contrast, achromatic invariance, hue wrapping, HSL conversion and all-appearance exports. Run through the About dialog. Additional browser checks for import/export, undo and layout are appropriate after UI edits.
