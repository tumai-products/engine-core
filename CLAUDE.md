# CLAUDE.md — Engine Core

Primary instructions and context for Claude when working in this repository.

## Project Overview

**Engine Core** is a reusable portal foundation built in Go and Vue/DevExtreme, designed to serve as the base for multiple SaaS products. It handles multi-tenancy, identity, permissions, content structure, navigation, theming, and feature management — so that each new product is a template configuration on top of the core rather than a rebuild from scratch.

**Repository:** https://github.com/tumai-products/engine-core
**Organisation:** tumai-products (Product Tier)
**Planning & architecture:** `../../tumai-hq/it-hub/engine-core/` — design docs, roadmap, ADRs
**Status:** Phase 0 — Foundation (architecture and planning)

## Technology Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
| **Backend** | Go (gorilla/mux) | REST API, module system, repository pattern |
| **Frontend** | Vue 3 (Composition API) + TypeScript | SPA served by Go backend |
| **UI Components** | DevExtreme (Vue edition) | Licensed, standard across all Tumai apps |
| **Database** | PostgreSQL (Supabase or self-hosted) | Multi-tenant schema design |
| **Auth** | Pluggable (JWT, Azure SSO, Google OAuth) | Configurable per product/tenant |
| **Deployment** | Docker + GitHub Actions + BCL infrastructure | Same CI/CD patterns as existing apps |

## Architecture

```
Browser → HAProxy (SSL) → Engine Core Go backend → PostgreSQL
                                ↓
              Vue SPA (static files from /frontend/dist)
```

The engine is composed of modules:

- **Identity** — user model, auth providers, session management
- **Tenancy** — tenant isolation, per-tenant configuration
- **Permissions** — RBAC/ABAC, role definitions, permission resolution
- **Content** — generic entity framework, forms, wizards
- **Navigation** — configurable sidebar, routing, menu system
- **Theming** — design tokens, per-product brand overrides
- **Features** — feature flags, module activation per product/tenant

## Shared Resources

| Resource | Location |
|----------|----------|
| Skills (65) | `../../tumai-hq/skills/` — [tumai-hq/skills](https://github.com/tumai-hq/skills) |
| API Credentials | `~/.mindatlas/credentials/.env` (local only) |
| Shared Config | `~/.mindatlas/config/` |
| Planning docs | `../../tumai-hq/it-hub/engine-core/` |
| Stack reference | `../../tumai-hq/it-hub/technology-stack-webapps/` |
| Frontend layouts | `../../tumai-hq/it-hub/technology-stack-webapps/frontend-layouts/` |
| DevExtreme components | `../../tumai-hq/it-hub/technology-stack-webapps/devextreme-components/` |
| Auth templates | `../../tumai-hq/it-hub/technology-stack-webapps/auth-templates/` |

## File Locations

| Path | Purpose |
|------|---------|
| `cmd/engine-core/main.go` | Entry point (planned) |
| `internal/config/` | Config struct + env loading (planned) |
| `internal/modules/` | Feature modules (identity, tenancy, permissions, etc.) (planned) |
| `internal/router/` | Route definitions (planned) |
| `internal/middleware/` | Auth, tenant, CORS, logging (planned) |
| `internal/storage/` | Repository interfaces + PostgreSQL implementation (planned) |
| `internal/models/` | Data structs (planned) |
| `migrations/` | SQL migration files (planned) |
| `frontend/src/` | Vue 3 SPA source (planned) |
| `deploy/` | Dockerfile, systemd, install script (planned) |
| `templates/` | Product template definitions (planned) |

## Available Skills

Skills are loaded from `../../tumai-hq/skills/` ([tumai-hq/skills](https://github.com/tumai-hq/skills)):

| Category | Skills |
|----------|--------|
| **Documents** | pdf, docx, pptx, pptx-to-pdf, xlsx, book-to-markdown, note-to-pdf, note-to-word |
| **Research** | article-reflection, source-digest |
| **Finance** | xero, airtable, rent-invoice, rent-invoice-approve |
| **Infrastructure** | cloudflare, statuscake, csv-inject, gpkg-inject, gpkg-export, gis-geometry, gis-outlier-detect, deploy, haproxy, network, postgres, proxmox, server, ssl, unifi, vm-decommission |
| **Media** | elevenlabs, fal-lipsync, remotion-presentation, video-convert, voice-to-text, html-to-video |
| **Google** | gmail, gcalendar, gdocs, gsheets, gdrive-sync, youtube, youtube-notes |
| **Atlassian** | atlassian-goals, atlassian-projects, confluence-sync, confluence-scan, confluence-summarise, confluence-publisher, confluence-manager, confluence-pull |
| **Utilities** | diagram-generator, email-send, export-images, skill-creator, task-creator, sync-workspace, sync-repos, refresh-settings, integrate-repo |
| **Development** | webapp-creator, website-deploy |
| **System** | check-integrations, enrich-twin, submission-reset |

## Conventions

- Go handlers follow: parse request → validate → execute → respond with JSON
- Vue components use `<script setup lang="ts">` (Composition API)
- DevExtreme for all UI components
- Repository pattern for database access (interface-based, swappable providers)
- SQL migrations in `migrations/` — portable across PostgreSQL providers
- Conventional commit messages (`feat:`, `fix:`, `docs:`, `refactor:`, `chore:`)
- No secrets in git — credentials live in `~/.mindatlas/credentials/`

## Related Repositories

### Brain Tier (tumai-hq)

| Repository | Purpose |
|------------|---------|
| `mind-atlas` | HEAD — Research, orchestration |
| `skills` | Shared AI skills (65) |
| `business-hub` | Business operations |
| `family-hub` | Personal/family life management |
| `beauty-hub` | Kseniia's beauty business |
| `marketing-hub` | Marketing operations |
| `learning-hub` | Education and training |
| `it-hub` | IT infrastructure and operations |
| `sql-hub` | Central SQL workspace |

### Product Tier (tumai-products)

| Repository | Purpose |
|------------|---------|
| `engine-core` | This repo — Reusable portal foundation (Go + Vue/DevExtreme) |
| `node-agent` | Infrastructure monitoring agent (Go + Vue) |
| `my-first-app` | Sandbox web app (Vue + Go) |

### Programme Tier (tumai-programmes)

| Repository | Purpose |
|------------|---------|
| `limitless` | Limitless programme |
| `limitless-portal` | Limitless portal webapp |
| `limitless-portal-design` | Limitless portal design assets |
| `limitless-website` | Limitless public website (Nuxt SSG) |
| `kseniia` | Kseniia Brow Art programme |
| `kseniia-website` | Renewed kseniia.co.uk (Nuxt + Tailwind) |
| `kseniia-academy` | Kseniia Academy programme |
| `kseniia-academy-webapp` | Academy webapp (Nuxt + Go) |

---

*Created 2026-03-12. Part of the [tumai-products](https://github.com/tumai-products) ecosystem.*
