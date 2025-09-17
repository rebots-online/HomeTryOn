# HomeTryOn Architecture Overview (Baseline AST Abstraction)

**Timestamp (UTC):** 2025-09-17T22:20:58Z

## Hybrid Knowledge Graph Sync
- Existing hybrid knowledge graph entry for this repository: _not found_.
- Created new baseline AST abstraction and queued for sync with hybrid knowledge graph under project UUID `urn:uuid:TODO-UUIDv8` (update with actual assignment when available).

## Repository Summary
- **Framework:** React 18 + TypeScript, Vite build tooling.
- **Primary Modules:**
  - `App.tsx`: Orchestrates application state, AI image composition workflow, drag-and-drop interactions, debug modal management.
  - `components/`: Presentational and interaction components (header, uploader, selectors, modals, drag helpers).
  - `services/geminiService.ts`: Handles interaction with Google Gemini API for image generation.
  - `types.ts`: Shared TypeScript interfaces.
  - `assets/`: Default imagery used when demo starts instantly.
  - `services`, `metadata.json`, `vite.config.ts`, etc.: supporting configs and service bindings.

## AST Abstraction (High-Level)
```
Program
└── Module(App.tsx)
    ├── Imports(React hooks, services, components, types)
    ├── Constants(transparentDragImage, loadingMessages)
    ├── HelperFunction(dataURLtoFile)
    ├── Component(App)
    │   ├── StateDeclarations
    │   ├── Refs(sceneImgRef)
    │   ├── DerivedData(sceneImageUrl, productImageUrl)
    │   ├── Callbacks(handleProductImageUpload, handleInstantStart, handleProductDrop, handleReset,...)
    │   ├── Effects(useEffect URL cleanup, loading message cycling)
    │   ├── Render(JSX layout with Header, ImageUploader, ObjectCard, DebugModal, TouchGhost)
    │   └── EventHandlers(DragDrop, Touch interactions)
    └── ExportDefault(App)
└── Module(components/*)
    ├── Header(tsx): Stateless header with CTA buttons and instructions.
    ├── ImageUploader(tsx): Handles upload inputs, drop zone, emits callbacks.
    ├── ProductSelector(tsx): Lists available products.
    ├── ObjectCard(tsx): Draggable product representation.
    ├── AddProductModal(tsx): Modal for product addition.
    ├── DebugModal(tsx): Shows prompt + debug image.
    ├── Spinner(tsx): Loading indicator.
    └── TouchGhost(tsx): Visual feedback for touch drag.
└── Module(services/geminiService.ts)
    ├── Imports(environment config)
    ├── Constants(API endpoints, keys)
    ├── Functions
    │   ├── `uploadImageToGemini`
    │   ├── `generateCompositeImage`
    └── Export(generateCompositeImage)
└── Module(types.ts)
    ├── Interface(Product)
    └── Interface(API Responses)
```

## Proposed Enhancements Overview
1. **Documentation & Architecture**
   - Add detailed architecture diagrams (.uml, .mmd) reflecting module interactions and data flow.
   - Document architecture and development roadmap within README and docs.
2. **UX & Visual Enhancements**
   - Introduce refined layout, typography, and color system.
   - Add guided onboarding and interactive previews.
3. **Billing Integration Strategy**
   - Define connector architecture for RevenueCat and decentralized AlbyHub/BTC Pay flow.
   - Abstract billing adapter to support "MicroSaaS packs" concept.
4. **Roadmap to Completion**
   - Stabilize AI pipeline, implement analytics, and release Beta with monetization.

Detailed actionable checklist stored in `docs/CHECKLISTS/20250917T222058Z-home-try-on-roadmap-checklist.md`.

