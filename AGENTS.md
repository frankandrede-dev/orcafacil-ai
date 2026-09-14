# Codex implementation brief — OrçaFácil AI

## Objective
Build a production-oriented MVP of OrçaFácil AI as a responsive web app/PWA for Brazilian small service businesses.

## Product promise
Turn a short description of a job into a professional quote, send it through a public link, capture the customer's decision, and follow the approved value through simple receivables.

## UX priorities
- Mobile-first but excellent on desktop.
- Portuguese (pt-BR) UI.
- Brazilian currency and date formatting.
- Quote creation should feel fast enough to complete in under two minutes.
- Avoid generic admin-template aesthetics. Use a clean, professional visual system suitable for tradespeople and small service companies.
- Desktop: persistent navigation/sidebar is acceptable.
- Mobile: compact navigation and thumb-friendly primary actions.
- Strong empty states and obvious next actions.

## Required screens
1. Login / cadastro
2. Onboarding da empresa
3. Dashboard
4. Clientes — lista, cadastro, edição, detalhe
5. Catálogo — produtos/materiais e serviços
6. Orçamentos — lista
7. Novo/editar orçamento
8. Detalhe do orçamento + timeline
9. Página pública da proposta
10. Ordem de serviço simplificada
11. Recebimentos
12. Configurações da empresa

## Dashboard KPIs
- Valor enviado
- Valor aprovado
- Taxa de conversão
- Valor recebido
- Saldo a receber
- Orçamentos recentes

## Quote workflow
`draft → sent → viewed → approved | changes_requested | rejected | expired`

Record quote events with timestamps. Customer-facing actions must generate events.

## Quote editor
Support:
- customer
- validity
- products/materials
- services
- quantity
- unit
- unit price
- automatic line totals
- discount: fixed or percentage
- surcharge: fixed or percentage
- total
- payment conditions
- PIX information from tenant settings
- scope/description
- notes/terms
- photos/attachments

## AI scope assistant
Create a server-side abstraction for AI. Input is a short raw service description; output is a polished professional scope in pt-BR. Never expose provider keys in client bundles. Provide a safe mock/fallback for local development when no provider key exists.

## Public proposal
Use a random public token/slug, not a sequential database ID. Public page should show only information intentionally exposed for that quote. Actions:
- Aprovar
- Solicitar alteração
- Recusar

Include a clear confirmation step and record date/time. Do not require the customer to create an account.

## Work order
An approved quote can generate a simple work order containing customer, scope, approved items, planned execution date, status and notes. Do not expand into field-service management in MVP.

## Receivables
Generate planned installments from payment conditions. Track due date, amount, status, paid date and payment method. Keep it deliberately simple.

## Firebase architecture
Use Firebase Authentication and Cloud Firestore behind clean repository/service abstractions. Prefer Firebase Storage for tenant logo and quote attachments.

Every internal business entity must be tenant-scoped. Do not trust a tenantId supplied by the browser as authorization by itself.

Provide `firestore.rules` and document the security model. Do not use permissive production rules.

## Suggested collections
- tenants/{tenantId}
- users/{uid}
- customers/{customerId}
- catalogItems/{itemId}
- quotes/{quoteId}
- quoteEvents/{eventId}
- workOrders/{workOrderId}
- receivables/{receivableId}

Operational documents include tenantId.

## Environment
Create `.env.example`; never commit real credentials. Separate public Firebase client configuration from server-only AI credentials.

## PWA
Add manifest, installable metadata/icons placeholders, theme metadata and sensible caching/offline shell behavior. Do not promise full offline mutation sync in MVP unless implemented reliably.

## Quality gates
- TypeScript strict
- lint
- production build
- basic tests for quote totals/calculation rules
- no committed secrets
- README setup instructions
- responsive UI checked at mobile and desktop widths
- accessible form labels and keyboard behavior

## Seed/demo
Provide a development/demo seed or local mock mode so the interface can be evaluated before Firebase credentials are configured. Demo should contain realistic Brazilian service-business data.

## Explicit non-goals
Do not build full accounting, tax invoicing/NF-e/NFS-e, bank reconciliation, inventory ERP, advanced CRM, payroll, GPS, native Android/iOS apps, lifetime licensing, seller/admin licensing screens, or single-device session locking.
