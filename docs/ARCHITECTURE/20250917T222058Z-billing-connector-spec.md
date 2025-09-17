# Billing Connector Specification — HomeTryOn Platform

**Timestamp:** 2025-09-17T22:20:58Z
**Project UUID:** `urn:uuid:TODO-UUIDv8`

## 1. Objectives
- Centralize billing for branded MicroSaaS experiences under a unified platform.
- Support both RevenueCat-managed subscriptions and a decentralized Lightning (AlbyHub/BTC Pay) module.
- Package entitlements into "MicroSaaS Packs" purchasable individually or in bundles.

## 2. High-Level Architecture
```
Client Runtime
   │
   ▼
Billing API Gateway ──> Billing Service (NestJS/Express) ──> Provider Adapters
                                      │
                                      ├─ RevenueCatAdapter
                                      └─ LightningAdapter
                                      
Shared Services: Postgres (catalog + entitlements), Redis (session), Webhook Processor, Telemetry Bus
```

## 3. Data Model (Postgres)
- `billing_catalog`
  - `id` (uuid)
  - `slug`
  - `display_name`
  - `description`
  - `feature_flags` (jsonb)
  - `default_price`
  - `is_active`
- `billing_catalog_provider`
  - `catalog_id` → `billing_catalog.id`
  - `provider` (`revenuecat` | `lightning`)
  - `provider_product_id`
  - `currency`
  - `price`
- `user_entitlements`
  - `id` (uuid)
  - `user_id`
  - `catalog_id`
  - `source_provider`
  - `purchase_reference`
  - `status` (`active`, `expired`, `revoked`)
  - `activated_at`
  - `expires_at`

## 4. BillingConnector Interface
```ts
interface BillingConnector {
  syncCatalog(packs: MicroSaasPack[]): Promise<void>;
  createCheckoutSession(input: CheckoutInput): Promise<CheckoutSession>;
  validateReceipt(payload: unknown): Promise<EntitlementUpdate>;
  revokeEntitlement(reference: string): Promise<void>;
}
```

### Shared Types
```ts
type Provider = 'revenuecat' | 'lightning';

type MicroSaasPack = {
  id: string;
  slug: string;
  displayName: string;
  description: string;
  features: string[];
  price: number;
  currency: string;
  providerSkus: Record<Provider, string>;
};

interface CheckoutInput {
  userId: string;
  packId: string;
  provider: Provider;
  metadata?: Record<string, string>;
}

interface CheckoutSession {
  sessionId: string;
  provider: Provider;
  url?: string;
  lightningInvoice?: string;
}

interface EntitlementUpdate {
  userId: string;
  packId: string;
  status: 'active' | 'expired' | 'revoked';
  provider: Provider;
  reference: string;
  activationDate?: string;
  expirationDate?: string;
}
```

## 5. RevenueCatAdapter
- **Catalog Sync**
  - Use RevenueCat REST endpoint `/v1/products` to fetch offerings; compare with `MicroSaasPack` definitions.
  - Push updates via Admin Panel or automated API when divergence detected.
- **Checkout**
  - Generate RevenueCat app user ID (e.g., `rc_${userId}`) and initialize RevenueCat web SDK for purchase.
  - Provide fallback hosted checkout using RevenueCat Paywalls API.
- **Receipt Validation**
  - Process webhooks (`INITIAL_PURCHASE`, `RENEWAL`, `CANCELLATION`, `UNCANCELLATION`).
  - Use `/v1/subscribers/{app_user_id}` to confirm entitlement states.
- **Entitlement Updates**
  - Translate RevenueCat entitlement identifiers to `MicroSaasPack` IDs.
  - Persist to `user_entitlements` and emit domain events (`entitlement.activated`, etc.).

## 6. LightningAdapter (AlbyHub/BTC Pay)
- **Catalog Sync**
  - Mirror packs into BTC Pay pricing via API; store `storeId` and `priceId` per pack.
  - Provide conversion for fiat pricing when necessary using price oracle microservice.
- **Checkout**
  - Create invoice via BTC Pay API or AlbyHub API, returning `lightningInvoice` + optional LNURL.
  - Poll or subscribe to invoice settlement webhooks; confirm payment finality.
- **Receipt Validation**
  - Trustless validation using payment hash; store hashed invoice ID as `purchase_reference`.
- **Entitlement Updates**
  - On settlement, activate pack and set expiration based on pack configuration (one-time vs subscription).
  - Provide revocation logic triggered by refund or expiry.

## 7. MicroSaaS Pack Bundling
- Support composite packs referencing child pack IDs.
- Checkout flow calculates aggregate price and issues entitlements for each child.
- UI should surface pack metadata (features, visuals) pulled from catalog service.

## 8. API Endpoints (Billing Service)
- `GET /v1/packs` – list available packs (with provider availability flags).
- `POST /v1/checkout` – initiate purchase (delegates to adapter).
- `POST /v1/webhooks/revenuecat` – handle RevenueCat events.
- `POST /v1/webhooks/lightning` – handle Lightning settlement callbacks.
- `GET /v1/users/:id/entitlements` – read entitlements for gating features.

## 9. Security & Compliance
- Store provider secrets in Vault or Secret Manager; rotate quarterly.
- Sign outgoing webhook acknowledgements; verify HMAC on incoming webhooks.
- Log all billing events with GDPR-compliant retention and deletion policies.

## 10. Rollout Plan
1. Implement connector scaffolding with feature-flag gating.
2. Pilot with RevenueCat adapter; validate entitlements end-to-end.
3. Introduce Lightning adapter for crypto-enabled markets; add currency conversion testing.
4. Launch microsaas pack marketplace UI with combined analytics dashboards.

