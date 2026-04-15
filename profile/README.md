Your agent calls another agent... something comes back. 

> Was it actually Opus 4.6 or did they run 4.0 and pocket the difference? Did the model even run? 

You have no idea, nor does it. 

The response is consumed and gone - no receipt, no proof, no way for anyone else to check what happened.

Weird, right? Every other economic transaction in history has a paper trail. However, AI compute doesn't.

VCR is a spec for that missing receipt. 21 fields that wrap a cryptographic proof in an economic envelope - what model ran, what went in, what came out, when, and who did it. Any stranger can verify it. Receipts chain into provenance graphs through hash links, so tamper with one and everything downstream breaks. Your history of verified work becomes your reputation. No token, blockchain or middleman.



I wrote a formal spec, built it in Python and Rust (both produce identical hashes for identical inputs — that's the point), and put together a network POC with agents actually transacting over HTTP.



Curious whether this resonates with anyone or if I'm solving a problem that doesn't exist yet. Either answer is useful.



## Repositories

| Repository | Description |
|---|---|
| [spec](https://github.com/tesseravcr/spec) | The formal spec, whitepaper, and test vectors. Start here. |
| [tessera-py](https://github.com/tesseravcr/tessera-py) | Reference implementation. Seven modules, ~1500 lines, one dependency. |
| [tessera-rust](https://github.com/tesseravcr/tessera-rust) | Conformant Rust implementation. Proves the spec is language-agnostic. |

## The proving backend is two functions

```
prove(artifacts, input_data) -> (proof_bytes, output_data)
verify(artifacts, proof_bytes) -> bool
```

Everything else is backend-agnostic. ZK proofs, TEE attestation, whatever comes next — the receipt doesn't care.

## Where it sits

MCP and APIs solve integration. LangChain and CrewAI solve orchestration. A2A solves discovery. Bittensor and Ritual require a chain and a token.

None of them answer: "did the agent actually do what it claimed?" That's what this is for.

MIT license.
