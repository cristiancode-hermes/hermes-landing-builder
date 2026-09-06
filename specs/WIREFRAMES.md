# Wireframes — Hermes Landing Builder

> 6 screens with ASCII wireframes and per-state documentation.

---

## 1. Login / Register

```
┌──────────────────────────────────────────────┐
│                                              │
│               🏗️ Landing Builder              │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │                                      │    │
│  │         Iniciar Sesión               │    │
│  │                                      │    │
│  │  ┌────────────────────────────────┐  │    │
│  │  │ Correo electrónico             │  │    │
│  │  │ usuario@ejemplo.com            │  │    │
│  │  └────────────────────────────────┘  │    │
│  │                                      │    │
│  │  ┌────────────────────────────────┐  │    │
│  │  │ Contraseña              👁️     │  │    │
│  │  │ •••••••••••                   │  │    │
│  │  └────────────────────────────────┘  │    │
│  │                                      │    │
│  │  [────── Iniciar Sesión ──────]      │    │
│  │                                      │    │
│  │  ¿No tienes cuenta? [Registrarse]   │    │
│  └──────────────────────────────────────┘    │
│                                              │
└──────────────────────────────────────────────┘
```

| State | Visual | Description |
|-------|--------|-------------|
| Normal | Clean form with email + password fields | Default rendering with placeholder text |
| Typing | Focus ring on active field, show/hide password toggle | Password field uses ••• with eye toggle |
| Validation Error | Red border on invalid field, error message below | "Email inválido" / "La contraseña debe tener 8+ caracteres" |
| Loading | Button shows spinner, fields disabled | "Iniciando sesión..." con spinner |
| API Error | Red alert banner above form | "Credenciales incorrectas. Intenta de nuevo." |
| Success | Redirect to dashboard | Smooth transition to /dashboard |

**Key interactions:** Tab between fields, Enter to submit, toggle password visibility, link to register.

---

## 2. Dashboard (Project List)

```
┌─────────────────────────────────────────────────────────┐
│ 🏗️ Landing Builder     [🔍 Buscar...]     👤 John Doe  │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  Tus Proyectos          [➕ Nuevo Proyecto]              │
│                                                         │
│  [Todos] [📝 Draft] [🌐 Publicados]                     │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │ Startup Pro  │  │ Portfolio V2 │  │ Evento 2026  │  │
│  │ 🌐 Published │  │ 📝 Draft     │  │ 🌐 Published │  │
│  │ 1.2K visits  │  │ —            │  │ 340 visits   │  │
│  │ Editado: hoy │  │ Editado: ayer│  │ Editado: lun │  │
│  │ [Editar] [·] │  │ [Editar] [·] │  │ [Editar] [·] │  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐                    │
│  │ SaaS Landing │  │ Mi Blog 🆕  │                    │
│  │ 📝 Draft     │  │ 📝 Draft     │                    │
│  │ —            │  │ —            │                    │
│  │ Editado: mar │  │ Editado: hoy │                    │
│  │ [Editar] [·] │  │ [Editar] [·] │                    │
│  └──────────────┘  └──────────────┘                    │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

| State | Visual | Description |
|-------|--------|-------------|
| Normal | Card grid with project info, status badges, visit counts | 4+ project cards with actions |
| Loading | 6 skeleton cards (gray rectangles with animated shimmer) | While projects fetch from API |
| Empty | Large illustration + message | "No tienes proyectos aún. ¡Crea tu primer landing page!" + CTA button |
| Error | Alert banner | "Error al cargar proyectos" + [Reintentar] button |
| Search results | Filtered card grid matching query | "3 resultados para 'startup'" |
| No search results | "No se encontraron proyectos para 'xxx'" | Clean empty state for search |

**Key interactions:** Click project card to edit, [·] menu → Delete/Archive, [➕ Nuevo Proyecto] → modal with template selection, search bar filters in real-time.

---

## 3. Project Editor (Main — Split View)

```
┌──────────────────────────────────────────────────────────────────────┐
│ 🏗️ Landing Builder  ← [Mis Proyectos]   [📝 Draft]  [💾 Guardado]  │
│                                                                      │
│  [📐 Secciones] [🔍 SEO] [📊 Analytics]   [👁️ Preview] [🌐 Publish] │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌─── Sidebar ───┐  ┌─────── Preview Pane ────────────────────────┐ │
│  │                │  │                                              │ │
│  │  Añadir sección│  │  ┌────────────────────────────────────────┐ │ │
│  │  [➕] (Hero)   │  │  │                                        │ │ │
│  │  [➕] Feat.    │  │  │  [Hero Section]                        │ │ │
│  │  [➕] Pricing  │  │  │  Build Amazing                        │ │ │
│  │  [➕] Testim.  │  │  │  Landing Pages                        │ │ │
│  │  [➕] FAQ      │  │  │  Without Code                         │ │ │
│  │  [➕] CTA      │  │  │                                        │ │ │
│  │  [➕] Footer   │  │  │  [Get Started Free →]                  │ │ │
│  │                │  │  │                                        │ │ │
│  │  ─── Orden ─── │  │  ├────────────────────────────────────────┤ │ │
│  │                │  │  │                                        │ │ │
│  │  ≡ 1. Hero     │  │  │  [Features Section]                   │ │ │
│  │  ≡ 2. Features │  │  │  ┌─────┐ ┌─────┐ ┌─────┐            │ │ │
│  │  ≡ 3. Pricing  │  │  │  │F1   │ │F2   │ │F3   │            │ │ │
│  │  ≡ 4. FAQ      │  │  │  │Fast │ │Easy │ │Flex │            │ │ │
│  │  ≡ 5. Footer   │  │  │  └─────┘ └─────┘ └─────┘            │ │ │
│  │                │  │  │                                        │ │ │
│  │  [🐾 Desktop]  │  │  ├────────────────────────────────────────┤ │ │
│  │  [📱 Mobile]   │  │  │  [Footer]                            │ │ │
│  │                │  │  │  © 2026 · Privacy · Terms             │ │ │
│  └────────────────┘  │  └────────────────────────────────────────┘ │ │
│                      │                                              │ │
└──────────────────────────────────────────────────────────────────────┘
```

| State | Visual | Description |
|-------|--------|-------------|
| Normal | Split view: sidebar (sections list) + preview pane | Sidebar shows section list with drag handles |
| Loading | Skeleton preview pane | While project sections load from API |
| Empty | Preview: "Añade tu primera sección" placeholder | No sections yet, show prompt |
| Section selected | Sidebar highlights active section, preview scrolls to it | Clicking section in sidebar scrolls preview |
| Mobile preview | Preview shrinks to 375px width with mobile chrome | "📱 Mobile" button toggles responsive view |
| Auto-saving | "💾 Guardando..." indicator in header | Debounced auto-save after section changes |
| Error | Toast: "Error al guardar. Reintentando..." | Network error during save |

**Key interactions:** Click section in sidebar to edit, drag sections to reorder (with optimistic UI), click "➕" to add new section, toggle desktop/mobile preview, click "🌐 Publish" to publish.

---

## 4. Section Editor (Right Panel)

```
┌──────────────────────────────────────────────┐
│ ← Volver al editor              [🔄 Reset]  │
│                                              │
│  Editar: Hero Section                        │
│  ───────────────────────                     │
│                                              │
│  Contenido                                   │
│  ┌────────────────────────────────────────┐  │
│  │ Título                                  │  │
│  │ Build Amazing Landing Pages             │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │ Subtítulo                               │  │
│  │ Create beautiful pages without coding   │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ┌────────────────────────────────────────┐  │
│  │ Texto del botón                         │  │
│  │ Get Started Free →                      │  │
│  └────────────────────────────────────────┘  │
│                                              │
│  ─── Estilos ───                             │
│                                              │
│  Color de fondo    [🟣 #6366f1]              │
│  Color de texto    [#ffffff   ]              │
│  Color acento      [#f59e0b   ]              │
│                                              │
│  Alineación:  [◄ Centro ►]                   │
│  Padding:     [Mediano    ▼]                 │
│                                              │
│  ─── Imagen de fondo ───                     │
│  [Subir imagen]  o  [URL...]                 │
│                                              │
└──────────────────────────────────────────────┘
```

| State | Visual | Description |
|-------|--------|-------------|
| Normal | Form with text fields, color pickers, selects | Editing a section's content and styles |
| Dirty | "🔄 Reset" button enabled, "Guardando..." indicator | Unsaved changes detected |
| Saving | "💾 Guardando..." with subtle animation | Debounced 1s auto-save to server |
| Color picker open | Native color input or custom palette | Clicking color field opens system color picker |
| Image selected | Thumbnail preview with remove button | Section with background image |

**Key interactions:** Edit text fields (auto-save on blur), color pickers change preview immediately, reset restores defaults, scrollable panel for long forms.

---

## 5. SEO Settings Screen

```
┌─────────────────────────────────────────────────────┐
│ ← Volver al editor                  [💾 Guardado]   │
│                                                     │
│  SEO y Redes Sociales                               │
│  ───────────────────────                            │
│                                                     │
│  ─── Google Search Preview ───                      │
│  ┌─────────────────────────────────────────────┐    │
│  │ 🔗 landingbuilder.app/p/mi-startup          │    │
│  │ 🏷️ Mi Startup — Landing Page Builder        │    │
│  │ 📝 Descubre cómo lanzamos nuestro producto...│    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │ Título SEO (42/60)                          │    │
│  │ Mi Startup — Landing Page Builder           │    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │ Meta Descripción (98/160)                   │    │
│  │ Descubre cómo lanzamos nuestro nuevo produ...│    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │ Palabras Clave (separadas por coma)         │    │
│  │ startup, landing page, producto digital     │    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
│  ─── Open Graph ───                                 │
│  ┌─────────────────────────────────────────────┐    │
│  │ OG Title                                     │    │
│  │ Mi Startup                                   │    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │ OG Description                               │    │
│  │ Descubre cómo lanzamos...                    │    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │ OG Image URL                                 │    │
│  │ https://ejemplo.com/og-image.jpg             │    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
└─────────────────────────────────────────────────────┘
```

| State | Visual | Description |
|-------|--------|-------------|
| Normal | Form fields + Google Search preview | All SEO fields editable |
| Typing | Character counter updates (42/60) | Real-time counter with color (green ← → red) |
| Over limit | Counter turns red, field border red | Title >60 or description >160 chars |
| Auto-save | "💾 Guardando..." during debounce | Auto-saves 2s after last keystroke |
| Saved | "💾 Guardado" check mark | Confirmation visual |

**Key interactions:** Type in fields, Google preview updates in real-time, character counters with color coding.

---

## 6. Public Landing Page View

```
┌─────────────────────────────────────────────────────┐
│  🔗 landingbuilder.app/p/mi-startup                 │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │                                             │    │
│  │        Build Amazing Landing Pages           │    │
│  │      Create without code. Launch fast.      │    │
│  │                                             │    │
│  │         [Get Started Free →]                │    │
│  │                                             │    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐         │
│  │ 🚀 Fast  │  │ 🎨 Easy  │  │ 📱 Resp. │         │
│  │ Ships in │  │ Drag &   │  │ Works on │         │
│  │ minutes  │  │ drop     │  │ all devs │         │
│  └──────────┘  └──────────┘  └──────────┘         │
│                                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │          Pricing Plans                       │    │
│  │  ┌──────┐  ┌──────┐  ┌──────┐              │    │
│  │  │ Free │  │ Pro  │  │Enterprise│          │    │
│  │  │ $0   │  │ $29  │  │  $99    │          │    │
│  │  │ [→]  │  │ [→]  │  │  [→]    │          │    │
│  │  └──────┘  └──────┘  └──────┘              │    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │  "Amazing product!" — Happy Customer        │    │
│  │  "Super easy to use!" — Another User        │    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
│  ┌─────────────────────────────────────────────┐    │
│  │  Ready to start?                            │    │
│  │  [Start Free Trial]                         │    │
│  └─────────────────────────────────────────────┘    │
│                                                     │
│  © 2026 · Privacy · Terms                           │
└─────────────────────────────────────────────────────┘
```

| State | Visual | Description |
|-------|--------|-------------|
| Normal | Full styled landing page with all sections | Public page rendered from JSONB components |
| Loading | Skeleton/page placeholder | While server renders initial HTML |
| Not found | 404 page | "Esta página no existe" + link to home |
| Draft (owner) | Banner: "📝 Vista previa — Esta página no está publicada" | Owner viewing unpublished draft |
| Mobile | Responsive stacked layout | All sections collapse to single column |

**Key interactions:** Scroll through sections, click CTA buttons (configurable links), view on any device.
