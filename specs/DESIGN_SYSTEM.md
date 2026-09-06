# Design System — Hermes Landing Builder

> Visual vocabulary for the landing page builder. All tokens are defined as Tailwind v4 CSS `@theme` custom properties for direct use in templates.

---

## Color Palette

Modern indigo primary with warm amber accent and slate neutrals. Colors chosen for strong contrast on both light and dark backgrounds, and to look professional on published landing pages.

### Primary (Indigo) — Main actions, links, buttons

```css
@theme {
  --color-primary-50: #eef2ff;
  --color-primary-100: #e0e7ff;
  --color-primary-200: #c7d2fe;
  --color-primary-300: #a5b4fc;
  --color-primary-400: #818cf8;
  --color-primary-500: #6366f1;
  --color-primary-600: #4f46e5;
  --color-primary-700: #4338ca;
  --color-primary-800: #3730a3;
  --color-primary-900: #312e81;
  --color-primary-950: #1e1b4b;
}
```

Usage: Primary buttons, active nav links, accent borders, link text.

### Secondary (Slate) — Surfaces, backgrounds, borders, text

```css
@theme {
  --color-secondary-50: #f8fafc;
  --color-secondary-100: #f1f5f9;
  --color-secondary-200: #e2e8f0;
  --color-secondary-300: #cbd5e1;
  --color-secondary-400: #94a3b8;
  --color-secondary-500: #64748b;
  --color-secondary-600: #475569;
  --color-secondary-700: #334155;
  --color-secondary-800: #1e293b;
  --color-secondary-900: #0f172a;
  --color-secondary-950: #020617;
}
```

Usage: Card backgrounds (50), page backgrounds (50/100), borders (200), muted text (400/500), body text (700/800).

### Accent (Amber) — Highlights, CTAs, emphasis

```css
@theme {
  --color-accent-50: #fffbeb;
  --color-accent-100: #fef3c7;
  --color-accent-200: #fde68a;
  --color-accent-300: #fcd34d;
  --color-accent-400: #fbbf24;
  --color-accent-500: #f59e0b;
  --color-accent-600: #d97706;
  --color-accent-700: #b45309;
  --color-accent-800: #92400e;
  --color-accent-900: #78350f;
  --color-accent-950: #451a03;
}
```

Usage: Preview mode indicators, upgrade prompts, special badges.

### Semantic Colors

```css
@theme {
  --color-success-50: #f0fdf4;
  --color-success-500: #22c55e;
  --color-success-600: #16a34a;

  --color-warning-50: #fffbeb;
  --color-warning-500: #f59e0b;
  --color-warning-600: #d97706;

  --color-danger-50: #fef2f2;
  --color-danger-500: #ef4444;
  --color-danger-600: #dc2626;

  --color-info-50: #eff6ff;
  --color-info-500: #3b82f6;
  --color-info-600: #2563eb;
}
```

Usage: Success toasts (green), validation warnings (amber), error states (red), info banners (blue).

---

## Typography

Font family: **Inter** (Google Fonts) for UI and landing page preview rendering.

```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
```

```css
@theme {
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', monospace;
}
```

### Type Scale

| Token | Size | Weight | Line Height | Usage |
|-------|------|--------|-------------|-------|
| `--text-h1` | 3.75rem (60px) | 800 (ExtraBold) | 1.1 | Landing page hero titles |
| `--text-h2` | 2.25rem (36px) | 700 (Bold) | 1.2 | Section headings |
| `--text-h3` | 1.5rem (24px) | 600 (SemiBold) | 1.3 | Card titles, modal headers |
| `--text-h4` | 1.25rem (20px) | 600 (SemiBold) | 1.4 | Subsection titles |
| `--text-body-lg` | 1.125rem (18px) | 400 (Regular) | 1.6 | Hero subtitle, lead text |
| `--text-body` | 1rem (16px) | 400 (Regular) | 1.5 | Default body text |
| `--text-body-sm` | 0.875rem (14px) | 400 (Regular) | 1.5 | Secondary text, metadata |
| `--text-caption` | 0.75rem (12px) | 500 (Medium) | 1.4 | Labels, timestamps, badges |
| `--text-overline` | 0.75rem (12px) | 600 (SemiBold) | 1.0 | Section labels, uppercase |

Usage in Tailwind: `text-h1` through `text-caption`.

---

## Spacing

```css
@theme {
  --spacing-0\.5: 0.125rem;  /* 2px — icon/button tight padding */
  --spacing-1: 0.25rem;      /* 4px — tight gaps */
  --spacing-1\.5: 0.375rem;  /* 6px — badge padding */
  --spacing-2: 0.5rem;       /* 8px — form element padding */
  --spacing-3: 0.75rem;      /* 12px — button padding */
  --spacing-4: 1rem;         /* 16px — card padding */
  --spacing-5: 1.25rem;      /* 20px — section padding */
  --spacing-6: 1.5rem;       /* 24px — section gap */
  --spacing-8: 2rem;         /* 32px — page section gap */
  --spacing-10: 2.5rem;      /* 40px — hero padding */
  --spacing-12: 3rem;        /* 48px — large section gap */
  --spacing-16: 4rem;        /* 64px — page margins */
  --spacing-20: 5rem;        /* 80px — hero section padding */
  --spacing-24: 6rem;        /* 96px — full section spacing */
}
```

Usage guidelines:
- `p-2` to `p-4` for card/panel padding
- `gap-4` to `gap-6` for grid gaps
- `p-6` to `p-10` for landing page section padding
- `gap-2` to `gap-3` for form element spacing

---

## Shadows

```css
@theme {
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  --shadow-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);
  --shadow-card: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  --shadow-card-hover: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  --shadow-modal: 0 25px 50px -12px rgb(0 0 0 / 0.25);
  --shadow-dropdown: 0 4px 6px -1px rgb(0 0 0 / 0.08), 0 2px 4px -2px rgb(0 0 0 / 0.05);
}
```

| Token | Usage |
|-------|-------|
| `shadow-sm` | Subtle separation for input fields |
| `shadow-card` | Default project cards |
| `shadow-card-hover` | Card hover — elevated, interactive |
| `shadow-lg` | Dropdown menus, popovers |
| `shadow-xl` | Section panels in editor |
| `shadow-modal` | Modals, dialogs |

---

## Border Radius

```css
@theme {
  --radius-sm: 0.25rem;    /* 4px — badges, tags */
  --radius-md: 0.375rem;   /* 6px — input fields */
  --radius-lg: 0.5rem;     /* 8px — buttons, cards */
  --radius-xl: 0.75rem;    /* 12px — modals, panels */
  --radius-2xl: 1rem;      /* 16px — large cards, sections */
  --radius-full: 9999px;   /* pills, avatars */
}
```

| Token | Usage |
|-------|-------|
| `rounded-sm` | Status badges, tags |
| `rounded-md` | Inputs, selects |
| `rounded-lg` | Buttons, card containers |
| `rounded-xl` | Modals, section panels in editor |
| `rounded-2xl` | Hero sections, large feature cards |
| `rounded-full` | User avatars, pill badges |

---

## Dark Mode

Dark mode uses CSS variable swapping — apply `.dark` class to `<html>` to override secondary/surface colors while keeping primary and accent constant.

```css
@custom-variant dark (&:where(.dark, .dark *));

.dark {
  --color-secondary-50: #0f172a;   /* page bg */
  --color-secondary-100: #1e293b;  /* card bg */
  --color-secondary-200: #334155;  /* borders */
  --color-secondary-300: #475569;  /* dividers */
  --color-secondary-400: #64748b;  /* muted text */
  --color-secondary-500: #94a3b8;  /* secondary text */
  --color-secondary-600: #cbd5e1;  /* body text */
  --color-secondary-700: #e2e8f0;  /* headings */
  --color-secondary-800: #f1f5f9;  /* high emphasis */
  --color-secondary-900: #f8fafc;  /* highest emphasis */

  --color-primary-400: #a5b4fc;    /* brighter primary for dark bg */
  --color-primary-500: #818cf8;

  --color-accent-400: #fcd34d;
  --color-accent-500: #fbbf24;

  --shadow-card: 0 1px 3px 0 rgb(0 0 0 / 0.3);
  --shadow-card-hover: 0 10px 15px -3px rgb(0 0 0 / 0.4);
  --shadow-modal: 0 25px 50px -12px rgb(0 0 0 / 0.5);
}

/* Dark mode semantic colors */
.dark {
  --color-success-50: #052e16;
  --color-warning-50: #451a03;
  --color-danger-50: #450a0a;
  --color-info-50: #172554;
}
```

Usage: Add `class="dark"` to `<html>` when user toggles dark mode. Store preference in localStorage.

---

## Transitions & Animations

### Base Transition Properties

```css
* {
  transition-property: color, background-color, border-color, outline-color, text-decoration-color, fill, stroke, opacity, box-shadow, transform, filter, backdrop-filter;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 150ms;
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### Animation Keyframes

```css
@keyframes fade-in {
  from { opacity: 0; }
  to   { opacity: 1; }
}

@keyframes slide-up {
  from { opacity: 0; transform: translateY(16px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes slide-in-right {
  from { opacity: 0; transform: translateX(20px); }
  to   { opacity: 1; transform: translateX(0); }
}

@keyframes scale-in {
  from { opacity: 0; transform: scale(0.95); }
  to   { opacity: 1; transform: scale(1); }
}

@keyframes pulse-soft {
  0%, 100% { opacity: 1; }
  50%      { opacity: 0.7; }
}

@keyframes shimmer {
  0%   { background-position: -200% 0; }
  100% { background-position: 200% 0; }
}

@keyframes skeleton-pulse {
  0%, 100% { background-color: var(--color-secondary-200); }
  50%      { background-color: var(--color-secondary-300); }
}
```

```css
@theme {
  --animate-fade-in: fade-in 200ms ease-out;
  --animate-slide-up: slide-up 300ms ease-out;
  --animate-slide-in-right: slide-in-right 250ms ease-out;
  --animate-scale-in: scale-in 200ms ease-out;
  --animate-pulse-soft: pulse-soft 2s ease-in-out infinite;
  --animate-shimmer: shimmer 2s linear infinite;
  --animate-skeleton-pulse: skeleton-pulse 1.5s ease-in-out infinite;
}
```

### Element Animation Map

| Element | Animation | Duration | Trigger |
|---------|-----------|----------|---------|
| Card hover | transform: translateY(-2px) + shadow transition | 200ms | :hover |
| Modal open | `animate-scale-in` | 200ms | Open click |
| Modal close | Opacity fade-out | 150ms | Close action |
| Page transition | `animate-fade-in` | 200ms | Route change |
| Section add | `animate-slide-up` | 300ms | Add section |
| Section remove | Opacity + scale-out | 200ms | Delete action |
| Toast appear | `animate-slide-up` | 250ms | Toast trigger |
| Toast dismiss | Opacity + translate-out | 200ms | Auto/timeout |
| Skeleton loading | `animate-skeleton-pulse` | 1.5s infinite | Data loading |
| Button press | transform: scale(0.97) | 100ms | :active |
| Dropdown open | `animate-scale-in` + origin-top | 150ms | Toggle click |
| Dark mode toggle | background-color + color | 300ms | Toggle click |
| Save indicator | `animate-fade-in` | 200ms | Auto-save trigger |
| Preview pane load | `animate-fade-in` | 300ms | Content render |
| Section reorder | transform + transition | 200ms | Drag end |

---

## Tailwind v4 @theme Integration

The complete `@theme` block for `styles.css`:

```css
@import "tailwindcss";

@theme {
  /* Colors */
  --color-primary-50: #eef2ff;
  --color-primary-100: #e0e7ff;
  --color-primary-200: #c7d2fe;
  --color-primary-300: #a5b4fc;
  --color-primary-400: #818cf8;
  --color-primary-500: #6366f1;
  --color-primary-600: #4f46e5;
  --color-primary-700: #4338ca;
  --color-primary-800: #3730a3;
  --color-primary-900: #312e81;
  --color-primary-950: #1e1b4b;

  --color-secondary-50: #f8fafc;
  --color-secondary-100: #f1f5f9;
  --color-secondary-200: #e2e8f0;
  --color-secondary-300: #cbd5e1;
  --color-secondary-400: #94a3b8;
  --color-secondary-500: #64748b;
  --color-secondary-600: #475569;
  --color-secondary-700: #334155;
  --color-secondary-800: #1e293b;
  --color-secondary-900: #0f172a;
  --color-secondary-950: #020617;

  --color-accent-50: #fffbeb;
  --color-accent-100: #fef3c7;
  --color-accent-200: #fde68a;
  --color-accent-300: #fcd34d;
  --color-accent-400: #fbbf24;
  --color-accent-500: #f59e0b;
  --color-accent-600: #d97706;
  --color-accent-700: #b45309;
  --color-accent-800: #92400e;
  --color-accent-900: #78350f;
  --color-accent-950: #451a03;

  --color-success-50: #f0fdf4;
  --color-success-500: #22c55e;
  --color-success-600: #16a34a;
  --color-warning-50: #fffbeb;
  --color-warning-500: #f59e0b;
  --color-warning-600: #d97706;
  --color-danger-50: #fef2f2;
  --color-danger-500: #ef4444;
  --color-danger-600: #dc2626;
  --color-info-50: #eff6ff;
  --color-info-500: #3b82f6;
  --color-info-600: #2563eb;

  /* Typography */
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', monospace;

  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-card: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  --shadow-card-hover: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  --shadow-modal: 0 25px 50px -12px rgb(0 0 0 / 0.25);
  --shadow-dropdown: 0 4px 6px -1px rgb(0 0 0 / 0.08), 0 2px 4px -2px rgb(0 0 0 / 0.05);

  /* Radius */
  --radius-sm: 0.25rem;
  --radius-md: 0.375rem;
  --radius-lg: 0.5rem;
  --radius-xl: 0.75rem;
  --radius-2xl: 1rem;
  --radius-full: 9999px;

  /* Animations */
  --animate-fade-in: fade-in 200ms ease-out;
  --animate-slide-up: slide-up 300ms ease-out;
  --animate-slide-in-right: slide-in-right 250ms ease-out;
  --animate-scale-in: scale-in 200ms ease-out;
  --animate-pulse-soft: pulse-soft 2s ease-in-out infinite;
  --animate-shimmer: shimmer 2s linear infinite;
  --animate-skeleton-pulse: skeleton-pulse 1.5s ease-in-out infinite;
}

/* Dark mode */
@custom-variant dark (&:where(.dark, .dark *));

.dark {
  --color-secondary-50: #0f172a;
  --color-secondary-100: #1e293b;
  --color-secondary-200: #334155;
  --color-secondary-300: #475569;
  --color-secondary-400: #64748b;
  --color-secondary-500: #94a3b8;
  --color-secondary-600: #cbd5e1;
  --color-secondary-700: #e2e8f0;
  --color-secondary-800: #f1f5f9;
  --color-secondary-900: #f8fafc;
  --color-primary-400: #a5b4fc;
  --color-primary-500: #818cf8;
  --color-accent-400: #fcd34d;
  --color-accent-500: #fbbf24;
  --shadow-card: 0 1px 3px 0 rgb(0 0 0 / 0.3);
  --shadow-card-hover: 0 10px 15px -3px rgb(0 0 0 / 0.4);
  --shadow-modal: 0 25px 50px -12px rgb(0 0 0 / 0.5);
  --color-success-50: #052e16;
  --color-warning-50: #451a03;
  --color-danger-50: #450a0a;
  --color-info-50: #172554;
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

## Component Base Patterns

### Buttons

| Variant | Classes | Usage |
|---------|---------|-------|
| Primary | `bg-primary-500 text-white hover:bg-primary-600 active:bg-primary-700 rounded-lg px-5 py-2.5 font-medium transition` | Main actions, Save, Publish |
| Secondary | `bg-secondary-200 text-secondary-700 hover:bg-secondary-300 active:bg-secondary-400 rounded-lg px-5 py-2.5 font-medium transition` | Cancel, secondary actions |
| Outline | `border border-secondary-300 text-secondary-700 hover:bg-secondary-50 rounded-lg px-5 py-2.5 font-medium transition` | Preview, View |
| Ghost | `text-secondary-600 hover:bg-secondary-100 rounded-lg px-3 py-2 transition` | Icon buttons, inline actions |
| Danger | `bg-danger-500 text-white hover:bg-danger-600 rounded-lg px-5 py-2.5 font-medium transition` | Delete, destructive actions |
| Link | `text-primary-600 hover:text-primary-700 underline-offset-2 hover:underline font-medium` | Inline links |

### Cards

| Variant | Classes | Usage |
|---------|---------|-------|
| Default | `bg-secondary-50 dark:bg-secondary-100 rounded-xl shadow-card p-5 transition` | Project cards, section cards |
| Interactive | `hover:shadow-card-hover hover:-translate-y-0.5 cursor-pointer` | Clickable cards on dashboard |
| Selected | `ring-2 ring-primary-500` | Active selection in editor |
| Section in preview | `bg-white dark:bg-secondary-50 rounded-2xl shadow-sm` | Landing page section rendering |

### Input Fields

| Variant | Classes |
|---------|---------|
| Default | `bg-white dark:bg-secondary-100 border border-secondary-300 dark:border-secondary-200 rounded-lg px-3 py-2.5 text-secondary-800 placeholder:text-secondary-400 focus:ring-2 focus:ring-primary-500 focus:border-primary-500 transition outline-none` |
| Error | `border-danger-500 focus:ring-danger-500 focus:border-danger-500` |
| Disabled | `bg-secondary-100 text-secondary-400 cursor-not-allowed` |

### Modals

| Element | Classes |
|---------|---------|
| Overlay | `fixed inset-0 bg-black/50 z-50 animate-fade-in` |
| Container | `fixed inset-0 z-50 flex items-center justify-center p-4` |
| Dialog | `bg-white dark:bg-secondary-100 rounded-xl shadow-modal w-full max-w-lg p-6 animate-scale-in` |
| Title | `text-h3 text-secondary-800 dark:text-secondary-700 mb-2` |
| Actions | `flex justify-end gap-3 mt-6` |

### Toast / Alert

| Variant | Classes |
|---------|---------|
| Success | `bg-success-50 dark:bg-success-50 border border-success-500/20 text-success-600` |
| Error | `bg-danger-50 dark:bg-danger-50 border border-danger-500/20 text-danger-600` |
| Warning | `bg-warning-50 dark:bg-warning-50 border border-warning-500/20 text-warning-600` |
| Info | `bg-info-50 dark:bg-info-50 border border-info-500/20 text-info-600` |
| Container | `fixed bottom-4 right-4 z-50 animate-slide-up shadow-lg rounded-xl px-4 py-3 flex items-center gap-3` |

### Status Badges

| Variant | Classes | Usage |
|---------|---------|-------|
| Draft | `bg-secondary-200 text-secondary-600 rounded-full px-2.5 py-0.5 text-caption font-medium` | Project draft status |
| Published | `bg-success-50 text-success-600 dark:bg-success-50 dark:text-success-500 rounded-full px-2.5 py-0.5 text-caption font-medium` | Project published status |
| Preview | `bg-accent-50 text-accent-600 rounded-full px-2.5 py-0.5 text-caption font-medium` | Preview mode indicator |
| Saving | `bg-info-50 text-info-600 rounded-full px-2.5 py-0.5 text-caption font-medium animate-pulse-soft` | Auto-save indicator |
| Saved | `bg-success-50 text-success-600 rounded-full px-2.5 py-0.5 text-caption` | Save confirmation |
