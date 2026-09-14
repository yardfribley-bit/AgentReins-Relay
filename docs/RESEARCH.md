# Research Map

This reading list tracks research directly relevant to detecting and assessing
third-party LLM relays, resellers, shadow APIs, and hidden upstream models.

## Priority 0: relay and reseller security

### Real Money, Fake Models: Deceptive Model Claims in Shadow APIs

- arXiv: https://arxiv.org/abs/2603.01919
- Direct relevance: systematic audit of official APIs and shadow APIs.
- Reported findings: performance divergence, inconsistent safety behavior, and
  identity-verification failures among services claiming premium models.
- AgentReins Relay use: threat model, benchmark design, and evidence categories
  for claimed-model inconsistency.

### KBF: Knowledge Boundary as Fingerprint for Language Model and Black-Box API Auditing

- arXiv: https://arxiv.org/abs/2605.29524
- Direct relevance: low-cost black-box auditing of reseller substitutions.
- Reported findings: detects economically meaningful substitutions and some
  mixed-routing behavior where only a fraction of traffic is substituted.
- AgentReins Relay use: active model-consistency probe candidate.

### The Proxy Knows Too Much: Sealing LLM API Routers with Attested TEEs

- arXiv: https://arxiv.org/abs/2606.16358
- Direct relevance: explains why a plaintext router can inspect or alter prompts,
  tool calls, dependencies, responses, and secrets.
- AgentReins Relay use: relay threat model, integrity checks, exposure reporting,
  and a future attestation design.

### Uncovering and Understanding Hidden Dependencies in the LLM API Reseller Ecosystem via Prefix-Cache Side Channels

- arXiv: https://arxiv.org/abs/2608.20732
- Direct relevance: API-only measurement of hidden dependencies among resellers.
- Reported study: 39 endpoints, 636 endpoint pairs, and multi-level shared cache
  reach that reveals concentrated upstream infrastructure.
- AgentReins Relay use: research basis for an explicitly authorized upstream
  dependency experiment. This technique is too request-intensive for default
  client-side use and requires a strict legal, cost, and rate-limit policy.

## Priority 1: black-box model identity

### LLMmap: Fingerprinting For Large Language Models

- arXiv: https://arxiv.org/abs/2407.15847
- Direct relevance: active identification of hidden models behind applications.
- Reported result: more than 95% accuracy across 42 model versions with as few as
  eight interactions in the authors' evaluation.
- AgentReins Relay use: baseline fingerprint suite and evaluation methodology.

### AdaptPrint: Response-Adaptive Fingerprinting of Black-Box LLM Services

- arXiv: https://arxiv.org/abs/2608.22213
- Direct relevance: adaptive probes designed for unknown system prompts and
  decoding configurations.
- Reported result: 80.6% Top-1 and 90.3% Top-3 accuracy across 27 candidates.
- AgentReins Relay use: escalation path after inexpensive passive checks fail.

### Black-Box Forensics for Conversational LLM Agents

- arXiv: https://arxiv.org/abs/2606.22698
- Direct relevance: attributes a base model and compares hidden system-prompt
  behavior through ordinary multi-turn conversations.
- AgentReins Relay use: evidence that normal user traffic may contribute to a
  model-family assessment without injecting obvious adversarial probes.

### Fingerprinting LLMs via Prompt Injection

- arXiv: https://arxiv.org/abs/2509.25448
- Direct relevance: robust model-origin fingerprints across post-training and
  quantized variants.
- AgentReins Relay use: research reference only until safety, authorization, and
  prompt-side effects are understood; it should not be a default live probe.

## Priority 2: limits and negative evidence

### Token Counts Are Not Model Lineage

- arXiv: https://arxiv.org/abs/2608.29930
- Direct relevance: evaluates prompt-token counts as a cheap API fingerprint.
- Key limitation: token-count consistency may identify a shared tokenization
  stack, but it is not sufficient evidence of model-family lineage.
- AgentReins Relay use: token counts may be one signal, never the final verdict.

## Engineering conclusions

The first implementation should combine multiple independent evidence groups:

1. Passive route identity: endpoint, DNS, TLS, IP, ASN, owner, and location.
2. Protocol behavior: response headers, errors, streaming format, usage fields,
   tool-call shape, supported parameters, and model-list behavior.
3. Context exposure: prompt, code, files, memory, skills, credentials, and tool
   arguments delivered to the endpoint.
4. Low-cost consistency probes: replayable reference queries with declared cost.
5. Adaptive fingerprinting only after user authorization.
6. Longitudinal evidence: endpoint changes, claimed-model changes, and mixed
   routing across repeated observations.

No individual fingerprint may produce a `verified` model identity. A result is
`verified` only when backed by authoritative or cryptographic evidence. Behavioral
fingerprints produce probabilistic `inferred` conclusions with limitations.

