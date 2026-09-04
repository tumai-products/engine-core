*Last updated: 2026-09-04 22:00 (UK)*

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
| Skills (119) | `../../tumai-hq/skills/` - [tumai-hq/skills](https://github.com/tumai-hq/skills) |
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
| **Documents** | pdf, pdf-form-filler, docx, pptx, pptx-to-pdf, xlsx, book-to-markdown, ch-accounts-to-markdown, report-to-markdown, note-to-pdf, note-to-word, export-images |
| **Research** | article-reflection, source-digest, linkedin, twitter |
| **Finance** | xero, xero-ap-invoice, rent-invoice, rent-invoice-approve, hmrc, tinytax, tumai-management-payroll-close, invoice-pdf, vat-invoice-request, fportal-prompt, n8n-ap-registry, edgematics-invoice, edgematics-agreement, cleaning-bill-ref |
| **Airtable** | airtable |
| **Atlassian** | jira, atlassian-discovery, atlassian-goals, atlassian-projects |
| **Confluence** | confluence-sync, confluence-scan, confluence-summarise, confluence-publisher, confluence-manager, confluence-pull, confluence-email-scan |
| **Google** | gmail, gcalendar, gdocs, gsheets, gdrive-sync, gworkspace-admin, google-ads, google-analytics, youtube, youtube-notes |
| **SEO** | bing-webmaster |
| **Infrastructure** | cloudflare, statuscake, godaddy, deploy, haproxy, nginx-proxy, network, postgres, proxmox, server, ssl, unifi, vm-decommission, website-deploy, domain-audit, domain-diagnose, mobaxterm-sessions, fleet-audit |
| **GIS** | geojson-inject, gis-geometry, gis-outlier-detect, gis-spatial-tag, gpkg-export, gpkg-inject, csv-inject, postcodes-io |
| **Media** | elevenlabs, audio-check, video-convert, html-to-video, fal-lipsync, voice-to-text, remotion-presentation, youtube-capture |
| **Design** | diagram-generator, figma-design, figma-to-code, frontend-design |
| **Companies House** | companies-house, companies-house-cs-preflight, companies-house-cs-postfiling, companies-house-status |
| **Comms** | email-send |
| **Utilities** | skill-creator, task-creator, sync-workspace, sync-repos, refresh-settings, repo-init, integrate-repo, check-integrations, enrich-twin, session-close, webapp-creator, submission-reset, snagit, watlas, cityfibre-bitbucket-refresh, mept-fixtures-prep, pc-logs, pc-host-canary |
| **Banking** | barclays-inbox |
| **Booking** | acuity |
| **External** | respond-io-ingest, respond-io-media-capture, web-capture-site, hubspot-academy-extract |
| **UK** | uk-vehicles |

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
| `mind-atlas` | HEAD, research, MIND Jira planning home |
| `skills` | Shared AI skills (121) |
| `business-hub` | Business operations |
| `family-hub` | Personal/family life management |
| `beauty-hub` | Kseniia's beauty business |
| `marketing-hub` | Marketing operations |
| `learning-hub` | Education and training |
| `it-hub` | IT infrastructure and operations |
| `sql-hub` | Central SQL workspace |
| `cloud-services` | Managed cloud services inventory |
| `integrations-hub` | Vendor layer - per-vendor docs, vault contracts, smoke tests, tested adapters; cut-off register (Jira: INTEG) |

### Product Tier (tumai-products)

| Repository | Purpose |
|------------|---------|
| `engine-core` | **This repo** - Reusable portal foundation (Go + Vue/DevExtreme) |
| `node-agent` | Infrastructure monitoring agent (Go + Vue) |
| `my-first-app` | Sandbox web app (Vue + Go) |
| `buildsmart` | Product (Jira: BSMART) - CityFibre FTTH DepoNet dataset custody + viewer |
| `file-sync` | Product (Jira: FSYNC) |
| `finance-portal` | Product |
| `mail-atlas` | Product |
| `media-capture` | Product |
| `media-forge` | Product (Jira: MFORGE) |
| `web-capture` | Product (Jira: WCAP) |
| `screen-capture` | Product (Jira: SCAP) |
| `codex` | Product (Jira: CODEX) |
| `work-atlas` | Product (Jira: WATLAS) |
| `buildsmart-portal` | Buildsmart portal - DepoNet dataset custody, S3-to-UNAS transfer, viewer (Jira: BSMART) |

### Programme Tier (tumai-programmes)

| Repository | Purpose |
|------------|---------|
| `kseniia` | Kseniia Brow Art programme |
| `kseniia-website` | Renewed kseniia.co.uk (Nuxt + Tailwind, replacing Tilda) |
| `kseniia-website-design` | Kseniia website design assets |
| `kseniia-academy` | Kseniia Academy programme (strategy + content) |
| `kseniia-academy-webapp` | Academy webapp (Nuxt + Go) |
| `kseniia-academy-webapp-design` | Academy webapp design assets |
| `kseniia-portal-api` | Kseniia portal API |
| `kseniia-portal-web` | Kseniia portal web frontend |
| `kseniia-portal-design` | Kseniia portal design assets |
| `kseniia-client-web` | Kseniia client-facing web app (lightweight, no DevExtreme) |
| `limitless` | Limitless programme |
| `limitless-portal` | Limitless portal webapp |
| `limitless-portal-design` | Limitless portal design assets |
| `limitless-website` | Limitless public website (Nuxt SSG) |
| `stationroadclinic-co-uk` | Station Road Clinic programme |
| `stationroadclinic-co-uk-portal` | Clinic patient portal |
| `stationroadclinic-co-uk-website` | Clinic public website |
| `vasilyev-co-uk-website` | vasilyev.co.uk website |
| `tumai-co-uk` | tumai.co.uk website (Tumai Management Ltd corporate site - Tilda to Nuxt migration, Jira: TUMUK) |
| `tumaifibre` | Tumai Fibre programme + tumaifibre.co.uk website (single-repo monorepo, Jira: TFIBRE) |
| `ftth-acquisition-dd-framework` | Tumai Fibre Scope of Work framework (vendor-neutral, mined from CF/Cheetah portfolio; TFIBRE workstream) |
| `cf-condor-migration` | CityFibre Condor FTTH migration |
| `cf-migration-genoa` | CityFibre Genoa FTTH pre-migration DD |
| `cf-migration-falcon` | CityFibre Falcon FTTH pre-migration DD |
| `cf-migration-osprey` | CityFibre Osprey FTTH pre-migration DD |
| `cf-migration-cougar` | CityFibre Cougar FTTH migration |
| `cf-migration-engine` | CityFibre MigrationEngine programme |
| `cf-migration-reference` | Shared CityFibre migration reference assets |
| `cf-migration-template` | CityFibre migration project template |
| `cf-comarch-reference` | CityFibre Comarch reference assets |
| `cf-me-performance-testing-strategy` | CityFibre MigrationEngine performance testing strategy (Tumai deliverable) |
| `cf-me-test` | ME-TEST online testsuite-driver service for the CityFibre IME migration engine pipeline |
| `cf-test-wrapper` | TEST-WRAPPER testsuite-driver running CityFibre migration jobs through Purplecube (`-549` / `-deploy549` are version/deploy variants) |
| `cf-ime-validation` | CF IME Validation (Hotspots) programme (Jira: CFIMEV) |
| `ime-hotspots` | IME hotspots analysis |
| `ime-mock` | IME mock service (OpenAPI codegen + HTTP server; `-mx480` is a variant) |
| `depotnet` | Depotnet programme (Jira: BSMART) - legacy DepoNet/VST data rescue |

---

*Created 2026-03-12. Part of the [tumai-products](https://github.com/tumai-products) ecosystem.*
