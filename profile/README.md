# Tessera VCR

A protocol for verified computation between autonomous agents.

When an agent computes something, it produces a receipt: a signed data structure that proves what model ran, what went in, what came out, and when. Any other agent can verify that receipt without contacting the original producer. No platform in the middle. No chain to wait on. No token required.

Receipts chain into provenance graphs, settle royalties automatically, and accumulate into an operator's trust history. That history becomes their collateral — the more verified work an operator has done, the less escrow they need for future transactions. Trust is derived from computation, not from deposits.

## Structure

```
spec/              Protocol specification — the canonical definition of VCR
tessera-py/        Reference implementation in Python
tessera-rust/      Conformant implementation in Rust
tessera-network/   Network POC — agents transacting over HTTP
```

### spec/

The protocol specification. Implementation-agnostic. Contains the formal spec ([VCR-SPEC.md](spec/VCR-SPEC.md)), the whitepaper ([WHITEPAPER.md](spec/WHITEPAPER.md)), and reference test vectors ([TEST-VECTORS.json](spec/TEST-VECTORS.json)).

A developer building a conformant implementation in any language starts here.

### tessera-py/

Reference implementation of the VCR protocol in Python. Seven modules, ~1500 lines, one external dependency (`cryptography`). Includes a demo using real AWS Nitro Enclave attestation, a real ezkl Halo2 ZK proof, and a 25-test protocol validation suite.

```
pip install cryptography
cd tessera-py && python3 demo.py
```

### tessera-rust/

Conformant implementation in Rust. Produces identical `receipt_id` values for identical fields, verified against `spec/TEST-VECTORS.json`.

### tessera-network/

Network proof of concept. Three agents on separate ports with separate databases communicate only through HTTP. Demonstrates discovery, compute, cold verification, provenance DAG construction, ownership transfer with royalty cascade, and independent chain verification.

```
cd tessera-network && python3 poc.py
```

## The Protocol

21 fields. Three things at once:

**A proof of work performed.** Carries the ZK proof or TEE attestation, model ID, and verification key. Any stranger can verify the computation.

**A certificate of provenance.** Links to parent VCRs via cryptographic hash references, forming a DAG. Modify any receipt and all descendants break.

**A transferable economic instrument.** Carries pricing, royalty terms, and transfer history. When a VCR changes hands, royalties flow back through the provenance chain automatically.

## How It Relates to Existing Protocols

**Tool-use protocols** (MCP, APIs) solve integration. They connect agents to endpoints. The response is ephemeral — consumed and gone.

**Agent frameworks** (LangChain, CrewAI, AutoGen) solve orchestration within a single trust boundary.

**Agent communication** (A2A) solves discovery and message passing. It says nothing about whether the agent did what it claimed.

**Blockchain AI** (Bittensor, Ritual) requires a chain, a token, and consensus. They financialise the network itself.

**VCR** sits underneath all of these. It is a verification and trust primitive. No token, no chain, no middleman. The receipt is the economic object. The accumulated receipts are the trust.

## Architecture

```
Layer 4: Application      Agents, marketplace, network POC
Layer 3: Protocol          VCR schema, provenance, settlement, stake, royalties
Layer 2: Ownership         Transparency logs — append-only Merkle trees
Layer 1: Proving backend   ZK proof or TEE attestation — two functions
```

The proving backend abstraction is two functions:
```
prove(artifacts, input_data) → (proof_bytes, output_data)
verify(artifacts, proof_bytes) → bool
```

Everything above this interface is backend-agnostic.

MIT license.
