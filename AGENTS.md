# Colour Studio development

- `index.html` is a self-contained offline app; no dependencies or build step.
- Read `MODEL.md` before changing colour generation, reference data, contrast or appearance logic. Do not change the mathematical model for UI-only requests.
- Keep the interface visually led, with progressive disclosure. All four appearances are first-class. No P3, gradients, automatic contrast adjustment or custom reference models.
- Maintain editable JSON project compatibility; all export formats always include four appearances. Xcode export must remain an ordinary ZIP of colour sets.
- Run the built-in numerical self-tests after engine changes, and browser smoke tests of affected controls after UI changes. Be explicit about what was tested; do not claim Xcode or Safari testing without it.
- Use small focused commits. Keep stable releases easy to revert.
