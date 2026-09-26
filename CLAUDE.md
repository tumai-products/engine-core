*Last updated: 2026-09-26 13:42 (UK)*

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
| Skills (122) | `../../tumai-hq/skills/` - [tumai-hq/skills](https://github.com/tumai-hq/skills) |
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
| **Utilities** | skill-creator, task-creator, sync-workspace, sync-repos, refresh-settings, repo-init, integrate-repo, check-integrations, enrich-twin, session-close, hub-session-close, webapp-creator, submission-reset, snagit, watlas, cityfibre-bitbucket-refresh, mept-fixtures-prep, pc-logs, pc-host-canary |
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
| `mind-atlas` | This repo - HEAD, research, MIND Jira planning home |
| `skills` | Shared AI skills (121) |
| `business-hub` | Business operations |
| `family-hub` | Personal/family life management |
| `beauty-hub` | Kseniia's beauty business |
| `marketing-hub` | Marketing operations |
| `learning-hub` | Education and training |
| `it-hub` | IT infrastructure and operations |
| `sql-hub` | Central SQL workspace |
| `integrations-hub` | Vendor layer - per-vendor docs, vault contracts, smoke tests, tested adapters; cut-off register (Jira: INTEG) |

### Product Tier (tumai-products)

| Repository | Purpose |
|------------|---------|
| `engine-core` (this repo) | Reusable portal foundation (Go + Vue/DevExtreme) |
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
| `front-desk` | Front Desk (internal name) - agent-first multi-channel product: one AI agent per business on every channel, people behind the desk who supervise and take over; Oblique Beauty first tenant (Jira: FDESK) |
| `media-studio` | Media Studio - web UI over media-forge jobs: review keyframes and guide steps on a timeline, caption, export guides plus screens for Claude Code; third of the media family (Jira: MSTUDIO) |

### Programme Tier (tumai-programmes)

| Repository | Purpose |
|------------|---------|
| `kseniia` | Kseniia Brow Art programme |
| `kseniia-website` | Renewed kseniia.co.uk (Nuxt + Tailwind, replacing Tilda) |
| `kseniia-website-design` | Kseniia website design assets |
| `kseniia-academy` | Kseniia Academy programme (strategy + curriculum + content; launch-site brief) - the technique-teaching stream of the Kseniia family (umbrella Jira KBA, KACAD proposed) |
| `kseniia-academy-webapp` | Academy webapp (Nuxt + Go); first increment is the kseniia.academy launch site in `web/` (live 2026-09-22 as a noindex draft) |
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
| `cf-platform-reference` | CityFibre engineering platform reference - GitHub org, CI/CD, EKS/Argo CD GitOps, ECR, Secrets Manager, RDS, SSO, guardrails; shared truth for every Tumai app inside CityFibre (Jira: CFPLAT) |
| `kseniia-portal-app` | Kseniia Portal App - practitioner phone edition of portal.kseniia.co.uk (native Android + iOS, Jira: KPAPP) |
| `kseniia-portal-app-design` | Kseniia Portal App design assets - Figma exports, design notes, source screenshots (Jira: KPAPP) |
| `oblique` | Oblique Beauty programme - AI booking concierge pilot over Phorest for three South Kensington salons, Telegram first then mobile web; catalogue-as-data thesis (Jira: OBLQ) |
| `kseniia-library` | Kseniia content library - L0 captured sources (HITCH 4.0, HITCH 6.0) and L1 bilingual knowledge nodes shared by Kseniia Academy, Kseniia Business and Kseniia Brow Art (KBA DEC-004) |

### Integrations Tier (tumai-integrations)

Runnable integration workloads - `<vendor>-sandbox` prototypes per vendor and `<vendor>-hub` / `<vendor>-gateway` shared services (org created 2026-08-31, DEC-INTEG-006, registered MIND-100). Boundary rule (DEC-INTEG-006, extends DEC-INTEG-004): vendor knowledge, vault contracts, adapters and smoke tests stay in `tumai-hq/integrations-hub`; runnable prototypes and shared gateway services live in `tumai-integrations`; a prototype graduating into a product moves to `tumai-products` deliberately, never by drift. Naming (vendor-first, decided 2026-09-01, DEC-INTEG-006 amendment): `<vendor>-<role>[-<qualifier>]` - `<vendor>` = the integrations-hub slug; `<role>` = `sandbox` (may rot and be archived; topic `sandbox`), `tenant` (registration/config repo a platform reads), `hub` / `gateway` (production treatment). First repo: `telegram-hub` (INTEG-38); next: `argo-cd-sandbox`, `argo-cd-tenant`, `argo-cd-sandbox-espresso` (Tumai twin platform).

| Repository | Purpose |
|------------|---------|
| `telegram-hub` | Telegram bot-host - multi-bot registry, plugin handlers, admin UI; long-poll v1 (Jira: INTEG, Epic INTEG-36) |
| `argo-cd-sandbox` | Tumai twin platform - Argo CD, ApplicationSet, Gateway API, ESO, admission policy, kind config (Jira: INTEG-43) |
| `argo-cd-tenant` | Tumai twin platform - Argo CD registrations apps/<app>/<env>/<tenant>/config.yaml (Jira: INTEG-43) |
| `argo-cd-sandbox-espresso` | Tumai twin platform - espresso sample app, our copy of CityFibre's cf-k8s-espresso-deployment shape (Jira: INTEG-43) |

### Tumai CF Platform (tumai-cf-platform)

The Tumai CF platform - a one-to-one replica of CityFibre's engineering platform org (`cityfibre-enterprise-architects`) on Tumai's side, built to rehearse and deliver IME-HOTSPOTS and later Tumai apps for CityFibre with full visibility (org created 2026-09-07, CFPLAT DEC-004, registered MIND-105; Jira: CFPLAT, epic CFPLAT-8; plan `cf-platform-reference/platform/tumai-cf-platform/README.md`). The org policy mirrors CityFibre's (Actions allow-list, read-only workflow token, owner-only repo creation, the same org secret `CICD_PRIVATE_KEY` and variable `CICD_APP_ID`). Naming: CityFibre's exact repo names, not the vendor-first INTEG convention - `ime-hotspots` and `cf-k8s-espresso-deployment` (app deployment repos), `tumai-eks-tenant` (Argo CD registrations, tenant `tumai`), `platform` (IaC, add-ons, runbook, journal). Nothing on screen says twin, sandbox or mirror. The reusable installer kit stays in `tumai-integrations/argo-cd-sandbox` (INTEG-43).

| Repository | Purpose |
|------------|---------|
| `tumai-eks-tenant` | Argo CD registrations for the Tumai CF platform - apps/<app>/<env>/tumai/config.yaml, CityFibre's cf-enterprise-architects-eks-tenant shape (Jira: CFPLAT) |
| `cf-k8s-espresso-deployment` | Espresso sample app for the Tumai CF platform - Go service, Dockerfile, kustomize base and overlays, rendered-branch workflows; the shape of CityFibre's cf-k8s-espresso-deployment (Jira: CFPLAT) |
| `platform` | The Tumai CF platform itself - Argo CD project + ApplicationSet over tumai-eks-tenant, gateway, External Secrets, admission policy, namespace limits, cluster IaC, runbook and journal; the platform-team side CityFibre does not show us (Jira: CFPLAT) |
| `me-runner` | ME-RUNNER (Migration Engine Runner) deployment repo on the Tumai CF platform - the primary line of ME-TEST for CityFibre's Kubernetes platform: code, Dockerfile, kustomize base/overlays, rendered-branch workflows; rehearsal of cityfibre-enterprise-architects/me-runner (Jira: CFPLAT E8, app side CFMEPT Track T) |

### Platform Tier (tumai-platform)

Build-and-run engineering - our own development tooling, Kubernetes platforms, AWS infrastructure, scaling, CI and fleet tooling: production-and-maintenance tools, not deliverables (org created 2026-09-26, Team plan; charter ASM DEC-001; Jira: ASM). The long-lasting platform domain; `tumai-cf-platform` is one client-specific direction inside it. Repo types `tool` (internal engineering tools) and `infra` (platform and infrastructure repos). Nothing here is model-specific: tools that drive an AI coding harness spawn the genuine CLI and stay on the Max subscription, never the API.

| Repository | Purpose |
|------------|---------|
| `assembly` | Assembly - the Windows desktop app where coding sessions are produced at scale: many sessions across the orgs started, watched, messaged and closed from one place; drives the genuine Claude Code CLI over stream-json, subscription never API; .NET 9, WPF, DevExpress 26.1 (Jira: ASM) |

### Legacy (pre-Claude Code workflow)

- `fedorgithub/cheetah-document-analysis` - Cheetah FTTH programme. Migration to `tumai-programmes` pending; delivery takes priority.
- `fedorgithub/cf-cheetah-migration-sql` - already a separate repo.
- `qoobo`, `tumaicoding` - to be reviewed over time.

> **Note:** `web-portal` and `frontend-shared` have been retired. See `engine-core` and `it-hub` for current frontend patterns.

---

*Created 2026-03-12. Part of the [tumai-products](https://github.com/tumai-products) ecosystem.*
