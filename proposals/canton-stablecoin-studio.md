## Development Fund Proposal

**Author:** [Varmeta](https://var-meta.com)  
**Status:** Draft  
**Created:** 2026-03-26  

---

## Abstract
Canton Stablecoin Studio is a self-service issuance and operations platform for regulated stablecoins on the Canton Network. It gives issuers a standards-conformant way to launch and manage stablecoins through DAML smart contracts, authenticated APIs, and an operator dashboard, with built-in controls for KYC, role-based administration, supply management, account freezes, wipes, pause controls, and proof-of-reserve attestation.

The project is designed to accelerate institutional adoption of Canton by turning stablecoin issuance into a reusable product instead of a bespoke integration. It is built natively on the Canton quickstart stack and aligned to CIP-0056, allocation workflow standards, and Canton’s privacy-preserving operating model.

---

## Specification

### 1. Objective
The Canton ecosystem needs a practical, production-oriented stablecoin issuance toolkit for regulated institutions. Today, an issuer wanting to launch a compliant stablecoin on Canton would need to assemble DAML contracts, backend APIs, compliance workflows, and operator tooling from scratch.

The objective of Canton Stablecoin Studio is to reduce that effort to a repeatable product: an issuer can deploy a new stablecoin by providing token parameters, assign operational and compliance roles, and manage lifecycle actions without writing code. The intended outcome is a reusable issuance platform that proves Canton can support institution-grade stablecoin workflows with privacy, auditability, and standards interoperability built in.

### 2. Implementation Mechanics
The implementation follows the existing Canton quickstart architecture and adds three coordinated layers:

- **DAML stablecoin package** implementing the core ledger model for `StablecoinInstrument`, `RoleAssignment`, `KycCredential`, and `ReserveAttestation`
- **Spring Boot REST layer** exposing issuer and operator workflows through authenticated APIs backed by ledger commands and PQS reads
- **React/TypeScript Studio UI** for issuers and operators to deploy instruments, perform token actions, and monitor reserve posture

Core operational capabilities include:

- Deploying a new stablecoin with `name`, `symbol`, `decimals`, and `supplyCap`
- Assigning and revoking the five required admin roles: KYC Admin, Freeze Admin, Supply Admin, Wipe Admin, and Pause Admin
- Granting and revoking on-ledger KYC credentials, including webhook-based updates from an external KYC provider
- Executing mint, burn, custodial transfer, freeze, unfreeze, wipe, pause, and unpause operations
- Polling external reserve data sources on a schedule and writing append-only reserve attestations to Canton
- Displaying instrument inventory, total supply, and reserve ratio in the Studio dashboard

The system uses Canton as the source of truth for all compliance-sensitive rules. KYC checks, freeze/wipe gating, pause enforcement, and supply-cap protections are enforced on-ledger in DAML choices rather than only in the application layer. The backend and UI improve usability, but the ledger remains authoritative.

### 3. Architectural Alignment
This work is strongly aligned with Canton architecture and ecosystem priorities:

- **Canton-native implementation:** built in DAML on Canton rather than introducing a separate VM or token runtime
- **Standards alignment:** targets CIP-0056 token behavior and uses the existing allocation and token metadata patterns already present in the Canton quickstart
- **Privacy-preserving design:** leverages Canton’s participant and contract visibility model so holdings and compliance data can be scoped appropriately
- **Operational auditability:** all role changes, KYC updates, token actions, and reserve attestations become ledger events with a verifiable history
- **Ecosystem reuse:** produces a reusable issuance framework that can support multiple stablecoins per issuer and serve as a foundation for broader tokenization activity

The project also follows research-driven architectural safeguards identified during planning, including explicit upgrade/version support, ledger-time KYC enforcement, shared supply tracking to prevent concurrent mint cap violations, complete pause-guard coverage, and reserve attestation expiry handling.

### 4. Backward Compatibility
*No backward compatibility impact.*

This proposal introduces net-new stablecoin issuance capabilities on top of the current Canton quickstart stack. It does not require breaking changes to existing applications, token workflows, or ledger integrations.

---

## Milestones and Deliverables

### Milestone 1: DAML Contract Foundation
- **Estimated Delivery:** Completed on 2026-03-20
- **Focus:** Deliver the foundational DAML package implementing the stablecoin ledger model and compliance guards required for v1
- **Deliverables / Value Metrics:**  
  `quickstart-stablecoin` DAR compiled and accepted by Canton; foundational templates for stablecoin instrument, role assignment, KYC credential, and reserve attestation implemented; DAML script tests covering pause enforcement, KYC gating, supply-cap conflict handling, freeze/wipe guard behavior, and upgrade path mechanics

### Milestone 2: Backend REST Layer
- **Estimated Delivery:** 4 weeks after funding approval
- **Focus:** Expose all issuer, operator, and compliance workflows through authenticated REST endpoints and integrate webhook-driven KYC updates
- **Deliverables / Value Metrics:**  
  Stablecoin deployment endpoint live; role assignment and revocation APIs live; KYC webhook endpoint validating HMAC-SHA256 signatures; mint, burn, custodial transfer, freeze, unfreeze, wipe, pause, and unpause APIs operational; successful end-to-end demonstration that all v1 role and token operations are reachable through the API layer and enforced on-ledger

### Milestone 3: Oracle and Dashboard
- **Estimated Delivery:** 2 weeks after Milestone 2 acceptance
- **Focus:** Add proof-of-reserve automation and complete the issuer/operator dashboard experience
- **Deliverables / Value Metrics:**  
  Scheduled reserve polling against configured external data sources; on-ledger `ReserveAttestation` creation with expiry handling; issuer dashboard showing managed stablecoins, total supply, reserve ratio, and stale-data warnings; operator and compliance views for lifecycle actions; end-to-end demonstration of a complete stablecoin studio workflow from deployment through operations and reserve monitoring

---

## Acceptance Criteria
The Tech & Ops Committee will evaluate completion based on:

- Deliverables completed as specified for each milestone
- Demonstrated functionality or operational readiness
- Documentation and knowledge transfer provided
- Alignment with stated value metrics

Project-specific acceptance conditions:

- **Milestone 1:** A compiled DAML package is delivered and accepted by the Canton environment; contract tests prove pause guard enforcement, KYC gating, supply-cap conflict protection, and upgrade support
- **Milestone 2:** An issuer can create a stablecoin and assign all five admin roles through the API; valid KYC webhook events update on-ledger KYC state; mint, burn, transfer, freeze, unfreeze, wipe, pause, and unpause operations succeed or fail according to ledger rules
- **Milestone 3:** Reserve attestations are created automatically after scheduler execution; the dashboard lists managed stablecoins with total supply and reserve ratio; stale reserve data is visibly flagged; operator and compliance workflows are available in the web interface

---

## Funding

**Total Funding Request:** 120,000 CC

### Payment Breakdown by Milestone
- Milestone 1 (DAML Contract Foundation): 25,000 CC upon committee acceptance
- Milestone 2 (Backend REST Layer): 55,000 CC upon committee acceptance
- Milestone 3 (Oracle and Dashboard): 40,000 CC upon final release and acceptance

### Volatility Stipulation
If the project duration is **greater than 6 months**:  
The grant is denominated in fixed Canton Coin and will require a re-evaluation at the 6-month mark.

If the project duration is **under 6 months**:  
Should the project timeline extend beyond 6 months due to Committee-requested scope changes, any remaining milestones must be renegotiated to account for significant USD/CC price volatility.

---

## Co-Marketing
Upon release, the implementing entity will collaborate with the Foundation on:

- Announcement coordination
- A technical case study or blog post covering the architecture and standards alignment of Canton Stablecoin Studio
- Ecosystem promotion through developer education, demos, or partner-facing enablement materials

---

## Motivation
Stablecoin infrastructure is one of the clearest entry points for institutional adoption on a financial network. A reusable issuance studio lowers the cost and risk of launching regulated digital cash products, gives prospective issuers a concrete path onto Canton, and demonstrates the network’s strengths in privacy, composability, and auditability.

For the ecosystem, the value extends beyond one application. The project creates reusable DAML components, standard-aligned APIs, and practical operational patterns that can inform other tokenization efforts. It also gives the Canton ecosystem a visible, issuer-focused reference implementation for compliant asset issuance.

---

## Rationale
This is the right approach because it stays close to the existing Canton quickstart stack and uses the ledger as the enforcement layer instead of treating compliance as middleware. That keeps the architecture credible for regulated use cases and minimizes unnecessary platform risk.

The phased structure is intentional. The DAML foundation is delivered first because contract design is the hard dependency and the most expensive area to correct later. The REST layer comes next to make all issuer and operator actions accessible through stable interfaces. The oracle and dashboard are last because they depend on the underlying issuance and operations model already being complete.

Alternative approaches, such as backend-only compliance enforcement, non-standard token modeling, or a generic off-ledger stablecoin dashboard, were rejected because they would weaken interoperability, reduce trust in enforcement, or fail to showcase Canton’s native strengths. This proposal instead delivers a standards-aligned, ledger-native product that is practical for issuers and strategically useful for the broader ecosystem.