# Database Schema — Hermes Landing Builder

> 6 entities with TypeORM decorators, JSONB flexible schema, PostgreSQL-compatible design.

---

## ERD (ASCII)

```
┌─────────────┐       ┌─────────────────┐
│    User     │       │    Project      │
│─────────────│       │─────────────────│
│ id (PK)     │──┐    │ id (PK)         │
│ name        │  │    │ userId (FK)     │────┐
│ email (UQ)  │  └───>│ name            │    │
│ password    │       │ slug (UQ)       │    │
│ createdAt   │       │ status          │    │
│ updatedAt   │       │ viewCount       │    │
└─────────────┘       │ templateId      │    │
                      │ createdAt       │    │
                      │ updatedAt       │    │
                      └────────┬────────┘    │
                               │             │
                      ┌────────┴────────┐    │
                      │    Section      │    │
                      │────────────────│    │
                      │ id (PK)        │    │
                      │ projectId (FK) │<───┘
                      │ type           │
                      │ position       │
                      │ title          │
                      │ componentData  │ (JSONB)
                      │ styles         │ (JSONB)
                      │ createdAt      │
                      │ updatedAt      │
                      └────────────────┘

┌──────────────────┐    ┌────────────────────┐
│    Template      │    │  AnalyticsEvent    │
│──────────────────│    │────────────────────│
│ id (PK)          │    │ id (PK)            │
│ name             │    │ projectId (FK)     │────> Project.id
│ description      │    │ timestamp          │
│ type             │    │ source             │
│ thumbnailUrl     │    │ userAgent          │
│ sectionConfigs   │    │ ipAddress          │
│                  │    │ createdAt          │
│ createdAt        │    └────────────────────┘
│ updatedAt        │
└──────────────────┘
```

### Relationships

| Entity | Relation | Target | Cascade | On Delete |
|--------|----------|--------|---------|-----------|
| User → Project | OneToMany | Project.userId | No | CASCADE |
| Project → Section | OneToMany | Section.projectId | Yes | CASCADE |
| Project → AnalyticsEvent | OneToMany | AnalyticsEvent.projectId | Yes | CASCADE |
| Template | Standalone | — | — | — |

### Entity TypeORM Definitions

#### User Entity

```typescript
import { Entity, PrimaryGeneratedColumn, Column, CreateDateColumn, UpdateDateColumn, OneToMany } from 'typeorm';
import { Project } from './project.entity';

@Entity('users')
export class User {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ type: 'text' })
  name: string;

  @Column({ type: 'text', unique: true })
  email: string;

  @Column({ type: 'text' })
  password: string; // bcryptjs hash

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;

  @OneToMany(() => Project, project => project.user)
  projects: Project[];
}
```

#### Project Entity

```typescript
import { Entity, PrimaryGeneratedColumn, Column, CreateDateColumn, UpdateDateColumn, ManyToOne, OneToMany, JoinColumn } from 'typeorm';
import { User } from './user.entity';
import { Section } from './section.entity';
import { AnalyticsEvent } from './analytics-event.entity';

@Entity('projects')
export class Project {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ type: 'text' })
  userId: string;

  @ManyToOne(() => User, user => user.projects)
  @JoinColumn({ name: 'userId' })
  user: User;

  @Column({ type: 'text' })
  name: string;

  @Column({ type: 'text', unique: true })
  slug: string;

  @Column({ type: 'text', default: 'draft' }) // 'draft' | 'published'
  status: string;

  @Column({ type: 'integer', default: 0 })
  viewCount: number;

  @Column({ type: 'text', nullable: true })
  templateId: string | null;

  // SEO fields
  @Column({ type: 'text', nullable: true })
  seoTitle: string | null;

  @Column({ type: 'text', nullable: true })
  seoDescription: string | null;

  @Column({ type: 'text', nullable: true })
  seoKeywords: string | null;

  @Column({ type: 'text', nullable: true })
  ogTitle: string | null;

  @Column({ type: 'text', nullable: true })
  ogDescription: string | null;

  @Column({ type: 'text', nullable: true })
  ogImage: string | null;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;

  @OneToMany(() => Section, section => section.project, { cascade: true })
  sections: Section[];

  @OneToMany(() => AnalyticsEvent, event => event.project, { cascade: true })
  analyticsEvents: AnalyticsEvent[];
}
```

#### Section Entity

```typescript
import { Entity, PrimaryGeneratedColumn, Column, CreateDateColumn, UpdateDateColumn, ManyToOne, JoinColumn } from 'typeorm';
import { Project } from './project.entity';

export type SectionType = 'hero' | 'features' | 'pricing' | 'testimonials' | 'faq' | 'cta' | 'footer';

@Entity('sections')
export class Section {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ type: 'text' })
  projectId: string;

  @ManyToOne(() => Project, project => project.sections, { onDelete: 'CASCADE' })
  @JoinColumn({ name: 'projectId' })
  project: Project;

  @Column({ type: 'text' })
  type: SectionType;

  @Column({ type: 'integer', default: 0 })
  position: number;

  @Column({ type: 'text', default: '' })
  title: string;

  // JSONB: Flexible content schema per section type
  // Hero: { title, subtitle, buttonText, buttonUrl, backgroundImage? }
  // Features: { items: [{ icon, title, description }], columns: 2|3 }
  // Pricing: { plans: [{ name, price, features, cta }] }
  // Testimonials: { items: [{ quote, author, role, avatar? }] }
  // FAQ: { items: [{ question, answer }] }
  // CTA: { title, subtitle, buttonText, buttonUrl }
  // Footer: { copyright, links: [{ text, url }] }
  @Column({ type: 'jsonb', default: {} })
  componentData: Record<string, any>;

  // { backgroundColor, textColor, accentColor, alignment, padding, backgroundImage? }
  @Column({ type: 'jsonb', default: {} })
  styles: Record<string, any>;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;
}
```

#### Template Entity

```typescript
import { Entity, PrimaryGeneratedColumn, Column, CreateDateColumn, UpdateDateColumn } from 'typeorm';

@Entity('templates')
export class Template {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ type: 'text' })
  name: string; // "Startup", "Portfolio", "SaaS", "Event", "Blank"

  @Column({ type: 'text' })
  description: string;

  @Column({ type: 'text' })
  type: string; // 'business' | 'portfolio' | 'saas' | 'event' | 'blank'

  @Column({ type: 'text', nullable: true })
  thumbnailUrl: string | null;

  // Array of section configs defining the template's structure:
  // [{ type: 'hero', title: 'Hero', componentData: {...}, styles: {...} }, ...]
  @Column({ type: 'jsonb', default: [] })
  sectionConfigs: Array<{
    type: SectionType;
    title: string;
    componentData: Record<string, any>;
    styles: Record<string, any>;
  }>;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;
}
```

#### AnalyticsEvent Entity

```typescript
import { Entity, PrimaryGeneratedColumn, Column, CreateDateColumn, ManyToOne, JoinColumn } from 'typeorm';
import { Project } from './project.entity';

@Entity('analytics_events')
export class AnalyticsEvent {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ type: 'text' })
  projectId: string;

  @ManyToOne(() => Project, project => project.analyticsEvents, { onDelete: 'CASCADE' })
  @JoinColumn({ name: 'projectId' })
  project: Project;

  @Column({ type: 'text', default: 'page_view' })
  source: string;

  @Column({ type: 'text', nullable: true })
  userAgent: string | null;

  @Column({ type: 'text', nullable: true })
  ipAddress: string | null;

  @CreateDateColumn()
  createdAt: Date;
}
```

---

## Key SQL Queries

### 1. Dashboard: Project count by status

```sql
SELECT
  status,
  COUNT(*)::integer AS count
FROM projects
WHERE "userId" = $1
GROUP BY status
ORDER BY status;
```

### 2. Dashboard: Recent projects (ordered by activity)

```sql
SELECT
  p.id, p.name, p.slug, p.status, p."viewCount",
  p.updatedAt,
  (SELECT COUNT(*)::integer FROM sections s WHERE s."projectId" = p.id) AS sectionCount
FROM projects p
WHERE p."userId" = $1
ORDER BY p.updatedAt DESC
LIMIT 50;
```

### 3. Section ordering by position

```sql
SELECT id, type, title, position, "componentData", styles
FROM sections
WHERE "projectId" = $1
ORDER BY position ASC;
```

### 4. Analytics: Total visits per project

```sql
SELECT
  p.id,
  p.name,
  p.slug,
  p."viewCount" AS totalVisits,
  COALESCE(SUM(CASE WHEN ae.createdAt >= date('now', '-30 days') THEN 1 ELSE 0 END), 0)::integer AS visits30d
FROM projects p
LEFT JOIN analytics_events ae ON ae."projectId" = p.id
WHERE p.id = $1
GROUP BY p.id;
```

### 5. Analytics: Daily visits (chart data)

```sql
-- SQLite version (strftime)
SELECT
  strftime('%Y-%m-%d', ae.createdAt) AS date,
  COUNT(*)::integer AS visits
FROM analytics_events ae
WHERE ae."projectId" = $1
  AND ae.createdAt >= date('now', '-7 days')
GROUP BY strftime('%Y-%m-%d', ae.createdAt)
ORDER BY date ASC;

-- PostgreSQL version (EXTRACT/DATE_TRUNC)
SELECT
  TO_CHAR(ae.createdAt, 'YYYY-MM-DD') AS date,
  COUNT(*)::integer AS visits
FROM analytics_events ae
WHERE ae."projectId" = $1
  AND ae.createdAt >= NOW() - INTERVAL '7 days'
GROUP BY TO_CHAR(ae.createdAt, 'YYYY-MM-DD')
ORDER BY date ASC;
```

### 6. Analytics: Recent visits

```sql
SELECT
  ae.id, ae.createdAt AS timestamp, ae.source
FROM analytics_events ae
WHERE ae."projectId" = $1
ORDER BY ae.createdAt DESC
LIMIT 50;
```

### 7. Slug uniqueness check

```sql
SELECT COUNT(*)::integer AS count
FROM projects
WHERE slug = $1 AND ("userId" != $2 OR $2 IS NULL);
```

### 8. Publish/unpublish status update

```sql
UPDATE projects
SET status = $2, "updatedAt" = CURRENT_TIMESTAMP
WHERE id = $1 AND ("userId" = $3 OR $3 IS NULL);
```

---

## Position Recalculation (Reorder)

```typescript
// Called from SectionService.reorder()
async reorderSections(projectId: string, orderedIds: string[]): Promise<Section[]> {
  const queryRunner = this.dataSource.createQueryRunner();
  await queryRunner.connect();
  await queryRunner.startTransaction();

  try {
    // Batch update positions
    for (let i = 0; i < orderedIds.length; i++) {
      await queryRunner.manager.update(Section, orderedIds[i], {
        position: i,
      });
    }

    await queryRunner.commitTransaction();

    return this.repo.find({
      where: { projectId },
      order: { position: 'ASC' },
    });
  } catch (err) {
    await queryRunner.rollbackTransaction();
    throw err;
  } finally {
    await queryRunner.release();
  }
}
```

---

## Publishing Status Machine

```typescript
const VALID_TRANSITIONS: Record<string, string[]> = {
  draft: ['published'],
  published: ['draft'],
};

function transitionProject(project: Project, newStatus: string): Project {
  const allowed = VALID_TRANSITIONS[project.status];
  if (!allowed?.includes(newStatus)) {
    throw new BadRequestException(
      `Cannot transition from ${project.status} to ${newStatus}. Allowed: ${allowed?.join(', ') || 'none'}`
    );
  }
  project.status = newStatus;
  if (newStatus === 'published') {
    project.viewCount = 0; // Reset view tracking for published state
  }
  return project;
}
```

---

## Neon/PostgreSQL Compatibility Notes

### SQLite vs PostgreSQL differences

| Feature | SQLite | PostgreSQL |
|---------|--------|------------|
| JSON column | `jsonb` → stored as TEXT | `jsonb` → native binary JSON |
| Auto-increment | `@PrimaryGeneratedColumn('uuid')` → TEXT UUID | Same — portable |
| Date functions | `strftime('%Y-%m-%d', date)` | `TO_CHAR(date, 'YYYY-MM-DD')` or `DATE_TRUNC('day', date)` |
| CURRENT_TIMESTAMP | `CURRENT_TIMESTAMP` → TEXT | `NOW()` → TIMESTAMPTZ |
| COALESCE | `COALESCE(SUM(x), 0)` | Same — portable |
| TypeORM relations | `{ relations: { user: true } }` | Same |
| TypeORM save() | `as unknown as T` cast | Same |

### Migration Strategy

- **Dev**: SQLite via `better-sqlite3` (fast, zero-config)
- **Prod**: PostgreSQL via `pg` driver with Neon connection string
- TypeORM `synchronize: true` for dev, migrations for prod
- All queries use portable SQL patterns (COALESCE, standard JOINs)
- JSONB columns work natively in both SQLite (as TEXT) and PostgreSQL (as JSONB)
- The `strftime` queries are isolated in the Analytics service — swap implementation when switching to PostgreSQL

### JSONB Schema per Section Type

```typescript
// componentData (type-specific)
type HeroData = {
  title: string;
  subtitle: string;
  buttonText: string;
  buttonUrl: string;
  backgroundImage?: string;
};

type FeaturesData = {
  columns: 2 | 3 | 4;
  items: Array<{
    icon: string;       // emoji or icon identifier
    title: string;
    description: string;
  }>;
};

type PricingData = {
  plans: Array<{
    name: string;
    price: string;
    currency: string;
    period: string;     // '/month', '/year', 'one-time'
    features: string[];
    cta: string;
    highlighted: boolean;
  }>;
};

type TestimonialsData = {
  items: Array<{
    quote: string;
    author: string;
    role: string;
    avatar?: string;
  }>;
};

type FaqData = {
  items: Array<{
    question: string;
    answer: string;
  }>;
};

type CtaData = {
  title: string;
  subtitle: string;
  buttonText: string;
  buttonUrl: string;
};

type FooterData = {
  copyright: string;
  links: Array<{ text: string; url: string }>;
};
```
