# HomeTryOn Update Checklist — 2025-09-17T22:20:58Z

Project UUID: `urn:uuid:TODO-UUIDv8`

## Documentation & Commentary Updates
- [x] Update `README.md`
  - [x] Insert "Codebase Map" section summarizing key modules (`App.tsx`, each component, `services/geminiService.ts`, `types.ts`, assets) using bullet hierarchy.
  - [x] Add "Architecture & AST Resources" subsection referencing files under `docs/ARCHITECTURE/` (include .uml/.mmd names).
  - [x] Provide "Product Roadmap" with phases (Foundation, UX Enhancements, Billing Integration, Analytics & Growth).
  - [x] Include "UX Vision" subsection describing visually stunning experience updates (color system, layout, animations, onboarding).
  - [x] Document "Billing Integration Strategy" detailing RevenueCat connector and alternative AlbyHub/BTC Pay Lightning module, including microSaaS pack concept and universal connector requirements.

## Architecture Artifacts
- [x] Ensure `.uml` and `.mmd` files accurately reference module relationships and are linked from README.

## Specification Deliverables
- [x] Create `docs/ARCHITECTURE/20250917T222058Z-billing-connector-spec.md`
  - [x] Detail universal billing connector requirements, including API surfaces, data model, and adapter pattern.
  - [x] Outline RevenueCat integration flow (authentication, product catalog sync, purchase validation).
  - [x] Outline AlbyHub/BTC Pay Lightning module flow (invoice creation, webhook handling, entitlement provisioning).
  - [x] Describe microsaas pack bundling and entitlement mapping.

## Commentary Expansion
- [x] Add `docs/ARCHITECTURE/20250917T222058Z-codebase-commentary.md`
  - [x] Provide narrative commentary on each module’s role, current limitations, and opportunities.
  - [x] Include assessment of AI pipeline dependencies and technical debt.

## Verification
- [x] Run `npm run lint` (or appropriate available script) to ensure consistency; if script absent document reason. *(No lint script defined in `package.json`; noted for maintainers.)*
- [x] Update checklist statuses inline (replace `[ ]` with `[x]` or `✅` after completion/testing).

