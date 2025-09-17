<div align="center">
<img width="1200" height="475" alt="GHBanner" src="https://github.com/user-attachments/assets/0aa67016-6eaf-458a-adb2-6e31a0763ed6" />
</div>

# Run and deploy your AI Studio app

This contains everything you need to run your app locally.

View your app in AI Studio: https://ai.studio/apps/drive/1Do75LgNN6NUbdd7WScIZfCip-P68oPs_

## Run Locally

**Prerequisites:**  Node.js

1. Install dependencies: `npm install`
2. Set the `GEMINI_API_KEY` in [.env.local](.env.local) to your Gemini API key
3. Run the app: `npm run dev`

---

## Codebase Map

- **App.tsx** – Core orchestration layer controlling AI composition workflow, drag-and-drop interactions, scene/product state management, and debug instrumentation.
- **components/**
  - `Header.tsx` – Hero header, CTA buttons, and usage guidance.
  - `ImageUploader.tsx` – Handles local uploads and drag-and-drop of scene imagery with visual affordances.
  - `ObjectCard.tsx` – Draggable product card supporting mouse and touch gestures.
  - `ProductSelector.tsx` – Optional selector grid for multi-product catalogues.
  - `AddProductModal.tsx` – Modal wizard for enriching catalog entries.
  - `DebugModal.tsx` – Surfaces composite prompt, debug render, and tuning insights.
  - `Spinner.tsx` – Loading overlay displayed during AI inference calls.
  - `TouchGhost.tsx` – Touch helper aligning feedback between gestures and drop zones.
- **services/geminiService.ts** – Gemini API wrapper that uploads assets, crafts prompt payloads, and returns composite imagery plus debugging metadata.
- **types.ts** – Domain interfaces (`Product`, API request/response contracts) shared across UI and services.
- **assets/** – Default scene/object imagery used for instant-start demos and fallback experiences.
- **metadata.json / vite.config.ts / tsconfig.json** – Build, deployment, and metadata scaffolding.

## Architecture & AST Resources
- Baseline architecture overview: [`docs/ARCHITECTURE/20250917T222058Z-home-try-on-architecture.md`](docs/ARCHITECTURE/20250917T222058Z-home-try-on-architecture.md)
- AST diagrams:
  - PlantUML: [`docs/ARCHITECTURE/20250917T222058Z-home-try-on-ast.uml`](docs/ARCHITECTURE/20250917T222058Z-home-try-on-ast.uml)
  - Mermaid: [`docs/ARCHITECTURE/20250917T222058Z-home-try-on-ast.mmd`](docs/ARCHITECTURE/20250917T222058Z-home-try-on-ast.mmd)
- Commentary & specifications (new):
  - Codebase commentary: [`docs/ARCHITECTURE/20250917T222058Z-codebase-commentary.md`](docs/ARCHITECTURE/20250917T222058Z-codebase-commentary.md)
  - Billing connector specification: [`docs/ARCHITECTURE/20250917T222058Z-billing-connector-spec.md`](docs/ARCHITECTURE/20250917T222058Z-billing-connector-spec.md)

## UX Vision – Toward a Visually-Stunning Experience
- **Immersive layout:** Split-view canvas with responsive glassmorphism panels, depth shadows, and dynamic perspective for product previews.
- **Design language:** Harmonized palette (deep indigo, warm neutrals, accent gradients), custom typography pairing (Display Serif + Geometric Sans), and micro-animations for interactions.
- **Guided creation:** Multi-step onboarding overlay, contextual helper tooltips, and progress timeline for placement workflow.
- **Dynamic previews:** Real-time parallax, highlight glows on drop targets, and cinematic transitions between AI-generated scene options.
- **Accessibility & responsiveness:** High-contrast theme toggle, WCAG-compliant focus states, and adaptive layouts across mobile, tablet, and desktop.

## Billing Integration Strategy – Universal MicroSaaS Packs Connector
1. **Connector Framework**
   - Establish `BillingConnector` interface encapsulating product catalog sync, purchase initiation, receipt validation, and entitlement provisioning.
   - Support plug-in adapters: `RevenueCatAdapter` and `LightningAdapter` (AlbyHub/BTC Pay) with shared logging & telemetry.
   - Define `MicroSaasPack` entity grouping SKUs, entitlements, and feature flags for branded sub-apps.
2. **RevenueCat Path**
   - Use RevenueCat REST API + SDK for cross-platform purchase flows.
   - Map microsaas packs to RevenueCat offerings; sync via cron/CI pipeline.
   - Handle webhooks (`INITIAL_PURCHASE`, `CANCELLATION`, `RENEWAL`) to update entitlements in the platform’s user graph.
3. **Decentralized Lightning Path**
   - Lightning module backed by AlbyHub/BTC Pay Server.
   - Generate invoices per pack, monitor settlement via webhook or LNURL callbacks, then issue JWT entitlements.
   - Provide custody-agnostic wallet support and fallback to RevenueCat when regionally required.
4. **Centralized Catalog Service**
   - Maintain Postgres-backed `billing_catalog` table referencing packs, pricing, provider SKU IDs.
   - Expose GraphQL/REST endpoints consumed by client runtime to fetch available packs and purchase methods.
   - Ensure telemetry + analytics integrated into shared data warehouse.

## Product Roadmap
1. **Foundation (Current)**
   - Stabilize Gemini compositing pipeline, improve error handling, and document architecture.
2. **UX Enhancements (Next)**
   - Implement visual refresh, advanced onboarding, and responsive breakpoints informed by the UX Vision.
3. **Billing & Monetization (Upcoming)**
   - Ship universal billing connector, integrate RevenueCat, deploy Lightning alternative, and enable microsaas pack purchase flows.
4. **Analytics & Growth (Later)**
   - Instrument product analytics, A/B testing harness, partner management portal, and automated marketing workflows.

