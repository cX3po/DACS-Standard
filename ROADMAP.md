# DACS Roadmap

DACS **v0.1** deliberately specifies only what is **shipped and reference-backed**:
a rail, method, or capability earns normative status when a live settlement /
verification path exists *and* a reference implementation exercises it. Everything
below is **anticipated follow-on work** — named honestly, with **no committed
dates**. DACS operates under progressive-anchoring phase PA-2 (single steward,
KyneSys Labs); we do not commit timelines we cannot guarantee. Items land in future
minor versions (v0.2, v0.3, …) per the independent per-stage versioning in
[CONTRIBUTING](./CONTRIBUTING.md).

This document is **informative**; the normative source is the
[specification](./spec/SPECIFICATION.md). Where an item is already described in the
spec, the section is cited.

## Settlement rails (DACS-4)

| Item | Status | Notes |
|------|--------|-------|
| `pay-d402` | Anticipated | Native Demos D402 payment path as a first-class rail, once it meets the v0.1 shipped-path + reference-implementation bar. Not subsumed by `pay-evm-erc20` / `pay-solana-spl` — it is a distinct native settlement path. |
| `pay-evm-erc8183` | Anticipated | EVM-native job-escrow rail composing ERC-8183 (already named in the spec, §9). |
| Liquidity Tank Phase 2–4 | Partially live | Phase 1 is live (ETH Sepolia ↔ Polygon Amoy testnet, USDC, unidirectional, EVM-source). Roadmap: Solana destination, bidirectional flow, validator-set co-signing of tank operations, mainnet deployments, additional stablecoins. |
| Streaming / continuous-flow rails | Anticipated | Per-second / per-byte settlement; out of scope for v0.1 (§11.2.4). |

## Identity & vetting (DACS-1 / DACS-2)

| Item | Status | Notes |
|------|--------|-------|
| 6 new CCI contexts (lei, finra-crd, sam-uei, fedramp, naics, cmmc) | Staged | Institutional DACS-1 conformance is gated on these GCR routines being built. |
| DAHR validator-body-signed mode | Anticipated | Strengthens SR-3 beyond hash-commitment; high-stakes recipes (lei, finra-crd, ofac-clear, sam-uei) migrate when it ships (§7.3.5). |
| Selective / minimised-claim disclosure | Anticipated | v0.1 provides no per-claim selective disclosure at the bundle layer (§11.2.7; scope note §6.3.2). |

## Negotiation (DACS-3)

| Item | Status | Notes |
|------|--------|-------|
| CCI-keyed L2PS membership + channel-envelope API + transcript export | Substrate-live; SDK backlog | Today the CH-1..CH-5 channel properties are orchestrator-enforced at the application layer; these move enforcement into the SR-4 substrate (DACS-3 Tier-1). |
| `negotiate-multi-quote` | Anticipated | First-class 1-to-N RFQ; today approximated via `negotiate-sealed-envelope` (§11.2.3). |

## Verify & accountability (DACS-5)

| Item | Status | Notes |
|------|--------|-------|
| DACS-X (dispute / execution-verification) | Anticipated; prototype in progress | Dispute initiation, arbitrator credentialing via DACS-1/2, named-arbitrator transcript disclosure, superseding outcome bundles (§11.2.1). The accountability layer — makes the session record adjudicable; it does not hide session content. |
| DACS-5 → ERC-8004 cross-claim hint | Candidate | Optional bridge for the primary-claim (DACS-5) vs token (ERC-8004) reputation-keying divergence (§10.7). For v0.1 the divergence stands and the §10.7 fetch-the-bundle requirement is the mitigation. |

## Cross-substrate & governance

| Item | Status | Notes |
|------|--------|-------|
| SR-3 / SR-4 wire-protocol harmonisation | Anticipated (v2) | v0.1 specifies SR-3 and SR-4 at the trust-property level only; cross-substrate sessions cannot complete for SR-3- or SR-4-dependent phases until wire formats are aligned (§5). |
| Cross-substrate portability test | In progress | Demonstrate an unmodified `verifyBundle` end-to-end on a non-Demos substrate (a filesystem-backed SR-2 candidate has been exercised; §11.3). |
| PA-2 → PA-3 multi-party governance | Anticipated | Constitution of a multi-party steward body to replace the single-signer recipe and rail registries (§11.2.6). |

## Out of scope (not roadmap)

- **DTR** — node-internal relay, below the DACS abstraction; no DACS-layer rail or parameter.
- **MCP** — agent-to-tool, not agent-to-agent commerce. It composes *adjacent* to DACS (tool discovery sits below DACS; an MCP server can expose a DACS catalog above it) without a normative binding in the standard.

---

*Have an item you think belongs here, or a substrate that ships the SR-1..SR-5
capabilities? See [CONTRIBUTING](./CONTRIBUTING.md).*
