---
title: "UI & UX Testing"
date: 2025-08-25 08:30:00
categories: [Software Testing and Quality Assurance]
tags: [UI Testing, UX Testing, Accessibility, Heuristics]
image: /assets/images/STQA/ui-ux-testing.png
---

# UI & UX Testing

This article summarizes the *UI & UX Testing* material (Group 2 slides) — covering differences, key focus areas, workflow, technique examples, and a heuristic checklist for design evaluation. The content is structured to be easily understood and can be used immediately as a quick reference in QA tasks or practice.

---

## Fundamental Differences: UI vs UX
- **UI Testing** = focuses on interface elements: colors, icons, button sizes, layout, visual consistency, and responsiveness.
  *Example*: ensuring the **Login** button appears in the correct position and the font size is legible.
- **UX Testing** = focuses on the overall user experience: task flow, ease of achieving goals, and user satisfaction.
  *Example*: a user can find a product and complete a purchase without confusion.

---

## Key Focus Areas for UI Testing
1. **Visual Consistency**  
   - Colors, typography, iconography, spacing, and button styles must be uniform across all pages.
   - Techniques: manual checklist & visual regression testing (Percy, Applitools).
2. **Responsiveness**  
   - The UI must be adaptive across various screen sizes: smartphone → tablet → desktop.
   - Techniques: manual testing + tools (BrowserStack, Responsively App).
3. **Compatibility**  
   - Cross-browser & cross-platform (Chrome, Firefox, Safari, Edge; macOS, Windows, iOS, Android).
   - Tools: BrowserStack, Sauce Labs, LambdaTest.

---

## Key Focus Areas for UX Testing
1. **Workflow**  
   - terative: planning → user recruitment → test session execution → analysis → design iteration.
   - Practice examples: generative interviews → card sorting → tree testing → prototype validation.
2. **Usability**  
   - Observe real users completing tasks, identify obstacles, and recommend improvements.
3. **Accessibility**  
   - Adhere to WCAG: color contrast, keyboard navigation, screen reader support.
   - Tests: contrast testing, keyboard-only flows, and assistance tools like Axe or Lighthouse.

---

## Frequently Used Methods & Tools
- **Heatmaps** (Hotjar, Crazy Egg, Microsoft Clarity) — to identify frequently clicked/viewed areas.
- **A/B Testing** — compare two UI variations for specific metrics.
- **Prototype testing** (Maze, Figma user testing) — quickly validate ideas without full development.
- **Automated visual regression** (Percy, Applitools) — detect unintended visual changes.

---

## Heuristic Evaluation — Practical Checklist
Use the following heuristics as a checklist when reviewing a design:

- **Visibility of system status** — the system provides clear feedback on every action.
- **Match between system and the real world** — use familiar language/icons.
- **User control and freedom** — provide clear undo/exit options.
- **Consistency and standards** — uniform design across features.
- **Error prevention** — design prevents errors; don't rely only on error messages.
- **Recognition rather than recall** — display affordance; don't force user memory.
- **Flexibility and efficiency of use** — provide shortcuts for expert users.
- **Aesthetic and minimalist design** — avoid irrelevant information.
- **Help and documentation** — clear and helpful error messages; easily searchable documentation.

---

## Brief Practical QA Checklist Examples
- [ ] Primary buttons (CTA) are visible and have contrast.
- [ ] Font size for body text is ≥14px (desktop) and remains readable on mobile.
- [ ] Navigation is accessible via keyboard (Tab order).
- [ ] All images have descriptive `alt` attributes.
- [ ] Text vs background color contrast meets the minimum WCAG ratio.
- [ ] Forms display clear error messages and automatically focus on the problematic field.
- [ ] The main page loads within a reasonable time (performance smoke test).

---

## Recommended Workflow Integration into SDLC
1. **Before development**: create design & define success metrics.
2. **During development**: run unit + component tests, visual regression.
3. **Before release**: usability testing with sample users, accessibility audit, performance smoke test.
4. **After release**: collect analytics & heatmaps → iterate based on data.

---

## Notes & Sources
This material is sourced and summarized from Group 2 presentation slides (UI/UX Testing). See below for detailed slides and original reference.

[View Group 2 Presentation Slides](https://drive.google.com/file/d/12N-ugshIQSDrLutsQgo-qsxfjeBa3daP/view?usp=sharing)