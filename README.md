# AI Guardrails

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

AI Guardrails are the runtime and design-time controls that screen the inputs and outputs of LLM-backed applications and agents. They detect and block prompt injection, jailbreak attempts, PII leakage, toxic or unsafe content, hallucinations, and policy violations — and they validate structured outputs against schemas. This API Evangelist topic repository catalogs the guardrails landscape, defines a vendor-neutral vocabulary and JSON Schema for policies and violations, and provides a JSON-LD context aligning the domain with OWASP, NIST, and MITRE references.

## The Landscape

The guardrails landscape splits along three axes.

### Input vs Output

- **Input guardrails (input rails)** inspect what reaches the model: prompt injection detection, jailbreak detection, PII redaction, denied-topic enforcement on the request side.
- **Output guardrails (output rails)** inspect what the model produced: content-safety filtering, hallucination / contextual-grounding checks, structured-output schema validation, secret-leakage prevention.
- **Retrieval, dialog, and execution rails** (a taxonomy popularized by NVIDIA NeMo Guardrails) cover RAG chunks, multi-turn dialog flow, and tool/agent action invocations.

### Provider-Native vs Third-Party vs Open Source

- **Provider-native** — guardrails coupled to a foundation-model platform:
  - Amazon Bedrock Guardrails (content filters, denied topics, PII filters, contextual grounding, automated reasoning checks)
  - Microsoft Azure AI Content Safety — Prompt Shields (user prompt + document attacks)
  - Google Cloud Model Armor (prompt injection, jailbreak, PII, malicious URLs, harmful content)
  - OpenAI Moderation API (`omni-moderation-latest`, free, multimodal)
- **Third-party vendors** — independent control planes spanning providers:
  - Lakera AI (Lakera Guard runtime API, Gandalf red-team game)
  - HiddenLayer (Discovery, Supply Chain, Attack Simulation, Runtime Security)
  - Cisco AI Defense (the Robust Intelligence acquisition)
  - Lasso Security (Intent Security, AI-BOM, Red Teaming, Runtime Enforcement)
  - PromptArmor (TPRM/GRC for AI; published research on indirect prompt injection in Claude for Excel, Google Antigravity, Slack AI, Writer.com)
  - Wallarm AI Security (API-security platform with OWASP LLM Top 10 coverage)
- **Open source** — libraries and frameworks teams self-host:
  - Guardrails AI (validator framework + Hub)
  - NVIDIA NeMo Guardrails (Colang + 5 rail types)
  - Confident AI's DeepEval and DeepTeam

### Runtime vs Design-Time

- **Runtime guardrails** sit in the request path and block/redact/transform in real time.
- **Design-time guardrails** scan models, datasets, and prompts before release — red teaming, vulnerability scanning, model BOM, evaluation, CI/CD gates.

A complete safety posture typically combines both: a design-time evaluation suite (DeepEval, Lakera Red, HiddenLayer Attack Simulation) feeding test cases into a runtime stack (Guard, Prompt Shields, Bedrock Guardrails, Model Armor).

## What's in This Repo

| Path | Purpose |
|---|---|
| `apis.yml` | apis.json 0.19 index cataloging every vendor and open-source project covered, with deployment type and threat categories. |
| `json-schema/guardrail-policy-schema.json` | Vendor-neutral JSON Schema for a guardrail policy: identifier, version, scope, rules (direction, category, detector, severity, action). |
| `json-schema/guardrail-violation-schema.json` | Vendor-neutral JSON Schema for a single violation event. |
| `json-ld/guardrails-context.jsonld` | JSON-LD context aligning the vocabulary with `schema.org`, `dcterms`, Hydra, and external references to OWASP LLM Top 10, NIST AI RMF, and MITRE ATLAS. |
| `vocabulary/guardrails-vocabulary.yml` | Domain vocabulary: core concepts, directions, threat categories, severities, actions, deployment patterns, vendor taxonomy, standards references. |
| `examples/guardrail-policy-example.json` | A sample multi-vendor policy combining Lakera, Azure Prompt Shields, AWS Bedrock contextual grounding, OpenAI Moderation, and a JSON Schema check. |
| `examples/guardrail-violation-example.json` | A sample violation event emitted when an input prompt-injection rule fired. |

## Vendors and Projects Cataloged

| # | Name | Deployment | Threat Focus |
|---|---|---|---|
| 1 | Guardrails AI | SDK | Structured output, PII, toxic language, jailbreak |
| 2 | NVIDIA NeMo Guardrails | SDK | Input/output/dialog/retrieval/execution rails |
| 3 | Lakera AI | API | Prompt injection, PII, multilingual content safety |
| 4 | Microsoft Azure Prompt Shields | Cloud Service | Jailbreak, indirect prompt injection (document attacks) |
| 5 | Amazon Bedrock Guardrails | Cloud Service | Content filters, denied topics, PII, contextual grounding |
| 6 | OpenAI Moderation API | API | Hate, harassment, self-harm, sexual, violence (free, multimodal) |
| 7 | Google Cloud Model Armor | Cloud Service | Prompt injection, PII, harmful content, malicious URLs |
| 8 | HiddenLayer | Platform | Adversarial ML, model supply chain, runtime security |
| 9 | Cisco AI Defense (formerly Robust Intelligence) | Platform | Model validation, algorithmic red teaming, runtime |
| 10 | Lasso Security | Gateway | Intent security, AI-BOM, runtime enforcement |
| 11 | PromptArmor | Platform | Vendor risk, indirect prompt injection research |
| 12 | Wallarm AI Security | API Gateway | OWASP LLM Top 10, API abuse |
| 13 | Confident AI | SDK | Evaluation, red teaming (DeepEval / DeepTeam) |
| 14 | Layerup AI | Platform (historical) | Pivoted to agentic insurance workflows |

## Threat Categories

The vocabulary normalizes the categories used across vendors:

- `prompt-injection`
- `indirect-prompt-injection`
- `jailbreak`
- `pii` / `sensitive-information`
- `content-safety` (hate, harassment, self-harm, sexual, violence)
- `hallucination` / `contextual-grounding`
- `denied-topic`
- `competitor-mention`, `profanity`, `toxic-language`
- `malicious-url`
- `data-exfiltration`
- `structured-output` (schema/regex/grammar violation)
- `tool-misuse`
- `agent-goal-hijack`

Each maps to its closest analog in OWASP LLM Top 10, NIST AI RMF, and MITRE ATLAS via the JSON-LD context.

## Standards and References

- [OWASP Top 10 for Large Language Model Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- [MITRE ATLAS](https://atlas.mitre.org/)
- [Azure AI Content Safety — Prompt Shields](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/concepts/jailbreak-detection)
- [Amazon Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
- [Google Cloud Model Armor](https://docs.cloud.google.com/security-command-center/docs/model-armor-overview)
- [OpenAI Moderation API](https://platform.openai.com/docs/guides/moderation)
- [Guardrails AI](https://www.guardrailsai.com/)
- [NVIDIA NeMo Guardrails](https://docs.nvidia.com/nemo/guardrails/)

## Maintainer

Kin Lane — [API Evangelist](https://apievangelist.com)
