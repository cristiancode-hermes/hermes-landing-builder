# Component Tree — Hermes Landing Builder

> Angular 22 component hierarchy with signal-based state management.

---

## Component Hierarchy

```
AppComponent
├── NavbarComponent
│   ├── LogoComponent
│   ├── SearchBarComponent
│   └── UserMenuComponent
│       ├── UserAvatarComponent
│       └── ThemeToggleComponent
│
├── RouterOutlet (lazy — app.routes.ts)
│   ├── LoginPageComponent
│   │   └── AuthFormComponent (email, password, submit, toggle register/login)
│   │
│   ├── RegisterPageComponent
│   │   └── AuthFormComponent (email, name, password, confirm, submit)
│   │
│   ├── DashboardPageComponent
│   │   ├── ProjectFiltersComponent (status tabs, search input)
│   │   ├── ProjectCardComponent (×N)
│   │   │   ├── ProjectThumbnailComponent
│   │   │   ├── StatusBadgeComponent
│   │   │   └── CardActionsDropdownComponent
│   │   ├── NewProjectModalComponent
│   │   │   ├── TemplateSelectorComponent
│   │   │   └── ProjectFormComponent (name, slug)
│   │   ├── EmptyStateComponent
│   │   ├── ErrorStateComponent
│   │   └── LoadingSkeletonComponent
│   │
│   ├── EditorPageComponent (main editor — split view)
│   │   ├── EditorHeaderComponent
│   │   │   ├── BackButtonComponent
│   │   │   ├── SaveIndicatorComponent
│   │   │   ├── ViewToggleComponent (desktop/mobile)
│   │   │   ├── PublishButtonComponent
│   │   │   └── PageUrlComponent (published only)
│   │   │
│   │   ├── EditorTabsComponent
│   │   │   ├── TabButtonComponent ("Secciones" | "SEO" | "Analytics")
│   │   │   └── ActiveTabContent
│   │   │
│   │   ├── [[Tab: Secciones]]
│   │   │   ├── SectionSidebarComponent
│   │   │   │   ├── AddSectionPanelComponent
│   │   │   │   │   └── SectionTypeButtonComponent (×N: Hero, Features, etc.)
│   │   │   │   └── SectionListComponent
│   │   │   │       └── SectionListItemComponent (×N — draggable)
│   │   │   │           ├── DragHandleComponent
│   │   │   │           ├── SectionTypeIconComponent
│   │   │   │           ├── SectionTitleComponent
│   │   │   │           └── SectionActionsComponent (edit, delete)
│   │   │   │
│   │   │   ├── PreviewPaneComponent
│   │   │   │   ├── PreviewFrameComponent
│   │   │   │   │   └── SectionRendererComponent (×N)
│   │   │   │   │       ├── HeroSectionComponent
│   │   │   │   │       ├── FeaturesSectionComponent
│   │   │   │   │       ├── PricingSectionComponent
│   │   │   │   │       ├── TestimonialsSectionComponent
│   │   │   │   │       ├── FaqSectionComponent
│   │   │   │   │       ├── CtaSectionComponent
│   │   │   │   │       └── FooterSectionComponent
│   │   │   │   └── PreviewEmptyStateComponent
│   │   │   │
│   │   │   └── SectionEditorPanelComponent (right panel)
│   │   │       ├── SectionFormComponent
│   │   │       │   ├── TextFieldComponent (×N — título, subtítulo, etc.)
│   │   │       │   ├── ColorPickerComponent (×N — fondo, texto, acento)
│   │   │       │   └── SelectFieldComponent (×N — align, padding)
│   │   │       └── ResetButtonComponent
│   │   │
│   │   ├── [[Tab: SEO]]
│   │   │   ├── GooglePreviewComponent
│   │   │   ├── SeoFormComponent
│   │   │   │   ├── SeoTextFieldComponent (×N — title, description, keywords)
│   │   │   │   ├── CharacterCounterComponent
│   │   │   │   └── OgFieldsComponent
│   │   │   │       ├── TextFieldComponent (og:title, og:description)
│   │   │   │       └── ImageUrlFieldComponent (og:image)
│   │   │   └── AutoSaveIndicatorComponent
│   │   │
│   │   ├── [[Tab: Analytics]]
│   │   │   ├── AnalyticsKpiCardsComponent
│   │   │   │   └── KpiCardComponent (×3 — total, today, unique days)
│   │   │   ├── VisitsChartComponent (SVG daily bar chart)
│   │   │   ├── RecentVisitsTableComponent
│   │   │   ├── AnalyticsEmptyStateComponent
│   │   │   ├── AnalyticsLoadingStateComponent
│   │   │   └── AnalyticsErrorStateComponent
│   │   │
│   │   └── ConfirmDialogComponent (delete, publish, unpublish)
│   │
│   ├── PublicPageComponent (public-facing, no auth)
│   │   └── PublicSectionRendererComponent (×N)
│   │       ├── PublicHeroSectionComponent
│   │       ├── PublicFeaturesSectionComponent
│   │       ├── PublicPricingSectionComponent
│   │       ├── PublicTestimonialsSectionComponent
│   │       ├── PublicFaqSectionComponent
│   │       ├── PublicCtaSectionComponent
│   │       └── PublicFooterSectionComponent
│   │
│   └── NotFoundPageComponent
│
├── ToastComponent (global — overlay)
└── SpinnerComponent (global — loading overlay)
```

---

## Shared Components Catalog

| Component | Inputs | Purpose |
|-----------|--------|---------|
| `SpinnerComponent` | size, color | Loading spinner for buttons and content areas |
| `LoadingSkeletonComponent` | type (card/line/chart), count | Skeleton loading placeholders |
| `EmptyStateComponent` | icon, title, message, actionLabel | Empty data state |
| `ErrorStateComponent` | message, retryFn | Error with retry button |
| `StatusBadgeComponent` | status (draft/published/saving/saved) | Status indicator pill |
| `ToastComponent` | (service-based) | Global toast notifications |
| `ConfirmDialogComponent` | title, message, confirmLabel, cancelLabel, onConfirm | Modal confirmation dialogs |
| `ThemeToggleComponent` | (toggles .dark class) | Dark mode switch |
| `CharacterCounterComponent` | current, max | Character counter with color coding |
| `DraggableListComponent` | items, onReorder | Reorderable list directive |

---

## Signal-Based State Services

### AuthService

```typescript
@Injectable({ providedIn: 'root' })
export class AuthService {
  private readonly http = inject(HttpClient);
  private readonly router = inject(Router);

  readonly user = signal<User | null>(null);
  readonly token = signal<string | null>(localStorage.getItem('token'));
  readonly isAuthenticated = computed(() => this.token() !== null);
  readonly loading = signal(false);
  readonly error = signal<string | null>(null);

  login(email: string, password: string): Observable<void> {
    this.loading.set(true);
    this.error.set(null);
    return this.http.post<{ access_token: string; user: User }>('/api/auth/login', { email, password }).pipe(
      tap(({ access_token, user }) => {
        localStorage.setItem('token', access_token);
        this.token.set(access_token);
        this.user.set(user);
        this.loading.set(false);
      }),
      catchError(err => {
        this.loading.set(false);
        this.error.set(err.error?.message || 'Error al iniciar sesión');
        return throwError(() => err);
      }),
      map(() => void 0)
    );
  }

  register(name: string, email: string, password: string): Observable<void> {
    this.loading.set(true);
    this.error.set(null);
    return this.http.post<{ access_token: string; user: User }>('/api/auth/register', { name, email, password }).pipe(
      tap(({ access_token, user }) => {
        localStorage.setItem('token', access_token);
        this.token.set(access_token);
        this.user.set(user);
        this.loading.set(false);
      }),
      catchError(err => {
        this.loading.set(false);
        this.error.set(err.error?.message || 'Error al registrarse');
        return throwError(() => err);
      }),
      map(() => void 0)
    );
  }

  logout(): void {
    localStorage.removeItem('token');
    this.token.set(null);
    this.user.set(null);
    this.router.navigate(['/login']);
  }
}
```

### ProjectService

```typescript
@Injectable({ providedIn: 'root' })
export class ProjectService {
  private readonly http = inject(HttpClient);

  readonly projects = signal<Project[]>([]);
  readonly loading = signal(false);
  readonly error = signal<string | null>(null);
  readonly selectedProject = signal<Project | null>(null);

  readonly isEmpty = computed(() => !this.loading() && this.projects().length === 0);

  loadProjects(): void {
    this.loading.set(true);
    this.error.set(null);
    this.http.get<Project[]>('/api/projects').subscribe({
      next: (projects) => { this.projects.set(projects); this.loading.set(false); },
      error: (err) => { this.error.set('Error al cargar proyectos'); this.loading.set(false); }
    });
  }

  createProject(data: CreateProjectDto): Observable<Project> {
    return this.http.post<Project>('/api/projects', data).pipe(
      tap(project => this.projects.update(p => [project, ...p]))
    );
  }

  updateProject(id: string, data: Partial<Project>): Observable<Project> {
    return this.http.put<Project>(`/api/projects/${id}`, data).pipe(
      tap(updated => {
        this.projects.update(p => p.map(proj => proj.id === id ? updated : proj));
        this.selectedProject.update(p => p?.id === id ? updated : p);
      })
    );
  }

  deleteProject(id: string): Observable<void> {
    return this.http.delete<void>(`/api/projects/${id}`).pipe(
      tap(() => this.projects.update(p => p.filter(proj => proj.id !== id)))
    );
  }

  publishProject(id: string): Observable<Project> {
    return this.http.patch<Project>(`/api/projects/${id}/publish`, {});
  }

  unpublishProject(id: string): Observable<Project> {
    return this.http.patch<Project>(`/api/projects/${id}/unpublish`, {});
  }
}
```

### SectionStore (Editor State)

```typescript
@Injectable()
export class SectionStore {
  private readonly http = inject(HttpClient);

  readonly sections = signal<Section[]>([]);
  readonly loading = signal(false);
  readonly error = signal<string | null>(null);
  readonly selectedSectionId = signal<string | null>(null);
  readonly previewDevice = signal<'desktop' | 'mobile'>('desktop');
  readonly saving = signal(false);
  readonly lastSaved = signal<Date | null>(null);

  readonly selectedSection = computed(() =>
    this.sections().find(s => s.id === this.selectedSectionId()) ?? null
  );

  readonly orderedSections = computed(() =>
    [...this.sections()].sort((a, b) => a.position - b.position)
  );

  readonly isEmpty = computed(() => !this.loading() && this.sections().length === 0);

  loadSections(projectId: string): void {
    this.loading.set(true);
    this.error.set(null);
    this.http.get<Section[]>(`/api/projects/${projectId}/sections`).subscribe({
      next: (sections) => { this.sections.set(sections); this.loading.set(false); },
      error: () => { this.error.set('Error al cargar secciones'); this.loading.set(false); }
    });
  }

  addSection(projectId: string, type: SectionType): void {
    this.http.post<Section>(`/api/sections`, { projectId, type }).subscribe({
      next: (section) => {
        section.position = this.sections().length;
        this.sections.update(s => [...s, section]);
      },
      error: () => this.error.set('Error al añadir sección')
    });
  }

  updateSection(sectionId: string, data: Partial<Section>): void {
    this.saving.set(true);
    this.http.put<Section>(`/api/sections/${sectionId}`, data).subscribe({
      next: (updated) => {
        this.sections.update(s => s.map(sec => sec.id === sectionId ? updated : sec));
        this.saving.set(false);
        this.lastSaved.set(new Date());
      },
      error: () => { this.saving.set(false); this.error.set('Error al guardar'); }
    });
  }

  reorderSections(projectId: string, orderedIds: string[]): void {
    // Optimistic update
    const oldOrder = [...this.sections()];
    this.sections.update(s =>
      s.map(sec => ({ ...sec, position: orderedIds.indexOf(sec.id) }))
    );

    this.http.post(`/api/sections/reorder`, { projectId, orderedIds }).subscribe({
      error: () => {
        this.sections.set(oldOrder); // Revert on error
        this.error.set('Error al reordenar');
      }
    });
  }

  removeSection(sectionId: string): void {
    this.http.delete(`/api/sections/${sectionId}`).subscribe({
      next: () => this.sections.update(s => s.filter(sec => sec.id !== sectionId)),
      error: () => this.error.set('Error al eliminar sección')
    });
  }

  selectSection(sectionId: string | null): void {
    this.selectedSectionId.set(sectionId);
  }

  toggleDevice(): void {
    this.previewDevice.update(d => d === 'desktop' ? 'mobile' : 'desktop');
  }
}
```

### SeoService

```typescript
@Injectable({ providedIn: 'root' })
export class SeoService {
  private readonly http = inject(HttpClient);
  private readonly debounceSave = new Subject<{ projectId: string; data: SeoData }>();

  readonly seoData = signal<SeoData | null>(null);
  readonly loading = signal(false);
  readonly saving = signal(false);
  readonly saved = signal(false);

  constructor() {
    // Debounced auto-save: 2s after last change
    this.debounceSave.pipe(debounceTime(2000)).subscribe(({ projectId, data }) => {
      this.saving.set(true);
      this.http.put(`/api/projects/${projectId}/seo`, data).subscribe({
        next: () => { this.saving.set(false); this.saved.set(true); },
        error: () => { this.saving.set(false); }
      });
    });
  }

  loadSeo(projectId: string): void {
    this.loading.set(true);
    this.http.get<SeoData>(`/api/projects/${projectId}/seo`).subscribe({
      next: (data) => { this.seoData.set(data); this.loading.set(false); },
      error: () => this.loading.set(false)
    });
  }

  updateSeo(projectId: string, data: Partial<SeoData>): void {
    const current = this.seoData();
    const merged = { ...current, ...data } as SeoData;
    this.seoData.set(merged);
    this.saved.set(false);
    this.debounceSave.next({ projectId, data: merged });
  }
}
```

### AnalyticsService

```typescript
@Injectable({ providedIn: 'root' })
export class AnalyticsService {
  private readonly http = inject(HttpClient);

  readonly analytics = signal<AnalyticsData | null>(null);
  readonly loading = signal(false);
  readonly error = signal<string | null>(null);
  readonly isPublished = signal(false);

  readonly totalVisits = computed(() => this.analytics()?.totalVisits ?? 0);
  readonly todayVisits = computed(() => this.analytics()?.todayVisits ?? 0);
  readonly dailyData = computed(() => this.analytics()?.daily ?? []);

  loadAnalytics(projectId: string): void {
    this.loading.set(true);
    this.error.set(null);
    this.http.get<AnalyticsData>(`/api/projects/${projectId}/analytics`).subscribe({
      next: (data) => { this.analytics.set(data); this.loading.set(false); },
      error: (err) => { this.error.set('Error al cargar analytics'); this.loading.set(false); }
    });
  }
}
```

---

## EditorPageComponent (Main State — integrated)

```typescript
@Component({
  selector: 'app-editor-page',
  standalone: true,
  imports: [EditorHeaderComponent, EditorTabsComponent, SectionSidebarComponent,
            PreviewPaneComponent, SectionEditorPanelComponent, SeoTabComponent,
            AnalyticsTabComponent, ConfirmDialogComponent, SpinnerComponent],
  template: `...`,
})
export class EditorPageComponent {
  private readonly route = inject(ActivatedRoute);
  readonly projectService = inject(ProjectService);
  readonly sectionStore = inject(SectionStore);
  readonly seoService = inject(SeoService);
  readonly analyticsService = inject(AnalyticsService);

  readonly projectId = signal<string>('');
  readonly activeTab = signal<'sections' | 'seo' | 'analytics'>('sections');

  projectId$ = this.route.paramMap.pipe(map(p => p.get('projectId')!));

  constructor() {
    effect(() => {
      const id = this.projectId();
      if (!id) return;
      this.sectionStore.loadSections(id);
      this.seoService.loadSeo(id);
      if (this.projectService.selectedProject()?.status === 'published') {
        this.analyticsService.loadAnalytics(id);
      }
    });
  }

  // Called when route params resolve
  ngOnInit() {
    this.route.paramMap.subscribe(params => {
      const id = params.get('projectId')!;
      this.projectId.set(id);
      this.projectService.selectedProject.set(
        this.projectService.projects().find(p => p.id === id) ?? null
      );
    });
  }

  setActiveTab(tab: 'sections' | 'seo' | 'analytics'): void {
    this.activeTab.set(tab);
    if (tab === 'analytics' && this.projectService.selectedProject()?.status === 'published') {
      this.analyticsService.loadAnalytics(this.projectId());
    }
  }

  // Cross-entity computed: reorder and group sections
  readonly sectionsByType = computed(() => {
    const groups = new Map<string, Section[]>();
    for (const section of this.sectionStore.orderedSections()) {
      const list = groups.get(section.type) ?? [];
      list.push(section);
      groups.set(section.type, list);
    }
    return groups;
  });
}
```

---

## Component State Matrix

| Component | Loading | Empty | Error | Normal | Edge Cases |
|-----------|---------|-------|-------|--------|------------|
| DashboardPage | Skeleton grid (6 cards) | Illustration + CTA | Error banner + retry | Project card grid | No search results → "no matches" state |
| EditorPage | Full-page skeleton | "Añade tu primera sección" | Toast on save fail | Split view editor | Offline → toast, save queue |
| SectionSidebar | Gray pills | "No hay secciones" + add CTA | N/A | List with drag handles | Single section → no drag needed |
| PreviewPane | Skeleton frame | Placeholder + add CTA | N/A | Rendered sections | Very long content → scroll, 15+ sections → perf |
| SectionEditorPanel | Spinner in panel | "Selecciona una sección" | Error on save | Form fields | Unsaved changes → "Reset" enabled |
| SeoTab | Form skeleton | Default empty values | Save error | Form + Google preview | Title >60 char → counter red, form lock |
| AnalyticsTab | KPI skeletons + chart skeleton | "Publica para ver analytics" | Error + retry | KPIs + chart + table | Zero visits → chart empty, table empty |
| PublicPage | Page skeleton | 404 | 500 error page | Full rendered page | Very long content → scroll |
