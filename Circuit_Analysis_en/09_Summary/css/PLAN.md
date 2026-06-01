# Local CSS Replacement Plan for Tailwind

## Current Status Analysis

The four HTML files (`01_DC_Resistive_Circuits.html` ~ `04_Two_Port_Networks.html`) all import Tailwind CSS via CDN (`<script src="https://cdn.tailwindcss.com"></script>`), but **Tailwind is only used for the "Return to Home" button component**; all other styles are custom CSS within `<style>` tags.

### Tailwind Classes Used by the Return to Home Button

| Element | Tailwind Classes |
|---------|-----------------|
| Outer container `<div>` | `flex justify-start mb-4` |
| Button link `<a>` | `inline-flex items-center gap-1 text-sm font-medium text-blue-600 bg-blue-50 hover:bg-blue-100 hover:text-blue-800 transition-colors duration-200 rounded-full px-4 py-2 shadow-sm` |
| SVG icon `<svg>` | `w-4 h-4` |

A total of **15 Tailwind utility classes**, all serving this single button component.

---

## Modification Plan

### Step 1: Create `css/back-button.css`

Create a new file `./css/back-button.css`, replacing the above Tailwind classes with equivalent hand-written CSS:

| Tailwind Class | Equivalent CSS | Description |
|---|---|---|
| `flex` | `display: flex` | Flexbox layout |
| `justify-start` | `justify-content: flex-start` | Align to start of main axis |
| `mb-4` | `margin-bottom: 1rem` | Bottom margin 16px |
| `inline-flex` | `display: inline-flex` | Inline flexbox layout |
| `items-center` | `align-items: center` | Center on cross axis |
| `gap-1` | `gap: 0.25rem` | Child element spacing 4px |
| `text-sm` | `font-size: 0.875rem; line-height: 1.25rem` | 14px font size |
| `font-medium` | `font-weight: 500` | Medium font weight |
| `text-blue-600` | `color: #2563eb` | Text color |
| `bg-blue-50` | `background-color: #eff6ff` | Light blue background |
| `hover:bg-blue-100` | `&:hover { background-color: #dbeafe }` | Hover background |
| `hover:text-blue-800` | `&:hover { color: #1e40af }` | Hover text color |
| `transition-colors duration-200` | `transition: color 0.2s, background-color 0.2s, border-color 0.2s` | 200ms color transition |
| `rounded-full` | `border-radius: 9999px` | Fully rounded capsule shape |
| `px-4 py-2` | `padding: 0.5rem 1rem` | Padding 16px/8px |
| `shadow-sm` | `box-shadow: 0 1px 2px rgba(0,0,0,0.05)` | Subtle shadow |
| `w-4 h-4` | `width: 1rem; height: 1rem` | 16px icon size |

The CSS file will use semantic class names:

```css
/* Container */
.back-btn-wrap { ... }

/* Button link */
.back-btn { ... }
.back-btn:hover { ... }

/* SVG icon */
.back-btn svg { ... }
```

### Step 2: Modify the Four HTML Files

Each file requires **two changes**:

**Change 1 -- Head Reference**

```html
<!-- Remove -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- Replace with -->
<link rel="stylesheet" href="./css/back-button.css">
```

**Change 2 -- Button HTML**

```html
<!-- Before modification -->
<div class="flex justify-start mb-4">
  <a href="../index.html"
    class="inline-flex items-center gap-1 text-sm font-medium text-blue-600 bg-blue-50 hover:bg-blue-100 hover:text-blue-800 transition-colors duration-200 rounded-full px-4 py-2 shadow-sm">
    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
      ...
    </svg>
    Return to Home
  </a>
</div>

<!-- After modification -->
<div class="back-btn-wrap">
  <a href="../index.html" class="back-btn">
    <svg ...>
      ...
    </svg>
    Return to Home
  </a>
</div>
```

### Step 3: Verification

After modification, open any of the HTML files in a browser to confirm the return button styling is consistent with before (blue capsule-shaped button, hover color change, shadow, icon alignment).

---

## File List

| File | Action |
|------|--------|
| `./css/back-button.css` | Create new |
| `./01_DC_Resistive_Circuits.html` | Modify head reference + button class |
| `./02_Sinusoidal_Steady_State.html` | Same as above |
| `./03_Transient_Analysis.html` | Same as above |
| `./04_Two_Port_Networks.html` | Same as above |
| `./css/PLAN.md` | This file (can be deleted after completion) |

---

## Risks and Notes

- This plan only replaces the 15 Tailwind utility classes used for the button; it does not introduce Tailwind's full CSS reset/preset.
- The button styling uses semantic class names (`back-btn`); future adjustments to the button appearance only require modifying the single `back-button.css` file.
- If other components need similar button styling in the future, the classes in `back-button.css` can be reused.
