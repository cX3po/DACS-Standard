# DACS Roadmap

DACS **v0.1** deliberately specifies only what is **shipped and reference-backed**:
a rail, method, or capability earns normative status when a live settlement /
verification path exists *and* a reference implementation exercises it. Everything
below is **anticipated follow-on work** — named honestly, with **no committed
dates**. DACS operates under progressive-anchoring phase PA-2 (single steward,
KyneSys Labs); we do not commit timelines we cannot guarantee.

This document is **informative**; the normative source is the
[specification](./spec/SPECIFICATION.md). Where an item is already described in the
spec, the section is cited.

The roadmap has two parts:

- **Part 1 — Standard (normative).** Work that changes the *specification* itself —
  new rails, methods, rules, governance. These land in future minor versions
  (v0.2, v0.3, …) per the independent per-stage versioning in
  [CONTRIBUTING](./CONTRIBUTING.md), merge through the steward, and can affect
  conformance.
- **Part 2 — Ecosystem & tooling (non-normative).** Supporting artifacts *around*
  the standard — tooling, documentation, reference implementations, test vectors.
  These never change the spec or conformance, are welcomed as community
  contributions under a "contributor prototypes, steward owns the standard" model,
  and may be designated canonical by the steward if they prove solid.

---

# Part 1 — Standard (normative)

## Settlement rails (DACS-4)

| Item | Status | Notes |
|------|--------|-------|
| `pay-d402` | Anticipated | Native Demos D402 payment path as a first-class rail, once it meets the v0.1 shipped-path + reference-implementation bar. Not subsumed by `pay-evm-erc20` / `pay-solana-spl` — it is a distinct native settlement path. |
| `pay-evm-erc8183` | Anticipated | EVM-native job-escrow rail composing ERC-8183 (already named in the spec, §9). |
| Liquidity Tank Phase 2–4 | Partially live | Phase 1 is live (ETH Sepolia ↔ Polygon Amoy testnet, USDC, unidirectional, EVM-source). Roadmap: Solana destination, bidirectional flow, validator-set co-signing of tank operations, mainnet deployments, additional stablecoins. |
| Streaming / continuous-flow rails | Anticipated | Per-second / per-byte settlement; out of scope for v0.1 (§11.2.4). |
| `SettlementFinality` field on SettlementEvidence | Candidate | Surface the chain-specific confirmation / finality model into the evidence record so audit tools can read it without re-deriving. Foundation already exists (PC-4 ties `ok:true` to the chain's finality semantics; rails carry `finalityBlocks` / `commitmentLevel`); this is the surfacing field. |
| `FeeSchedule` disclosure in AgreementDocument | Candidate | Declared/negotiated fee disclosure for regulated contexts (EU MiCA, US MSB). Optional, non-breaking. Design-issue first. |

## Identity & vetting (DACS-1 / DACS-2)

| Item | Status | Notes |
|------|--------|-------|
| 6 new CCI contexts (lei, finra-crd, sam-uei, fedramp, naics, cmmc) | Staged | Institutional DACS-1 conformance is gated on these GCR routines being built. |
| DAHR validator-body-signed mode | Anticipated | Strengthens SR-3 beyond hash-commitment; high-stakes recipes (lei, finra-crd, ofac-clear, sam-uei) migrate when it ships (§7.3.5). |
| Selective / minimised-claim disclosure | Anticipated | v0.1 provides no per-claim selective disclosure at the bundle layer (§11.2.7; scope note §6.3.2). |
| `ReputationHint` at discovery | Candidate | Surface a DACS-5 summary (e.g. `{completedSessions, disputeRate}`) in the DACS-1 listing so buying agents can filter before vetting. Optional, additive (§6.3). |
| `ServiceCategory` taxonomy + category-scoped reputation | Candidate | Listing-level category tag (recommended vocabulary) so DACS-5 reputation can be queried per-domain ("20+ completed data-analysis sessions") rather than as one undifferentiated score (§6.3, §10.5). |
| Primary-claim revocation / compromise signalling | Candidate; design-issue first | A way to publish "this primary claim is compromised" so reputation does not silently transfer to an attacker after key theft. Hard part is revocation without the old key — needs an RFC before a PR. |

## Negotiation (DACS-3)

| Item | Status | Notes |
|------|--------|-------|
| CCI-keyed L2PS membership + channel-envelope API + transcript export | Substrate-live; SDK backlog | Today the CH-1..CH-5 channel properties are orchestrator-enforced at the application layer; these move enforcement into the SR-4 substrate (DACS-3 Tier-1). |
| `negotiate-multi-quote` | Anticipated | First-class 1-to-N RFQ; today approximated via `negotiate-sealed-envelope` (§11.2.3). |
| L2PS channel key rotation / forward secrecy | Anticipated | Rekeying, session-key expiry, and forward-secrecy requirements for long-lived (hours/days) negotiation channels. Lands with the SR-4 ergonomics work. |
| `PriceAnchor` in AgreementDocument | Candidate | Optional DAHR-attested price snapshot both parties sign against, anchoring an agreement to a verifiable market reference (regulated/financial use). Optional, non-breaking (§8.5). |

## Verify & accountability (DACS-5)

| Item | Status | Notes |
|------|--------|-------|
| DACS-X (dispute / execution-verification) | Anticipated; prototype in progress | Dispute initiation, arbitrator credentialing via DACS-1/2, named-arbitrator transcript disclosure, superseding outcome bundles (§11.2.1). The accountability layer — makes the session record adjudicable; it does not hide session content. |
| DACS-5 → ERC-8004 cross-claim hint | Candidate | Optional bridge for the primary-claim (DACS-5) vs token (ERC-8004) reputation-keying divergence (§10.7). For v0.1 the divergence stands and the §10.7 fetch-the-bundle requirement is the mitigation. |

## Cross-substrate & governance

| Item | Status | Notes |
|------|--------|-------|
| SR-3 / SR-4 wire-protocol harmonisation | Anticipated (v2) | v0.1 specifies SR-3 and SR-4 at the trust-property level only; cross-substrate sessions cannot complete for SR-3- or SR-4-dependent phases until wire formats are aligned (§5). |
| PA-2 → PA-3 multi-party governance | Anticipated | Constitution of a multi-party steward body to replace the single-signer recipe and rail registries (§11.2.6). |
| AML / Travel-Rule compliance-metadata hook | Candidate; design-issue first | An optional extension point carrying compliance metadata (e.g. a Travel-Rule flag) for flows that touch real money (fiat via AP2, stablecoins), so DACS is usable in regulated markets. Hook only — DACS does not itself perform AML. |

---

# Part 2 — Ecosystem & tooling (non-normative)

Builder-facing artifacts *around* the standard. None change the spec or conformance.
Welcomed as community contributions under the "contributor prototypes, steward owns
the standard" model; if an artifact proves solid the steward may designate it
canonical. Anything that **mirrors** the spec (a reference tool, a generated doc)
MUST generate from this repo — so it cannot drift from the source of truth — and
version-stamp which DACS version it reflects.

## Tooling

| Item | Status | Notes |
|------|--------|-------|
| DACS spec-reference MCP server | Anticipated (community) | Query the spec by rule/section, fetch artifact schemas (Listing, IdentityBundle, AgreementDocument, SettlementEvidence, AttestationBundle, …), list rule families (SE-*, HTLC-*, PC-*, RAV-*), pull the §14 conformance vectors. Read/search/fetch only. |
| DACS builder/validator MCP server | Anticipated (community) | Construct and verify bundles, run conformance vectors. Follow-on to the reference MCP; held until the reference implementation + §14 vectors stabilise so it doesn't harden against a moving API. |

## Documentation

| Item | Status | Notes |
|------|--------|-------|
| Operational builders guide | Anticipated | A practical integration guide distinct from the existing [why-DACS guide](./docs/builders-guide.md): capital / float requirements, escrow sizing, behaviour when undercapitalised mid-session, key custody / HSM practice, and settlement-finality integration. These are implementation/ops concerns — deliberately **not** normative spec material. |

## Reference implementations & test vectors

| Item | Status | Notes |
|------|--------|-------|
| `agent-commerce-demo` | Live | KyneSys Labs reference implementation; runs an end-to-end DACS-1 … DACS-5 session against Demos testnet. |
| `pathos-dacs-ref` | Live (third-party) | PATH-OS Labs' independent implementation (DACS-1 publisher, DACS-2 GLEIF verifier, DACS-5 envelope-receipt verifier CLI) — the empirical third-party-implementability evidence (§11.3). |
| §14 conformance test vectors | In progress | Machine-runnable vectors for the §14 conformance plan, so implementers can self-check. Grows as contributor work (e.g. DACS-X) produces cases. |
| Cross-substrate portability test | In progress | Demonstrate an unmodified `verifyBundle` end-to-end on a non-Demos substrate (a filesystem-backed SR-2 candidate has been exercised) — the artifact-level proof behind the §11.3 portability claim. |

---

## Out of scope (not roadmap)

- **DTR** — node-internal relay, below the DACS abstraction; no DACS-layer rail or parameter.
- **MCP (as a normative binding)** — agent-to-tool, not agent-to-agent commerce. It composes *adjacent* to DACS (tool discovery sits below DACS; an MCP server can expose a DACS catalog above it) without a normative binding in the standard. (A *spec-reference* MCP server is a separate, welcomed tooling item — see Part 2 → Tooling.)

---

*Have an item you think belongs here, or a substrate that ships the SR-1..SR-5
capabilities? See [CONTRIBUTING](./CONTRIBUTING.md).*
