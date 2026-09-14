# AgentReins Relay

**Know who receives your AI context.**

AgentReins Relay is the provider-trust and AI gateway security engine for the
[AgentReins](https://github.com/yardfribley-bit/AgentReins) product family. It is
designed to identify the endpoint that receives an agent's model request,
preserve the supporting network evidence, explain what context was exposed,
and distinguish verified facts from informed conclusions.

## Why it exists

AI coding tools may connect directly to a model provider, use a trusted routing
service, or send prompts and source code through an unknown third-party relay.
An endpoint can claim to serve a particular model without giving the user enough
evidence to verify that claim. It can also retain prompts, memory, source code,
credentials, tool arguments, and uploaded files.

AgentReins Relay aims to answer:

- Which endpoint received the request?
- Is it an official provider, a known router, or an unverified relay?
- Who owns the destination infrastructure and where is it located?
- What prompt, code, memory, skills, files, and secrets were exposed?
- Does the claimed provider or model conflict with observable evidence?
- Which findings are observed, inferred, or independently verified?

## Initial scope

1. Endpoint discovery and route classification.
2. TLS certificate, DNS, IP, ASN, location, and infrastructure evidence.
3. Official-provider and known-router identification.
4. Context-exposure inventory with local secret detection.
5. Claimed-provider and claimed-model consistency checks.
6. Stable JSON reports for AgentReins Desktop and command-line integrations.

AgentReins Relay will not claim that encrypted network metadata alone can prove
which upstream model generated a response. Unverifiable model identity remains
explicitly marked as **unverified**.

## Integration model

```text
AgentReins Desktop
        |
        | local JSON request
        v
AgentReins Relay
  |- Endpoint evidence
  |- Provider identity
  |- Context exposure
  |- Model-claim consistency
  `- Risk report with confidence
```

The engine is intended to run locally. AgentReins remains responsible for agent
discovery, turn correlation, evidence presentation, and user decisions.

## Status

This repository is at the architecture and evidence-contract stage. The first
milestone is one reproducible end-to-end test covering a direct provider, a
known router such as OpenRouter, and an unverified OpenAI-compatible relay.

See [Architecture](docs/ARCHITECTURE.md) for the trust model and first delivery
boundary.

See [Research Map](docs/RESEARCH.md) for the relay-security and black-box model
fingerprinting papers guiding the implementation.

## AgentReins family

- **AgentReins Trace** — understand what an agent did.
- **AgentReins Relay** — understand who received model context.
- **AgentReins Memory** — understand what an agent remembered or retrieved.
- **AgentReins Code** — independently inspect generated code.
- **AgentReins Web** — inspect external content used by agents.
- **AgentReins Verify** — verify whether the requested outcome was achieved.
