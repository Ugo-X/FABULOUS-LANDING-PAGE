┌─────────────────────────────────────────────────────────────────────────┐
│                          1) Clients & Interfaces                        │
│  Web / Mobile DApp  |  Maker Console  |  Ops Dashboard (Observability) │
└───────────────▲───────────────────────────────────────────▲─────────────┘
                │                                           │
                │ gRPC/HTTPS                                │
                │                                           │
┌───────────────┴───────────────────────────────────────────┴─────────────┐
│                          2) ProofBridge Services                        │
│  Orchestrator (NestJS)  •  Quote/Match  •  Risk/Policy  •  Webhooks     │
│  ┌───────────────────────────────────────────────────────────────────┐  │
│  │                      Proof / Verification Layer                   │  │
│  │  - ZK Verifier (SNARK/Plonk)                                      │  │
│  │  - Merkle/MMR Proof Checker                                       │  │
│  │  - BLS Aggregation (WIP)                                          │  │
│  └───────────────────────────────────────────────────────────────────┘  │
│                                                                         │
│  Treasury & Reconciliation • Compliance (KYC/KYB/AML) • Audit Log       │
└───────────────▲───────────────────────────────────────────▲─────────────┘
                │                                           │
                │ on-chain tx / anchor calls                │
                │                                           │
┌───────────────┴───────────────────────────────────────────┴─────────────┐
│                 3) Settlement & Contract Surfaces (per chain)           │
│                                                                         │
│  [EVM Chains]                                            [Hedera]       │
│  ┌──────────────────────────────┐            ┌────────────────────────┐ │
│  │ AdManager (EVM)              │            │ ⭐ HSCS (EVM)          │ │
│  │  - posts/locks deposits      │            │   - AdManager (EVM)   │ │
│  │  - updates MMR              │            │   - OrderPortal (EVM)  │ │
│  └──────────────────────────────┘            └────────────────────────┘ │
│  ┌──────────────────────────────┐            ┌────────────────────────┐ │
│  │ OrderPortal (EVM)            │            │ ⭐ HTS (Token Service) │ │
│  │  - verifies ZK/Merkle proofs │            │   - wrap/mint/burn    │ │
│  │  - releases funds            │            │     bridged assets     │ │
│  └──────────────────────────────┘            └────────────────────────┘ │
│                                                 ┌────────────────────┐  │
│                                                 │ ⭐ HCS (Consensus) │  │
│                                                 │  - notarize roots │  │
│                                                 │  - cross-chain    │  │
│                                                 │    coordination   │  │
│                                                 └────────────────────┘  │
│                                                 ┌────────────────────┐  │
│                                                 │ ⭐ Mirror Nodes    │  │
│                                                 │  - proof sourcing │  │
│                                                 │  - monitoring     │  │
│                                                 └────────────────────┘  │
└───────────────▲──────────────────────────────────────────────────────────┘
                │
                │ optional submission
                │
┌───────────────┴──────────────────────────────────────────────────────────┐
│                        4) Relayer / Referee Node                         │
│  Current (MVP): Stateful pre-auth relayer (session mgmt, idempotency)    │
│  Future: Stateless, permissionless (anyone can submit aggregated proofs) │
│  ⭐ Uses Hedera SDK (JS/Java) for:                                        │
│     - HCS topic messages (ordering, notarization)                        │
│     - Scheduled txs / atomicity assists                                  │
└──────────────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────────────────┐
│                             5) AI Assist Layer                           │
│  Maker co-pilot: anomaly detection, auto-responses, fee tuning,          │
│  liquidity rebalancing suggestions, routing hints across EVM/Hedera      │
└──────────────────────────────────────────────────────────────────────────┘
