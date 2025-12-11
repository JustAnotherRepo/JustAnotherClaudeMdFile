# Domain Seed Index

Compact reference for domain classification and content generation. Subagents use this to identify relevant domains, then generate detailed content on-demand.

## Primary Domains

| Domain | Triggers | Key Concepts |
|--------|----------|--------------|
| web-apps | website, SPA, SSR, frontend, browser | Rendering strategies, state management, routing, bundling, hydration |
| apis-backend | API, REST, GraphQL, backend, server | Endpoint design, versioning, authentication, rate limiting, documentation |
| mobile-apps | iOS, Android, mobile, React Native, Flutter | Native vs cross-platform, offline, push notifications, app stores |
| desktop-apps | Electron, desktop, native app, Windows/Mac | Distribution, auto-update, system integration, installers |
| event-driven | events, messaging, pub/sub, async, queue | Event sourcing, CQRS, message brokers, eventual consistency, sagas |
| data-pipelines | ETL, data pipeline, batch, streaming, warehouse | Orchestration, transformation, quality, lineage, CDC |
| ml-ai-systems | ML, AI, model, training, inference | Training pipelines, serving, feature stores, monitoring, MLOps |
| saas-multitenant | SaaS, multi-tenant, B2B platform | Tenant isolation, billing, onboarding, white-labeling, usage metering |
| ecommerce | store, cart, checkout, products, orders | Catalog, inventory, payments, fulfillment, promotions |
| fintech-payments | payments, transactions, ledger, banking | Double-entry, reconciliation, fraud, compliance, card processing |
| healthcare | patient, HIPAA, EHR, clinical, PHI | FHIR, HL7, consent, audit trails, BAAs |
| gaming | game, multiplayer, real-time, lobby | Game loop, netcode, matchmaking, anti-cheat, leaderboards |
| media-streaming | video, audio, streaming, live, VOD | Transcoding, CDN, adaptive bitrate, DRM, low-latency |
| collaboration-realtime | real-time, collaboration, sync, presence | CRDTs, OT, WebSockets, conflict resolution, cursors |
| search-discovery | search, discovery, recommendations, ranking | Indexing, relevance, facets, personalization, embeddings |
| analytics-bi | analytics, reporting, dashboards, metrics | OLAP, dimensions, aggregations, visualization, self-serve |
| identity-auth | auth, login, SSO, permissions, users | OAuth, OIDC, RBAC, ABAC, MFA, session management |
| scheduling-booking | scheduling, booking, calendar, appointments | Availability, conflicts, time zones, reminders, recurrence |
| workflow-bpm | workflow, approval, process, state machine | Orchestration, human tasks, escalation, SLAs, audit |
| geospatial | maps, location, GPS, spatial, routing | Geocoding, tile servers, spatial queries, geofencing |
| iot-edge | IoT, sensors, edge, devices, firmware | Protocols (MQTT, CoAP), provisioning, OTA, telemetry |
| infrastructure | deployment, scaling, networking, compute | Load balancing, auto-scaling, service mesh, DNS |
| blockchain-web3 | blockchain, smart contracts, crypto, web3 | Consensus, wallets, gas, oracles, DeFi |

## Cross-Cutting Concerns

| Concern | When to Add | Key Topics |
|---------|-------------|------------|
| security-architecture | Always | Defense in depth, threat modeling, zero trust |
| compliance-frameworks | Regulated data, specific industries | HIPAA, PCI-DSS, SOC2, GDPR, FedRAMP |
| cloud-architecture | Cloud deployment | Provider services, regions, networking, IAM |
| observability | Production systems | Logging, metrics, tracing, alerting |
| resilience | High availability needs | Circuit breakers, retries, failover, chaos |
| performance | Scale/latency requirements | Caching, optimization, load testing |
| devops-cicd | Any production system | Pipelines, environments, deployment strategies |

## Compliance Triggers

| Keywords | Required Domains |
|----------|------------------|
| healthcare, patient, PHI, medical | healthcare + compliance-frameworks |
| payment, credit card, PCI | fintech-payments + compliance-frameworks |
| EU users, GDPR, privacy | compliance-frameworks |
| financial, banking, SOX | fintech-payments + compliance-frameworks |
| government, federal, FedRAMP | compliance-frameworks |

## Common Combinations

| System Type | Typical Domains |
|-------------|-----------------|
| B2B SaaS | saas-multitenant + identity-auth + apis-backend |
| E-commerce | ecommerce + payments + search-discovery |
| Healthcare platform | healthcare + identity-auth + compliance-frameworks |
| Real-time collaboration | collaboration-realtime + identity-auth + web-apps |
| Data platform | data-pipelines + analytics-bi + apis-backend |
| Mobile app + backend | mobile-apps + apis-backend + identity-auth |
| IoT system | iot-edge + data-pipelines + infrastructure |

## Architecture Styles

| Style | Best For | Key Patterns |
|-------|----------|--------------|
| monolith | Small team, fast MVP, <100K users | Modular structure, clear boundaries |
| modular-monolith | Medium team, clear domains, future extraction | Domain modules, internal events |
| microservices | Large team, independent scaling, complex domains | Service mesh, API gateway, saga |
| serverless | Variable load, event-driven, minimal ops | Functions, managed services, cold starts |
| event-driven | Async workflows, audit needs, eventual consistency | Event sourcing, CQRS, message bus |
| hybrid | Mixed requirements | Combine styles by bounded context |

## Project Types

| Type | Key Questions | Special Patterns |
|------|---------------|------------------|
| greenfield | "What problem? Who for? Why now?" | Full flexibility, clean-slate |
| migration | "What's the legacy? What's the risk?" | Strangler fig, parallel run, feature flags |
| enhancement | "What exists? What constraints?" | Incremental, backward compatible |
| integration | "What APIs? What contracts?" | Adapters, anti-corruption layer |

## Migration Patterns

| Pattern | When to Use | Approach |
|---------|-------------|----------|
| strangler-fig | Gradual replacement | Route requests incrementally to new system |
| parallel-run | High-risk data migration | Run both systems, compare outputs |
| branch-by-abstraction | Replace internal components | Abstract interface, swap implementation |
| event-interception | Decouple from legacy | Capture events from legacy, process in new |
| database-first | Data model changes | Migrate data, then migrate application |
| feature-flags | Controlled rollout | Toggle between old/new per feature/user |

## Migration Risk Factors

| Factor | High Risk Indicators | Mitigation |
|--------|---------------------|------------|
| data | Complex schema, no documentation | Data profiling, mapping exercise |
| integration | Many consumers, undocumented APIs | Contract testing, versioned APIs |
| business-logic | Rules in stored procs, tribal knowledge | Behavior characterization tests |
| downtime | 24/7 availability requirement | Blue-green, canary, feature flags |
| rollback | Can't easily revert | Parallel run, incremental migration |

## Estimation Complexity Indicators

| Indicator | Simple | Medium | Complex |
|-----------|--------|--------|---------|
| requirements | Clear, stable | Some ambiguity | Unclear, changing |
| technology | Team knows well | Some learning | New to team |
| integration | None or simple | Few, documented | Many, undocumented |
| scale | Known, modest | Growth expected | Unknown, potentially large |
| compliance | None | Standard (SOC2) | Strict (HIPAA, PCI) |
| team | Co-located, stable | Distributed | New team, changing |
