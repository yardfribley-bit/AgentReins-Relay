# Architecture

## Product boundary

AgentReins Relay is an evidence and assessment engine. It accepts observations
from AgentReins collectors or an explicit diagnostic probe and returns a local,
machine-readable provider-trust report. It does not own the desktop UI and does
not silently redirect user traffic.

## Evidence levels

Every conclusion must carry one of three confidence levels:

- `observed`: directly present in local process, request, DNS, socket, or TLS evidence.
- `inferred`: derived from multiple observations with an explanation.
- `verified`: independently checked against an authoritative or cryptographic source.

Absence of evidence must remain `unknown`; it must never be converted into a
safe result.

## Core pipeline

```text
Request metadata / captured context / network evidence
                         |
                         v
                Evidence normalization
                         |
          +--------------+---------------+
          |              |               |
          v              v               v
   Route identity   Exposure scan   Claim consistency
          |              |               |
          +--------------+---------------+
                         |
                         v
             Explainable trust assessment
                         |
                         v
                  Versioned JSON report
```

## First report contract

```json
{
  "schemaVersion": 1,
  "assessmentId": "uuid",
  "agent": "workbuddy",
  "turnId": "turn-id",
  "route": {
    "endpoint": "https://relay.example/v1/chat/completions",
    "classification": "unverified_relay",
    "ip": "203.0.113.10",
    "asn": "AS64500",
    "networkOwner": "Example Hosting",
    "country": "SG",
    "confidence": "observed"
  },
  "identity": {
    "claimedProvider": "OpenAI",
    "claimedModel": "gpt-5",
    "verifiedProvider": null,
    "verifiedModel": null,
    "confidence": "unverified"
  },
  "exposure": {
    "categories": ["prompt", "source_code", "memory", "tool_arguments"],
    "secretFindings": 1,
    "contentRetainedLocally": true
  },
  "result": {
    "level": "high",
    "reasons": ["An unverified relay received source code and memory."],
    "limitations": ["The upstream model identity could not be independently verified."]
  }
}
```

## Milestone 1 acceptance

- Direct official-provider traffic is classified using reproducible evidence.
- OpenRouter is identified as a known router, not mislabeled as an unknown relay.
- A custom OpenAI-compatible endpoint remains unverified until evidence proves otherwise.
- Context categories and secret findings are attached to the same agent turn.
- Raw evidence references and assessment reasons are included in every report.
- The same fixtures produce deterministic results on repeated runs.

