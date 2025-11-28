
# Accessibility Rules – Focused Accessibility Reviewer
These rules expand the logic used by `agent.md`. They serve as the authoritative accessibility checklist and issue detection guide, aligned with WCAG 2.2 AA.

---

# 1. Perceivable
- Ensure **text alternatives** for non-text content (`alt` for images, `aria-label`/`aria-labelledby` for icons).
- Provide **captions** for video, **transcripts** for audio.
- Maintain **color contrast** ≥ 4.5:1 for normal text, ≥ 3:1 for large text (≥18pt or 14pt bold).
- Avoid conveying information by **color alone**; include text or icons.

**Dangerous Patterns**
```html
<img src="chart.png">
<span style="color: red">Error</span>
```

**Safe**
```html
<img src="chart.png" alt="Sales by quarter bar chart">
<span class="error" aria-live="polite">Error: Invalid email address</span>
```

---

# 2. Operable
- All functionality available via **keyboard**; no keyboard traps.
- Provide **visible focus** indicators on interactive elements.
- Use **semantic HTML** (`button`, `a`, `label`) over `div`/`span` roles.
- Support **skip links** to main content.
- Avoid **auto-playing** media; provide controls.

**Dangerous Patterns**
```html
<div onclick="submitForm()">Submit</div>
```

**Safe**
```html
<button type="submit">Submit</button>
```

---

# 3. Understandable
- Use **clear labels**, **instructions**, and **error messages**.
- Associate labels with inputs (`label for` / `aria-labelledby`).
- Provide **error prevention** and **inline validation**.
- Preserve **language attributes** (`lang`) and consistent navigation.

**Dangerous Patterns**
```html
<input id="email">
```

**Safe**
```html
<label for="email">Email address</label>
<input id="email" name="email" type="email" aria-describedby="email-help">
<small id="email-help">We will send a confirmation link.</small>
```

---

# 4. Robust
- Use **valid HTML** and ARIA roles according to spec; avoid ARIA when native semantics suffice.
- Ensure components **work with screen readers** (NVDA, JAWS, VoiceOver).
- Provide **programmatic names** (`aria-label`) for custom controls.

**Dangerous Patterns**
```html
<div role="button">Menu</div>
```

**Safe**
```html
<button aria-expanded="false" aria-controls="menu">Menu</button>
```

---

# 5. Forms & Validation
- Link **errors** to inputs via `aria-describedby` or `aria-errormessage`.
- Use **required** indicators (`required`, `aria-required="true"`).
- Provide **help text** and **instructions** before input.
- Preserve **state** after submission errors.

**Dangerous Patterns**
```html
<input required>
<div class="error">Email is invalid</div>
```

**Safe**
```html
<input id="email" name="email" aria-invalid="true" aria-describedby="email-error" required>
<div id="email-error" role="alert">Enter a valid email address.</div>
```

---

# 6. Keyboard & Focus Management (React)
- Avoid `tabIndex` misuse; keep natural tab order.
- Manage **focus** on route changes and dialogs.
- Prevent **focus loss** on dynamic content.

**Dangerous Patterns**
```jsx
<div onClick={openModal} tabIndex={0}>Open</div>
```

**Safe**
```jsx
<button onClick={openModal}>Open</button>
// After modal opens
useEffect(() => {
  if (isOpen) modalRef.current.focus();
}, [isOpen]);
```

---

# 7. ARIA & Custom Widgets
- Use ARIA only when necessary; prefer native elements.
- Implement required **roles, states, and properties** (e.g., `role="dialog"`, `aria-modal="true"`, focus trapping).
- Ensure **keyboard interactions** follow WAI-ARIA Authoring Practices.

**Dangerous Patterns**
```html
<div role="dialog">...</div>
```

**Safe**
```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
  <button>Cancel</button>
  <button>Delete</button>
</div>
```

---

# 8. Media & Motion
- Provide **captions**, **transcripts**, and **audio descriptions** where needed.
- Expose **play/pause/stop** controls; disable autoplay.
- Offer **reduced motion** variants; respect `prefers-reduced-motion`.

**Dangerous Patterns**
```css
* { scroll-behavior: smooth; }
```

**Safe**
```css
@media (prefers-reduced-motion: reduce) {
  * { animation-duration: 0.01ms; animation-iteration-count: 1; }
}
```

---

# 9. Color, Themes & Contrast (Design System)
- Provide **dark/light** themes with sufficient contrast.
- Ensure **link** contrast and underline or alt-state indicator.
- Avoid low-contrast placeholders; use accessible skeleton loaders.

---

# 10. Tables, Charts & Data Viz
- Mark up tables with **headers** (`th`, `scope`).
- Offer **data table summaries** and allow **CSV export**.
- Provide **text equivalents** for charts and complex visuals.

**Safe**
```html
<table>
  <caption>Quarterly sales</caption>
  <thead>
    <tr><th scope="col">Quarter</th><th scope="col">Sales</th></tr>
  </thead>
  <tbody>
    <tr><th scope="row">Q1</th><td>$120k</td></tr>
  </tbody>
</table>
```

---

# 11. Mobile & Touch
- Ensure **target sizes** ≥ 44×44px.
- Support **zoom**; avoid disabling pinch-to-zoom.
- Maintain **responsive layouts** without horizontal scroll.

---

# 12. Testing & Tooling
- Include **automated checks** (axe, Lighthouse, eslint-plugin-jsx-a11y).
- Run **manual screen reader** and **keyboard** tests.
- Add **unit tests** for focus management and ARIA attributes.

---

# 13. Severity Rules
- **Critical**: Missing keyboard access, inaccessible dialogs, no alt text for key content, blocking focus.
- **High**: Insufficient contrast, missing labels, broken forms, autoplaying media.
- **Medium**: Missing landmarks, inconsistent focus styling, minor ARIA misuse.
- **Low**: Content readability issues, minor visual affordances.

---

# 14. Remediation Requirements
Every fix must:
- Be **specific and actionable**.
- Use **semantic HTML** and **accessible defaults**.
- Include **example patched code**.
- Include **tests** (axe/Lighthouse/unit) to prevent regressions.

---

