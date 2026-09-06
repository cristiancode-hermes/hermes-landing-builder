# Routes — Hermes Landing Builder

> Frontend routes (Angular lazy) and backend API endpoints (NestJS modules).

---

## Frontend Routes (Angular 22 — Lazy Loading)

| Path | Component | Auth Guard | Lazy Chunk | Purpose |
|------|-----------|------------|------------|---------|
| `/login` | `LoginPageComponent` | No (redirect if authed) | `login` | User login |
| `/register` | `RegisterPageComponent` | No (redirect if authed) | `register` | User registration |
| `/dashboard` | `DashboardPageComponent` | Yes (`AuthGuard`) | `dashboard` | Project list + management |
| `/editor/:projectId` | `EditorPageComponent` | Yes (`AuthGuard`) | `editor` | Section editor + preview + SEO + analytics |
| `/p/:slug` | `PublicPageComponent` | No | `public` | Public landing page view |
| `**` | `NotFoundPageComponent` | No | `not-found` | 404 catch-all |

### Route Configuration

```typescript
export const routes: Routes = [
  { path: 'login', loadComponent: () => import('./login/login-page.component').then(m => m.LoginPageComponent) },
  { path: 'register', loadComponent: () => import('./register/register-page.component').then(m => m.RegisterPageComponent) },
  { path: 'dashboard', loadComponent: () => import('./dashboard/dashboard-page.component').then(m => m.DashboardPageComponent),
    canActivate: [() => inject(AuthService).isAuthenticated()] },
  { path: 'editor/:projectId', loadComponent: () => import('./editor/editor-page.component').then(m => m.EditorPageComponent),
    canActivate: [() => inject(AuthService).isAuthenticated()] },
  { path: 'p/:slug', loadComponent: () => import('./public/public-page.component').then(m => m.PublicPageComponent) },
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: '**', loadComponent: () => import('./not-found/not-found-page.component').then(m => m.NotFoundPageComponent) },
];
```

### Auth Guard Pattern (functional guard)

```typescript
export const authGuard: CanActivateFn = () => {
  const auth = inject(AuthService);
  const router = inject(Router);
  if (auth.isAuthenticated()) return true;
  router.navigate(['/login']);
  return false;
};
```

---

## API Endpoints (NestJS Modules)

**Backend port:** `{BACKEND_PORT}` (assigned by qa-tester-deployer from 3001–3029)

**Base URL:** `http://localhost:{BACKEND_PORT}/api`

### Module Structure

```
src/
├── auth/
│   ├── auth.module.ts
│   ├── auth.controller.ts
│   ├── auth.service.ts
│   ├── jwt-auth.guard.ts
│   ├── current-user.decorator.ts
│   └── dto/
│       ├── login.dto.ts
│       └── register.dto.ts
├── users/
│   ├── users.module.ts
│   ├── users.service.ts
│   └── entities/
│       └── user.entity.ts
├── projects/
│   ├── projects.module.ts
│   ├── projects.controller.ts
│   ├── projects.service.ts
│   ├── dto/
│   │   ├── create-project.dto.ts
│   │   └── update-project.dto.ts
│   └── entities/
│       └── project.entity.ts
├── sections/
│   ├── sections.module.ts
│   ├── sections.controller.ts
│   ├── sections.service.ts
│   ├── dto/
│   │   ├── create-section.dto.ts
│   │   ├── update-section.dto.ts
│   │   └── reorder-sections.dto.ts
│   └── entities/
│       └── section.entity.ts
├── templates/
│   ├── templates.module.ts
│   ├── templates.controller.ts
│   ├── templates.service.ts
│   └── entities/
│       └── template.entity.ts
├── analytics/
│   ├── analytics.module.ts
│   ├── analytics.controller.ts
│   ├── analytics.service.ts
│   ├── dto/
│   │   └── track-event.dto.ts
│   └── entities/
│       └── analytics-event.entity.ts
├── public/
│   ├── public.module.ts
│   ├── public.controller.ts
│   ├── public.service.ts
│   └── dto/
│       └── page-response.dto.ts
├── seed/
│   └── seed.module.ts
│       └── seed.service.ts
├── app.module.ts
└── main.ts
```

### Auth Module

| Method | Path | Auth | Body | Response | Description |
|--------|------|------|------|----------|-------------|
| `POST` | `/api/auth/register` | No | `{ name, email, password }` | `{ access_token, user }` | Register new user |
| `POST` | `/api/auth/login` | No | `{ email, password }` | `{ access_token, user }` | Login existing user |
| `GET` | `/api/auth/profile` | JWT | — | `User` | Get current user profile |

**POST /api/auth/register**
```json
// Request
{ "name": "Juan Pérez", "email": "juan@ejemplo.com", "password": "Password123!" }
// Response 201
{ "access_token": "eyJhbGciOiJIUzI1NiIs...", "user": { "id": "usr-1", "name": "Juan Pérez", "email": "juan@ejemplo.com" } }
// Response 409
{ "message": "El email ya está registrado", "statusCode": 409 }
```

### Projects Module

| Method | Path | Auth | Body | Response | Description |
|--------|------|------|------|----------|-------------|
| `GET` | `/api/projects` | JWT | — | `Project[]` | List user's projects |
| `POST` | `/api/projects` | JWT | `{ name, slug?, templateId? }` | `Project` | Create new project |
| `GET` | `/api/projects/:id` | JWT | — | `Project` | Get project details |
| `PUT` | `/api/projects/:id` | JWT | `{ name?, slug? }` | `Project` | Update project |
| `DELETE` | `/api/projects/:id` | JWT | — | `204` | Delete project |
| `PATCH` | `/api/projects/:id/publish` | JWT | — | `Project` | Publish project (draft→published) |
| `PATCH` | `/api/projects/:id/unpublish` | JWT | — | `Project` | Unpublish project (published→draft) |
| `GET` | `/api/projects/:id/seo` | JWT | — | `SeoData` | Get SEO settings |
| `PUT` | `/api/projects/:id/seo` | JWT | `SeoData` | `SeoData` | Update SEO settings |

**POST /api/projects**
```json
// Request
{ "name": "Mi Startup", "slug": "mi-startup", "templateId": "tpl-startup" }
// Response 201
{
  "id": "proj-abc123",
  "name": "Mi Startup",
  "slug": "mi-startup",
  "status": "draft",
  "userId": "usr-1",
  "createdAt": "2026-07-15T02:30:00Z",
  "updatedAt": "2026-07-15T02:30:00Z"
}
// Response 400
{ "message": ["El nombre es requerido"], "statusCode": 400 }
```

### Sections Module

| Method | Path | Auth | Body | Response | Description |
|--------|------|------|------|----------|-------------|
| `GET` | `/api/projects/:projectId/sections` | JWT | — | `Section[]` | List project sections (ordered) |
| `POST` | `/api/sections` | JWT | `{ projectId, type }` | `Section` | Create new section |
| `PUT` | `/api/sections/:id` | JWT | `{ title?, componentData?, styles? }` | `Section` | Update section content |
| `DELETE` | `/api/sections/:id` | JWT | — | `204` | Delete section |
| `POST` | `/api/sections/reorder` | JWT | `{ projectId, orderedIds }` | `Section[]` | Reorder sections |

**POST /api/sections**
```json
// Request
{ "projectId": "proj-abc123", "type": "hero" }
// Response 201
{
  "id": "sec-def456",
  "projectId": "proj-abc123",
  "type": "hero",
  "position": 0,
  "title": "Hero",
  "componentData": {
    "title": "Build Amazing Landing Pages",
    "subtitle": "Create without code. Launch fast.",
    "buttonText": "Get Started Free →",
    "buttonUrl": "#"
  },
  "styles": {
    "backgroundColor": "#6366f1",
    "textColor": "#ffffff",
    "accentColor": "#f59e0b",
    "alignment": "center",
    "padding": "large"
  }
}
```

**POST /api/sections/reorder**
```json
// Request
{ "projectId": "proj-abc123", "orderedIds": ["sec-def456", "sec-ghi789", "sec-jkl012"] }
// Response 200
[
  { "id": "sec-def456", "position": 0, ... },
  { "id": "sec-ghi789", "position": 1, ... },
  { "id": "sec-jkl012", "position": 2, ... }
]
```

### Templates Module

| Method | Path | Auth | Body | Response | Description |
|--------|------|------|------|----------|-------------|
| `GET` | `/api/templates` | JWT | — | `Template[]` | List available templates |
| `GET` | `/api/templates/:id` | JWT | — | `Template` | Get template details with sections |

### Analytics Module

| Method | Path | Auth | Body | Response | Description |
|--------|------|------|------|----------|-------------|
| `GET` | `/api/projects/:id/analytics` | JWT | — | `AnalyticsData` | Get project analytics |
| `POST` | `/api/analytics/track` | No | `{ projectId }` | `204` | Track a page view (from public page) |

**GET /api/projects/:id/analytics**
```json
// Response 200
{
  "totalVisits": 1247,
  "todayVisits": 23,
  "daily": [
    { "date": "2026-07-09", "visits": 180 },
    { "date": "2026-07-10", "visits": 210 },
    { "date": "2026-07-11", "visits": 145 },
    { "date": "2026-07-12", "visits": 198 },
    { "date": "2026-07-13", "visits": 234 },
    { "date": "2026-07-14", "visits": 257 },
    { "date": "2026-07-15", "visits": 23 }
  ],
  "recentVisits": [
    { "timestamp": "2026-07-15T10:30:00Z" },
    { "timestamp": "2026-07-15T10:15:00Z" }
  ]
}
```

### Public Module (no auth)

| Method | Path | Auth | Body | Response | Description |
|--------|------|------|------|----------|-------------|
| `GET` | `/api/p/:slug` | No | — | `PageResponse` | Get published page data by slug |

**GET /api/p/:slug**
```json
// Response 200
{
  "project": {
    "id": "proj-abc123",
    "name": "Mi Startup",
    "slug": "mi-startup"
  },
  "seo": {
    "title": "Mi Startup — Landing Page Builder",
    "description": "Descubre cómo lanzamos nuestro nuevo producto...",
    "ogTitle": "Mi Startup",
    "ogDescription": "Descubre cómo lanzamos...",
    "ogImage": "https://ejemplo.com/og-image.jpg"
  },
  "sections": [
    {
      "type": "hero",
      "position": 0,
      "componentData": { "title": "Build Amazing Landing Pages", ... },
      "styles": { "backgroundColor": "#6366f1", ... }
    }
  ]
}
// Response 404
{ "message": "Página no encontrada", "statusCode": 404 }
// Response 404 (not published)
{ "message": "Página no disponible", "statusCode": 404 }
```
