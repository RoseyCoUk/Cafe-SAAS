# CleaningPRO-CODEX Implementation Plan v1.4

## Vision
Transform the cleaning-company dashboard into CleaningPRO — a unified operations hub that blends TailAdmin Pro’s expanded component inventory with our bespoke cleaning workflows. The goal is to accelerate delivery, improve UX consistency, and unlock new automation use cases (AI, logistics, integrations) without rebuilding patterns that already ship in the Pro bundle.

## Objectives
- Upgrade every core journey (scheduling, billing, staffing, quality, support) using TailAdmin Pro layouts and components as foundations.
- Leverage Tailwind CSS v4 features (`@theme`, container queries, state variants) for consistent theming and responsive behaviour documented in `tailwind-docs.md`.
- Introduce new modules enabled by the Pro bundle (AI assistants, integration marketplace, API key management, logistics dashboards, invoice suites).
- Maintain clean separation between frontend contracts and backend services per `cleaning-saas-frontend-plan.md` while expanding the component library with Pro assets.
- Reuse every relevant TailAdmin Pro page/component in-place, layering cleaning-specific behaviour through adapters so we preserve update paths and licensing compliance.
- Deliver a complete frontend implementation that can operate on mock data first; backend wiring, auth, and tenancy logic will follow once UI is feature complete.

## Project Setup & Migration Strategy
- Treat `tailadmin-next-typescript-pro-2.0-main` as the canonical codebase. Initialise a new Git history (or branch) there and migrate custom cleaning code into it. Avoid copying Pro components into the free demo project.
- Create a `src/cleaning` (or `src/modules/cleaning`) workspace to hold domain-specific hooks, types, constants, and UI wrappers. Import TailAdmin Pro components into these wrappers rather than editing vendor files directly.
- Move any bespoke components authored previously (stat cards, scheduling widgets, forms) into the new workspace and refactor them to consume Pro primitives (`Card`, `DataTable`, `Modal`, etc.). Delete originals in the legacy demo after successful migration to prevent divergence.
- Catalogue every legacy cleaning component and classify as **wrap**, **replace with Pro**, or **retire** so duplication is eliminated up front.
- Preserve all TailAdmin Pro routes/pages even if unused today. For pages outside the cleaning scope, hide navigation entries but keep files untouched for future SaaS expansion.
- Maintain a shared theme file that maps the cleaning brand palette onto the Pro CSS variables. Ensure typography, spacing, and iconography remain compatible with Tailwind v4 tokens.
- Document all changes in `docs/cleaningpro-changelog.md` (new file) so future TailAdmin Pro updates can be merged by replaying the customization steps.
- Use feature flags (lightweight toggle config) for modules earmarked for future multi-tenant SaaS (e.g., Integration Marketplace, API keys). Keep UI present but gated to admin users.

### Legacy Component Audit & Migration Matrix
| Classification | Components | Strategy |
| --- | --- | --- |
| **Wrap** | `RecurringJobScheduler`, `RouteOptimizer`, `ChecklistManager`, `QualityAssurance`, `CostCalculator` | Rebuild as thin adapters around Pro FullCalendar, logistics, task, support, and billing primitives while keeping bespoke business rules (recurrence, scoring, rate calculations). |
| **Replace with Pro** | `BookingForm`, `ClientCard`, `ClientList`, `ContractManager`, `CustomerPortal`, `EquipmentTracker`, `EstimateBuilder`, `FeedbackCollector`, `InspectionReport`, `InventoryManager`, `InvoiceGenerator`, `JobAssignment`, `JobCard`, `PayrollCalculator`, `ServiceCatalog`, `StaffScheduler`, `ReminderSystem` | Drop legacy implementations and implement equivalent flows using TailAdmin Pro’s ecommerce, CRM, logistics, and communication suites plus cleaning-specific theming and copy. |
| **Retire** | `AppointmentCalendar`, `QuoteGenerator`, `ServiceAreaMapLegacy` | Do not migrate; functionality is covered by Pro dashboards/modules above or superseded by updated workflows. Remove after confirming no dependencies remain. |

Maintain a living spreadsheet mirroring this table so future TailAdmin updates include a quick verification step for each wrapped component.

### Data & Mock Service Migration
- Create `src/cleaning/mocks` housing typed fixtures for clients, bookings, staff, invoices, expenses, and quality reports. Export Zod schemas that mirror backend contracts to guarantee compatibility.
- Provide transformation scripts (e.g., `/scripts/migrate-demo-data.ts`) that convert legacy demo JSON into the new schema shape consumed by Pro components.
- Define `src/cleaning/services` interfaces (`SchedulingService`, `BillingService`, `MessagingService`) with mock implementations so backend teams can later drop in live adapters without touching UI code.
- Validate API contracts via unit tests that parse mock payloads with the shared Zod schemas, ensuring frontend expectations remain in sync during backend development.

## TailAdmin Pro Opportunity Map
- **Dashboards**: Import analytics, SaaS, logistics, CRM, marketing, stocks layouts as configurable landing pages (`tailadmin-next-typescript-pro-2.0-main/src/app/(admin)/(home)/`).
- **Business Suites**: E-commerce/billing stack for invoices, products, transactions (`.../(others-pages)/(ecommerce)`), Support ticketing (`.../(support)`), AI assistants (`.../(ai)`), API key and integrations centers (`.../api-keys`, `.../integrations`).
- **Component Library**: Advanced tables, calendars, popovers, ribbons, progress bars, data tiles referenced under `src/components/` provide ready-to-skin primitives.
- **Forms & Wizards**: Form layouts, grouped inputs, multi-step examples under `src/components/form` streamline onboarding, booking and quality-check flows.
- **UI Elements**: Enhanced notifications, tooltips, tabs, pagination and carousel patterns (`src/app/(admin)/(ui-elements)/`) fill gaps in our base component set.

## Cleaning Company Requirements & Page Inventory
Cover the owner’s current operations while keeping room for SaaS expansion. Deliver all pages below; leave unused TailAdmin Pro samples intact but unlinked.

### Core Operations (Must Ship Day One)
- `Dashboard · Operations Command Center` – cleaning KPIs, crew & job status, quick actions.
- `Schedule / Calendar` – recurring bookings, drag-and-drop assignment, route planner, day/week views.
- `Bookings / Jobs` – tabular list with filters, bulk actions, job creation modal, incident logging.
- `Clients` – CRM list, profile detail (history, documents, communications), tagging.
- `Staff` – roster, availability, certifications, time clock, performance graphs.
- `Quality` – checklist library, inspection reports, photo evidence gallery, audit trail.
- `Support & Communications` – ticket list/reply UI, automation rules, messaging templates.
- `Billing & Invoices` – invoices, payments, subscriptions, pricing plans, revenue dashboards.
- `Expenses & Inventory` – expense entry/import, supply levels, vendor management, purchase orders.
- `Reports & Analytics` – revenue, utilisation, satisfaction, retention, crew efficiency.
- `Settings` – company profile, teams, roles, permissions, notification preferences, theming.

### Client Portal Deliverables (Self-Service)
- Branded portal (`/client/portal`) with dashboards for upcoming services, invoices, and documents.
- Self-service booking/reschedule workflows leveraging Pro forms + calendar widgets.
- Service history with before/after photo gallery, notes, and satisfaction feedback submission.
- Payment center (saved cards, receipts, auto-pay settings) powered by Pro billing pages.
- Document vault (contracts, insurance certificates) and referral/review modules to drive growth.
- Mobile-first responsive layouts with clear CTAs for contact, reschedule, or upsell offerings.

### Field Workforce Experience
- `/field` PWA-ready views for daily schedule, check-in/out (GPS + timestamp), supply usage, and signature capture.
- Camera integration for photo evidence (before/after, incident reporting) with automatic compression + metadata storage.
- Offline-ready job cards with sync queue, plus push notification hooks for assignment changes.
- Simple supply request + damage reporting forms that flow into support/operations queues.

### Extended Value (Prepare for near-term expansion)
- `Integrations Marketplace` – cards & modals for Plaid, QuickBooks, Twilio, Mapbox, Stripe, Zapier.
- `API Keys` – developer console, key creation/revocation, usage charts.
- `AI Assistants` – text/image/code generator tailored to cleaning use cases (email drafts, SOPs).
- `Task & Project Boards` – Kanban for deep cleans, move-out projects, inspection remediation.
- `Knowledge Base & Training` – FAQ layout with SOPs, videos, and certification tracking.
- `Logistics Dashboard` – crew routing, vehicle health, travel analytics.
- `Marketing & Growth` – pipeline dashboard, campaign performance (optional but retain layout).

### Multi-Tenant Readiness (UI preparation only)
- Tenant switcher pattern (no backend yet) for future franchise roll-out.
- Subscription management screen (reuse Pro pricing tables) hidden behind feature flag.
- Audit log & activity feed surfaces to ease compliance in SaaS phase.

## Experience Architecture Enhancements
### 1. Operations Command Center
- Start from the SaaS dashboard shell (`.../saas/page.tsx`) and adapt metrics to cleaning KPIs (daily cleans, utilisation, NPS).
- Embed Logistics widgets (delivery map, activity timeline from `.../logistics/page.tsx`) to visualise crew routes and real-time status.
- Enable quick actions ribbon using Pro action cards (`src/components/ui/card`) to jump into booking, dispatch, or incident logging.

### 2. Intelligent Scheduling & Route Management
- Replace custom calendar scaffolding with Pro FullCalendar integration and drag-and-drop tables (`src/components/calendar`, `components/tables/DataTables`).
- Use Logistics Delivery Timeline components to build a “Route Planner” view showing consecutive jobs, travel times, and equipment per crew.
- Integrate pagination tabs and filter pills from `ui/tabs` and `ui/buttons-group` for schedule filters (region, crew, recurrence).

### 3. Client Lifecycle & CRM
- Extend the CRM dashboard assets (`.../(home)/crm`) for customer segmentation, retention insight, and contract values.
- Reuse advanced profile layout (`.../(others-pages)/profile`) for client and partner detail pages (show cleaning plans, documents, satisfaction scores).
- Adopt Support ticket list components for client communication history and escalation pipelines.

### 4. Staff Productivity & Compliance
- Combine time-clock UI from `cleaning-saas-frontend-plan.md` with Pro attendance tables (`components/tables/BasicTables`) and performance charts (`components/analytics`).
- Use Task/Kanban pages (`.../(others-pages)/(task)`) to assign deep-clean projects, inspections, and follow-ups.
- Adopt notification and badge components to surface compliance alerts (missed clock-ins, certifications expiring).

### 5. Billing, Revenue & Procurement
- Drop in the Pro invoice suite (`.../(ecommerce)/invoices`, `create-invoice`, `single-invoice`) to manage recurring contracts, one-off jobs, and payment tracking.
- Adapt product management pages to track cleaning service packages, add-ons, and consumable sales (supply restock, equipment rental).
- Couple transaction detail view with financial KPIs (AR aging, revenue recognition) using charts from `components/analytics`.

### 6. Quality & Support Automation
- Utilise Support ticket list/reply pages for incident management, satisfaction follow-up, and client communication threads.
- Integrate AI assistant suite (`.../(ai)`) to draft customer emails, training scripts, quality checklist narratives, and marketing materials. Tie outputs into messaging templates defined in the main plan.
- Implement knowledge base & FAQ screens using existing FAQ layout to house SOPs, cleaning standards, and training videos.

### 7. Integrations & Platform Settings
- Reuse API key management (`.../api-keys`) to let franchise partners or mobile teams connect external tools.
- Present integration marketplace using cards/modals from `.../integrations` for Plaid, QuickBooks, Twilio, Mapbox, Stripe, Zapier.
- Surface environment configuration, webhooks, and audit logs with tables and modals from the Pro settings pages.

### 8. AI-Assisted Workforce Tools
- Embed AI chat panel within staff or scheduling views to suggest rescheduling, crew assignments, or cleaning checklists.
- Use AI code/image generator layouts to run “before/after” marketing asset creation based on uploaded photos (hook into `react-dropzone` flows).
- Combine AI outputs with workflow automation (auto-fill quality forms, summarise client feedback) via `React Query` mutations.

## Navigation & Access Control Guidelines
- Build navigation config in `src/config/navigation.ts` that mirrors TailAdmin Pro structure but groups menu items by cleaning workflows (Operations, Clients, Staff, Finance, Support, Platform).
- Keep all Pro sample pages registered but toggle visibility via roles/feature flags. For now, expose only the cleaning owner’s required modules; leave SaaS-only items hidden yet routable.
- Implement role scaffolding (`owner`, `manager`, `field_staff`) to gate advanced modules (billing, integrations) while still presenting a cohesive experience.
- Add breadcrumb and quick action links to ensure every page in the inventory is discoverable without manual URL entry.

## Component Library Upgrades
- **Stat & Metric Cards**: Extend `SaasMetrics`, `LogisticsMetrics`, `SupportMetrics` to build a reusable metric factory with Tailwind CSS `@theme` tokens.
- **Charts**: Standardise on Pro ApexCharts wrappers for revenue, utilisation, churn, and satisfaction analytics.
- **Tables**: Adopt DataTable variants (with inline actions, bulk selection, column menus) for bookings, invoices, expenses, and inventory.
- **Modals & Popovers**: Utilise pre-built modal stacks for multi-step forms (e.g., rapid booking, request reschedule) with accessible focus traps.
- **Progress & Ribbons**: Use ribbons and progress bars to communicate cleaning quality scores, SLA adherence, and onboarding progress.
- **Notifications & Toasts**: Style success/error/alert states using Pro notification patterns plus Tailwind dark-mode variants from `tailwind-docs.md`.

## Tailwind & Design System Alignment
- Keep tokens defined in `app/globals.css` but sync brand and accent colours with CleaningPRO palette; Pro components rely on CSS variables such as `--color-brand-*`.
- Apply container queries (`@container`) to maintain responsive card grids within complex layouts (refer to Tailwind docs).
- Consolidate spacing, radius, and typography scale so Pro imports respect the cleaning theme without manual overrides.

## Implementation Timeline Snapshot
| Phase | Scope Highlights | Target Duration |
| --- | --- | --- |
| Phase 0 | Project bootstrap, workspace creation, mock scaffolding | 2 days |
| Phase 1 | Branding, navigation, role/feature flags | 2 days |
| Phase 2 | Dashboards, scheduling, route planner | 4 days |
| Phase 3 | Client CRM, support, staff management, field UI | 3 days |
| Phase 4 | Billing, payments, inventory, expenses | 3 days |
| Phase 5 | Quality control, documentation, media workflows | 2 days |
| Phase 6 | Integrations, API keys, AI assistants, settings | 2 days |
| Phase 7 | QA, accessibility, performance, handoff | 2 days |

Adjust durations as team capacity changes, but keep total around 20 working days to meet the owner’s expectations.

**Performance & Quality Guardrails (apply to every phase exit)**
- Maintain Core Web Vitals baselines (LCP < 2.5s, CLS < 0.1, TBT < 200ms on mid-tier hardware).
- Ensure lighthouse accessibility score ≥ 95 in both light/dark themes.
- Keep incremental bundle increases under 200 KB per phase by leveraging code-splitting/lazy loading.
- Target ≥ 80% component/unit test coverage for new modules; add Storybook stories for every new composite UI.
- Verify mock data completeness (create/read/update flows) before marking a phase complete.
- If performance/regression thresholds are breached, document fallback steps (component replacement or custom rebuild) before promoting to the next phase.

## Phased Action Plan
Each phase assumes frontend-only scope driven by mock data and service stubs. Move to the next phase only after exit criteria are met to keep the build orderly and auditable.

### Phase 0 – Project Bootstrap & Migration
**Objectives**
- Clone TailAdmin Pro as the base project and stand up development tooling.
- Create the `src/cleaning` workspace and port custom utilities/components from the demo.
- Establish wrapper exports so Pro components remain untouched.

**Key Tasks**
- Initialise repository, install dependencies, verify `npm run dev`/`npm run build`.
- Copy domain models, hooks (`useRecurringSchedule`, etc.), mock data, and helper utilities into `src/cleaning`.
- Create `src/components/pro` proxy modules that re-export TailAdmin Pro primitives ready for theming overrides.
- Set up shared mock services (`src/cleaning/mocks`) and Storybook/Playwright scaffolding.
- Draft higher-level Git/CI notes (branch protections, required checks) in `docs/cleaningpro-changelog.md` for future ops work.

**Deliverables**: Running app on Pro codebase with cleaning workspace added, documented change log entry, mock data available.

**Exit Criteria**: No direct edits in vendor directories, Storybook boots, all migrated components compile against Pro wrappers, Git workflow draft captured for future automation.

#### Phase 0 Progress – Session 1
- Confirmed TailAdmin Pro base `npm run build` succeeds without vendor edits; noted existing Storybook lint violations are ignored via ESLint config rather than modifying bundled stories.
- Established CleaningPRO workspace scaffold (`src/cleaning/**`) with domain barrels, Zod schemas, and mock datasets for clients, staff, bookings, invoicing, expenses, messaging, quality, and payroll.
- Added `src/components/pro` adapter exports to keep Pro primitives untouched while exposing a stable import surface for CleaningPRO modules.
- Introduced mock service layer (`src/cleaning/services/*`) and a `useCleaningServices` hook that surfaces mock adapters behind consistent interfaces.
- Ran `npm run lint`; configured Flat ESLint ignore for `src/stories/**` and documented owner request to postpone Storybook/Playwright scaffolding. Storybook boot exit criterion deferred and tracked as a follow-up when test tooling reinstates.

### Phase 1 – Theme, Navigation, Security Expectations & System Foundations
**Objectives**
- Align branding, typography, and spacing tokens with cleaning palette without breaking Pro defaults.
- Define navigation, role gating, feature flags, and baseline security expectations for owner, manager, staff, and client roles.

**Key Tasks**
- Update `app/globals.css` and associated token files with CleaningPRO branding using Tailwind v4 `@theme`.
- Implement navigation config (`src/config/navigation.ts`), breadcrumbs, quick actions, and role visibility rules.
- Add layout wrappers (AppShell, sidebar, header variants) referencing Pro components but injecting cleaning copy/icons.
- Introduce feature-flag utility for SaaS-only modules and tenant-ready UI.
- Document frontend security touchpoints: session strategy (JWT/cookie placeholder), CSRF mitigation plan, role-based route guards, and API error-handling expectations to align with future backend work.

**Deliverables**: Branded shell, navigation tree covering all required pages, role-based visibility matrix, feature-flag map.

**Exit Criteria**: Dark/light themes verified, navigation exposes/hides modules per owner vs manager vs field role, design tokens documented, security expectation notes reviewed with backend lead.

#### Phase 1 Progress – Session 1
- Refreshed Tailwind `@theme` tokens in `src/app/globals.css` to match CleaningPRO palette (brand blues, eco accents, surf neutrals) and updated focus ring styling; verified light/dark contrast with the ThemeToggle.
- Defined feature-flag scaffold (`src/cleaning/config/featureFlags.ts`) and role-aware navigation sections (`src/cleaning/config/navigation.ts`) covering Operations, Clients, Staff, Finance, Support, and Platform modules with badge metadata for beta/flagged items.
- Added layout wrappers (`CleaningAppShell`, `CleaningSidebar`, `CleaningHeader`) under `src/cleaning/components/layout/` reusing TailAdmin primitives; new route group `/admin/cleaning/[[...section]]` consumes the shell and surfaces mock-driven workspace metrics.
- Implemented navigation helpers (`resolveNavigationSections`, `isRouteAccessible`) plus mock owner dashboard preview to ensure role gating works with default and feature-flagged states.
- Captured security baseline in `src/cleaning/config/security.ts` (sessions, CSRF, RBAC, audit, storage, uploads) for backend review; noted owner decision to postpone Storybook/Playwright so verification relies on lint/build + manual QA.

### Phase 2 – Operations Command Center & Scheduling Suite
**Objectives**
- Build the main operations dashboard using Pro SaaS/logistics widgets.
- Deliver scheduling experiences (calendar, kanban, route planner) using FullCalendar and task components.

**Key Tasks**
- Compose dashboard overview combining `SaasMetrics`, logistics charts, and cleaning custom cards.
- Implement schedule pages with FullCalendar, drag/drop interactions, kanban workflow, and quick booking modal.
- Wire mock data to dashboards and scheduling views; ensure data states (empty/loading/error) are covered.
- Configure route-planning layout using logistics map/timeline components.
- Build a robust recurrence engine (1/2/4-week, biweekly, custom seasonal patterns) with holiday skipping, weather-based rescheduling, client preferences per occurrence, and dynamic pricing adjustments tied to service agreements.
- Link recurrence instances to automated invoice generation rules (per-visit vs monthly) and ensure billing events publish to messaging service stubs for receipts.
- Hook scheduling events into messaging service stubs for weekend reminders and day-of ETA notifications, with fallback flows for manual overrides.

**Deliverables**: `/admin/dashboard`, `/admin/cleaning/schedule`, `/admin/cleaning/jobs`, `/admin/cleaning/routes` with full UI interactions.

**Exit Criteria**: All scheduling views responsive, kanban + calendar share data store, quick actions navigate correctly, QA screenshots captured, recurrence engine validated against owner’s automated scheduling/billing scenarios (including weather/holiday overrides).

#### Phase 2 Progress – Session 1
- Added operations data structures: Zod schemas (`src/cleaning/schemas/operations.ts`), mock datasets (`src/cleaning/mocks/operations.ts`), recurrence utilities (`src/cleaning/utils/recurrence.ts`), and an `OperationsService` mock so UI layers consume a consistent contract.
- Delivered Command Center experience (`/admin/cleaning/operations/command-center`) with stat grid, upcoming visits table, job board snapshot, and route plan overview powered by the new operations service.
- Built Scheduling workspace (`/admin/cleaning/operations/schedule`) combining calendar (FullCalendar), list view, recurrence manager with simulated vs scheduled cadence comparison, and quick booking modal stubbing client/staff selection.
- Implemented Job Board (`/admin/cleaning/operations/jobs`) and Route Planner (`/admin/cleaning/operations/routes`) pages reusing TailAdmin logistics/task primitives and highlighting risk badges (holiday/weather) sourced from recurrence flags.
- Expanded hooks (`useOperationsDashboard`, `useSchedulingWorkspace`) and updated navigation wrappers so all operations routes resolve correctly with role gating and breadcrumb metadata.

#### Phase 2 Progress – Session 2
- Extended operations schemas and mocks with billing automation events, messaging queue items, and alert payloads generated from recurrence outputs (holiday/weather flags drive status and notes).
- Updated billing/messaging/operations service contracts to expose the new queues; hooks (`useOperationsDashboard`) now fetch automations alongside metrics, job board, and routes for the command-center view.
- Composed new command center cards (`AutomationQueueCard`, `OperationsAlertsCard`, `JobBoardSnapshotCard`, `CommandCenterQuickActions`) and wired them into `/admin/cleaning/operations/command-center` with refresh + error states.
- Introduced shared `OperationsDashboardProvider` context so all operations routes reuse the same data/refresh cycle; command center now includes an operations snapshot card summarising approvals, billing, messaging, and weather risk counts with lightweight refresh controls across pages.
- Added last-sync telemetry via the shared provider so Command Center, Job Board, and Routes views surface refresh timestamps and reuse the new operations snapshot payload instead of duplicating data work.
- Added empty/blank-state messaging across scheduling and operations components (upcoming visits table, list view, kanban board, calendar, route planner) so mocks surface helpful guidance once data is exhausted.
- Snapshot export now includes automations/alerts for future dashboard widgets, keeping operations mocks aligned with billing + messaging service stubs.
- Scheduling workspace now surfaces an occurrence insights drawer that highlights billing automations, messaging queue items, and risk alerts per visit, sharing the operations refresh telemetry for quick QA.
- Added an operations timeline card in the command center that blends upcoming visits, billing runs, and messaging events with filter controls, plus anomalies surfaced on routes to flag high-risk stops quickly.
- Introduced seasonal pricing adjustments, preferred crew windows, and per-visit pricing breakdowns so recurrence validation highlights base vs premium charges inside scheduling and billing flows.
- Added route efficiency metrics (service vs drive minutes, capacity utilisation) with date filtering and summaries on the routes view to validate crew load balancing.

#### Phase 2 Progress – Session 3
- Delivered a drag-and-drop route planner board that supports stop resequencing and cross-crew reassignment with inline utilisation feedback and staging lanes.
- Augmented `OperationsDashboardProvider` to apply manual overrides to metrics, timeline entries, and alerts while persisting state via the mock operations service.
- Refactored route mocks to emit multi-crew plans per date (including flex and unassigned lanes) and capture manual override alerts so command center refreshes reflect dispatcher edits.
- Added planner view toggles (day-centric calendar view vs crew-first view) and enhanced drop zones so dispatchers can move visits into empty lanes without friction.

### Phase 3 – Client & Staff Management Modules
**Objectives**
- Adapt Pro CRM, profile, and support components to cleaning client workflows.
- Implement staff roster, availability, time clock, and performance dashboards.

**Key Tasks**
- Build client list/detail pages using CRM layouts, add cleaning-specific tabs (properties, preferences, history).
- Integrate support ticket components for client communications and automation rules.
- Assemble staff module with roster tables, attendance charts, certification trackers, and time clock panel.
- Provide field-staff mobile-friendly views (using Pro mobile layouts where available).
- Configure automated SMS/email templates for 4-week reminders, day-of arrival notices, follow-up surveys, and emergency alerts.
- Add referral tracking, review management, and client tiering to CRM analytics widgets.
- Implement time-clock batch entry + payroll journal export, including manual overrides and approval workflows.
- Outline offline sync strategy for field apps (IndexedDB queue, conflict resolution, network detection) so backend engineers can align on APIs later.

**Deliverables**: `/admin/clients/*`, `/admin/support/*`, `/admin/staff/*`, `/field/*` with mock data.

**Exit Criteria**: Client + staff CRUD operations simulated via mocks, tablet/mobile breakpoints verified, role gating enforced, automated messaging templates tested against scheduling events, offline sync notes reviewed with engineering.

#### Phase 3 Progress – Session 1
- Added CRM/support schema extensions (`clientSegment`, `supportTicket`, `staffShift`, time clock, certifications) with mock datasets, services, and hooks powering Phase 3 views.
- Implemented client workspace: `/admin/cleaning/clients/crm` directory + detail routes, messaging automation preview, feedback dashboards, and portal teaser powered by the new hooks.
- Delivered staff workspace (roster, time clock, payroll preview, field briefings) and support desk kanban with SLA metrics, knowledge base list, and QA reporting seeded from mock data.

#### Phase 3 Progress – Session 2
- Normalised finance + messaging schemas so CRM, billing, and automation flows share the same typed payloads (`invoice.number`/`balanceDue`, template `category`).
- Enhanced CleaningPRO Pro wrappers (`Card`, `Table`, `Badge`) to accept refs/native attributes, unlocking drag targets and responsive layouts required by staff + field tooling.
- Shipped the mobile-first Field App (`/field`, `/field/briefings`, `/field/time-clock`) with bottom navigation, staffing summaries, and mock time-clock interactions aligned to field roles.
- Expanded navigation config to expose Field App links for `field_staff` roles and verified role-gated access across admin + mobile shells.
- Captured offline sync approach (IndexedDB queue, optimistic punches, conflict resolution, connectivity banners) to brief backend engineers ahead of API integration.

### Phase 4 – Finance, Billing & Inventory Workflows
**Objectives**
- Leverage Pro ecommerce/billing suite for invoices, payments, and product catalogues.
- Stand up expense, procurement, and inventory interfaces.

**Key Tasks**
- Configure invoice list/detail/create pages with cleaning templates (residential, commercial, deep clean, etc.).
- Repurpose product/transaction modules for service packages, consumables, and revenue tracking.
- Implement expenses and supply management using tables + forms, add import modal for bank statements.
- Ensure financial dashboards feed metrics into command center.
- Enable automated billing options (per-visit, monthly consolidation) tied to recurrence engine, with receipt generation and delivery via messaging service stubs.
- Build manual payroll journal generator from time-clock data with CSV/PDF export and audit trail.
- Add supply reorder points, vendor assignments, and low-stock alerts leveraging Pro notification patterns.
- Gate bank-statement upload behind feature flag so the owner can disable it if Rocket Money integration is preferred.

**Deliverables**: `/admin/billing/*`, `/admin/payments/*`, `/admin/inventory/*`, expense dashboards.

**Exit Criteria**: Invoice flows end-to-end with mock data, expense import modal functional (or flagged off), financial KPIs visible on dashboard, payroll journal export validated against time-clock mocks.

#### Phase 4 Progress – Session 1
- Established finance schema/service layer (`inventoryItem`, `paymentRecord`, `FinanceService`) with mock datasets covering invoices, expenses, payroll, and supply stock.
- Shipped `/admin/cleaning/finance/billing` (list, detail, new invoice) using reusable finance components and receivables timeline integrated with mock payments.
- Delivered `/admin/cleaning/finance/payments` for payouts, payroll journal, expenses, and a bank-import modal plus `/admin/cleaning/finance/inventory` highlighting low-stock and critical supply alerts.
- Updated navigation + workspace hooks so finance metrics (outstanding balance, payroll pending, low-stock counts) surface consistently across admin dashboards.

#### Phase 4 Progress – Session 2
- Embedded finance KPIs on the operations command center via `FinanceSummaryCards`, wiring the widget to `useFinanceWorkspace` and showing guarded errors if finance mocks fail to resolve.
- Updated command center quick actions to point nightly billing reviews at `/admin/cleaning/finance/billing`, keeping operations teams aligned with the new finance workspace routes.
- Wrapped the payments bank statement import modal in the `bankStatementUpload` feature flag using `resolveFeatureFlags()` so environments without the integration keep the UI hidden.
- Shipped a general ledger CSV export stub on the payments workspace, combining payment and expense mocks so accounting can validate mappings ahead of live integrations.

#### Phase 4 Progress – Session 3
- Captured the end-to-end bank statement import handshake in `docs/finance-bank-import-handshake.md`, detailing session creation, signed uploads, confirmation, and polling contracts so backend enablement can proceed without further UI work.
- Logged error taxonomy, idempotency rules, and follow-up tasks to guide backend readiness while keeping `bankStatementUpload` defaulted to false until endpoints land.
- Verified the payments modal respects feature flag + role checks and documents future wiring so Phase 4 exit criteria now cover finance KPIs, exports, and gated imports.

### Phase 5 – Quality Assurance, Support & Documentation
**Objectives**
- Provide comprehensive quality management, checklist, and media evidence features.
- Finalise support portal, knowledge base, and document centre.

**Key Tasks**
- Build quality checklist library using Pro form examples; include inspection report gallery and photo upload (dropzone).
- Configure support ticket list/reply pages with cleaning issue taxonomy and SLA ribbons.
- Hook FAQ/knowledge base layouts into cleaning SOP content; add video training page.
- Adapt file manager for contracts, certifications, before/after albums.
- Implement structured photo evidence workflow (before/after pairing, timestamp & GPS metadata, compression, legal disclaimers).
- Include configurable consent and retention policies (watermarking, chain-of-custody logs, GDPR-compliant retention toggles) surfaced within admin settings.
- Add emergency job intake flow that feeds both scheduling and support pipelines.

**Deliverables**: `/admin/quality/*`, `/admin/documents/*`, enhanced support centre, media galleries.

**Exit Criteria**: Checklist creation/editing works with mocks, photo gallery responsive with metadata retained, emergency job handling tested, support ticket workflow navigable start-to-finish.

#### Phase 5 Kickoff Prep
- Coordinate with backend on finance contract handoff (inventory, payments, bank imports) so quality/support work can reuse the same service scaffolding.
- Finalise scope for quality checklist data models and ticket escalations, ensuring mock services mirror expected API shapes.
- Outline QA acceptance matrix (accessibility, offline support for field uploads, ticket SLA alerts) before building new modules.

#### Phase 5 Progress – Session 1
- Added quality service adapters + `useQualityWorkspace`, expanded quality mocks (checklists, action plans) and refreshed the QA dashboard with scorecards, checklist library, remediation plans, and evidence gallery dropzone.
- Surface support escalations within the quality workspace while keeping remediation plans aligned to linked inspection reports.
- Implemented support ticket detail route with SLA ribbons, internal log stubs, and kanban deep-linking; enhanced knowledge base with category filters, training media placeholders, and document centre preparation notes.
- Layered in a checklist detail drawer with inline status/notes toggles so inspectors can update items without leaving the dashboard (UI-only until backend sync).
- Built an emergency intake workflow that captures high-severity incidents, previews dispatch data, and stages billing flags ahead of live integrations.
- Added an inspection timeline component highlighting recent reports and surfaced escalation summary messaging so emergency intakes document downstream notifications.
- Introduced issue-trend KPIs on the quality dashboard and added AI prompt prep/download guidance to the knowledge base in preparation for Phase 6 assistants.
- Added an evidence gallery and mock ticket action buttons, with backend TODOs noted for media uploads, ticket assignment, billing holds, and escalation APIs.
- Added a document centre page with search/filter + download placeholders and surfaced SLA trend analytics on the support desk (flagged for backend data wiring).

#### Phase 5 Progress – Session 2
- Extended the Document Centre with a bulk-upload modal that supports multi-file staging, category overrides, description, and tagging so owners can prep SOP drops for demos.
- Stored uploaded files in-memory with mock object URLs to keep newly added assets visible and searchable without touching the shared support service mocks.
- Highlighted backend follow-ups in the modal copy (object storage target, POST endpoint, retention policies) so enabling the real API is a drop-in once storage contracts are ready.
- Added a summary panel surfacing document counts, active categories, top tags, and recent uploads to round out the Document Centre experience for Phase 5 demos.
- Introduced a preview modal with metadata, inline iframe support for friendly file types, and download fallback messaging so the mock demo mirrors the production contract expectations.

#### Phase 5 Progress – Session 3
- Refined the Document Centre grid with Next.js-backed thumbnail previews, keeping cards visually rich without abandoning the mock data pipeline.
- Added pending-assignment card states (warning badges, release targets, awaiting-team copy) so staged documents demonstrate real-world routing workflows while backend APIs bake.
- Expanded `supportDocument` contracts and mocks to capture thumbnails, assignment status, awaiting teams, and release targets for handoff to storage/services teams.
- Updated the bulk-upload flow to flag new files as pending assignments with automatic thumbnails and 24-hour release windows, showcasing the future approvals queue end-to-end.

#### Phase 5 Progress – Session 4
- Wired quality checklist evidence and field assignments into both the admin dashboard and field app, highlighting pending approvals and offline uploads.
- Established `/admin/cleaning/documents` with storage/retention controls, a backend contract checklist, and companion docs to guide Phase 6 service wiring.
- Enhanced the knowledge base with taxonomy, AI prompt inventories, and a working transcript export hook that exercises the new support transcript service.
- Linked emergency intake to scheduling and finance mocks so dispatch, billing, and escalation summaries reflect real data during handoff demos.

### Phase 6 – Platform, Integrations & AI Assistants
**Objectives**
- Surface integration marketplace, API key console, and configuration settings.
- Embed AI assistant suite tailored to cleaning scenarios.

**Key Tasks**
- Customise integration cards/modals for Plaid, QuickBooks, Twilio, Mapbox, Stripe; add placeholder credential states.
- Implement API key management with usage metrics and role-based actions.
- Integrate AI text/image/code generator layouts with cleaning prompts and output previews.
- Extend settings pages (business, services, pricing, notifications, security) with cleaning copy and toggles.
- Introduce weather provider integration hooks so scheduling can auto-flag jobs for reschedule when severe conditions are forecast.

**Deliverables**: `/admin/integrations`, `/admin/api-keys`, `/admin/settings/*`, `/admin/ai/*`.

**Exit Criteria**: Feature flags in place for SaaS-only modules, AI assistants render with cleaning prompt catalog, settings forms save to mock store, weather alert toggle feeds into scheduling mocks.

#### Phase 6 Progress – Session 1
- Added a mock platform service + workspace hook exposing integrations, API keys, and global settings so platform modules read from consistent contracts.
- Stood up `/admin/cleaning/platform/settings` with editable business/security toggles powered by `PlatformSettingsForm` and documented mock-save behaviour.
- Delivered the integration marketplace grid and API key console, gating them behind `integrationMarketplace`/`apiKeys` feature flags while showcasing connector status, scopes, and mock actions.
- Seeded platform mock data (Plaid, QuickBooks, Twilio, Mapbox, Stripe) and API key records to make Phase 6 demos meaningful before live credentials land.

#### Phase 6 Progress – Session 2
- Built a CleaningPRO AI assistant hub with curated prompt packs, sample outputs, and activation guidance so stakeholders can rehearse quality/support workflows ahead of model wiring.
- Layered in integration credential modals that outline Plaid, Twilio, Mapbox, and QuickBooks onboarding steps, triggered from the marketplace grid.
- Added weather-aware scheduling controls by extending platform settings and surfacing high-risk visit alerts directly on the scheduling workspace.
- Published reusable AI data sets and preview components to accelerate future assistant routes while keeping everything feature-flag friendly.
- **Backend follow-up**: Wire real OAuth/token exchanges for Plaid/Twilio/Mapbox/QuickBooks, persist platform settings server-side, and stream AI responses once LLM endpoints are assigned.

#### Phase 6 Progress – Session 3
- Expanded the AI workbench with a runnable editor, local run history, and mock responses so demos can simulate assistant output without backend wiring.
- Enhanced integration modals with credential forms, save affordances, and success feedback to mirror upcoming OAuth/token workflows.
- Propagated weather-alert banners across scheduling, command center, and route planner views to keep dispatch decisions consistent.
- **Backend follow-up**: Replace mock prompt execution with live LLM streaming, store integration credentials securely, and feed real weather data into occurrence flags.

#### Phase 6 Progress – Session 4
- Completed the platform settings suite with service catalog, pricing, notification, and security cards so `/admin/settings/*` is fully represented in the CleaningPRO shell.
- Enabled integration status overrides so credential saves flip marketplace cards to “Connected,” matching the future post-OAuth UX.
- Polished the AI workbench with clipboard helpers and consolidated run history, keeping the experience production-ready once models land.
- **Backend follow-up**: Persist the new settings and credential changes server-side, audit integration status updates, and wire model streaming when APIs are available.

### Phase 7 – QA, Accessibility & Launch Preparation
**Objectives**
- Validate design/system integrity, responsiveness, and accessibility.
- Prepare documentation for backend handoff and future updates.

**Key Tasks**
- Run UI regression suite (Storybook snapshots, Playwright flows) across primary journeys.
- Perform accessibility pass (focus order, ARIA, contrast via Tailwind tokens) and dark-mode verification.
- Finalise navigation/site map matrix, data contract docs, and frontend handoff guide for backend team.
- Create release checklist covering environment config, lint/type checks, smoke test scripts.
- Define layered testing approach: unit tests (Vitest + Testing Library), integration tests (Testing Library + MSW), end-to-end tests (Playwright), visual regression (Chromatic/Storybook), accessibility automation (axe-core), and optional Lighthouse performance runs.

**Deliverables**: Test reports, accessibility checklist, navigation/site map documentation, backend integration brief.

**Exit Criteria**: Zero open UI defects, accessibility checklist signed off, documentation approved by stakeholders, test suites green across unit/integration/E2E/visual baselines.

## Delivery Notes
- Preserve Pro file structure when importing; wrap with domain-specific adapters to avoid direct modification (easier to apply future TailAdmin updates).
- Track license usage and note components customised beyond the template to simplify future merges.
- Document any new APIs required (AI suggestions, integration management) in parity with `cleaning-saas-frontend-plan.md` contracts.
- Focus exclusively on frontend implementation; backend services, auth, and multi-tenant provisioning will be wired after the UI is feature-complete and validated with mock data.
- Maintain a lightweight engineering runbook capturing Git workflow (branch protections, release tags) and CI/CD steps so deployment automation can follow once the frontend stabilises.
- Log risks and mitigation status (Pro component compatibility, bundle size creep, offline sync reliability, photo compliance) in `docs/cleaningpro-changelog.md` alongside remediation owners.
