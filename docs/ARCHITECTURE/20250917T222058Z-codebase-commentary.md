# Codebase Commentary — HomeTryOn

**Timestamp:** 2025-09-17T22:20:58Z
**Project UUID:** `urn:uuid:TODO-UUIDv8`

## App.tsx
- Serves as the central controller for product and scene state, orchestrating AI calls via `generateCompositeImage`.
- Touch and mouse drag logic coexist; consider abstracting into custom hooks (`useDragState`, `useTouchGhost`) to reduce component breadth.
- Loading message rotation implemented via interval; ensure cleanup to prevent stale timers when component unmounts.
- Error handling is inline; a notification/toast system would improve UX consistency.

## Components
- **Header.tsx**: Static hero component; opportunity to introduce marketing storytelling, plan/pack highlights, and quick access to billing flows.
- **ImageUploader.tsx**: Drag-and-drop logic is tightly coupled with DOM; migrating to `react-dropzone` or custom hook would increase testability.
- **ObjectCard.tsx**: Handles drag gestures and drop ghost adjustments; could share behavior with a forthcoming catalog grid.
- **ProductSelector.tsx / AddProductModal.tsx**: Provide extension points for pack upsells and cross-sell placements.
- **DebugModal.tsx**: Valuable for internal QA; hide behind feature flag in production builds.
- **Spinner.tsx**: Basic loader; align with new brand motion guidelines for upcoming UX refresh.
- **TouchGhost.tsx**: Utility overlay for mobile; will benefit from CSS variable-driven styling once design system lands.

## Services
- **services/geminiService.ts**: Wraps Gemini API calls; currently using fetch with manual payload assembly. Consider extracting retry/backoff and telemetry instrumentation. Factor environment/config management (e.g., base URL, model selection) into dedicated config file.

## Types
- **types.ts**: Minimal type declarations; plan to expand to include billing entities, analytics payloads, and UI view models.

## Assets & Styling
- **assets/**: Stores placeholder imagery; future plan includes CDN integration and dynamic asset selection per microsaas brand.
- **index.css**: Global styles rely on default tokens; upcoming design system should migrate to CSS variables or Tailwind/resilient utility approach.

## Technical Debt & Opportunities
1. **State Management**: App-level state is monolithic. Introduce context providers or Zustand store to share scene/product/billing states cleanly.
2. **Error Boundaries**: Add React error boundary for AI service errors and network issues.
3. **Testing**: Currently lacks unit/integration test coverage. Introduce Vitest + React Testing Library.
4. **Telemetry**: Logging limited to `console`. Integrate structured logging and analytics ingestion.
5. **Performance**: Large base64 conversions performed on main thread. Move heavy operations to web workers when scaling.

## AI Pipeline Notes
- Gemini compositing depends on stable base64 conversions and prompt assembly. Document prompts and ensure they are versioned.
- Monitor quotas and latency; implement caching of intermediate results when possible.
- Consider alternative fallback models or local diffusers for offline scenarios.

