# Governance as Code — Automated Supplier Submission Review

A reference implementation that makes governance controls observable in a recurring supplier billing review.

## The engineering problem

A supplier sends a monthly billing submission. Someone has to establish that the arithmetic reconciles, that the rates match the approved rate card, that the period falls inside the contract's validity window, that the required service reports are attached, that the spending limit still holds and that this invoice has not already been paid — and then decide.

Automating the checking is easy. Automating it so that a reviewer can later establish *what happened and which control intervened* is the actual problem. That is what this implementation is about: the route through validation, evidence checking and human review is explicit, and the record of it is the product.

I built the workflow and a static, interactive showcase over it. The frontend connects four views — architecture, compliance mappings, assurance controls and recorded evidence — over the same executed traces.

## Inspect three recorded cases

Each file below is a trace emitted by the workflow executing a synthetic fixture. No event is hand-written, relabelled or reordered. Repeated exports over unchanged sources are byte-identical.

### 1. A correction loop with two review gates

An invoice arrives with a unit rate that does not match the approved rate card. The contract cross-check fails, the case suspends, and the reviewer requests a correction. Version 2 arrives, keeps the same case and invoice identity, and is revalidated **from the start** — every check runs again. The second gate records a different decision from the first.

[Read or download the correction trace](evidence/supplier_rate_correction.json)

Look for two `hitl.opened` events. The first is followed by `REQUEST_CHANGES`, the second by `APPROVE`. The replay on the showcase pauses at both, and labels each with the decision recorded at *that* gate rather than the run's final one.

### 2. Correct figures, invented story

A proposal reports a real 30% increase against the prior period — the numbers verify exactly — and attaches a fabricated account of events: that the supplier admitted overbilling on a call.

[Read or download the invented-narrative trace](evidence/supplier_invented_narrative.json)

The verifier re-derives the observation, confirms it is bound to this invoice, and then **rebuilds the finding text from that evidence**. The proposal's narrative does not survive into the review package or the supplier clarification draft. Checking a structured number is not the same as verifying the prose that arrived with it; a verifier that only checked the number would have passed the story through.

### 3. A case that does not complete

The workflow reaches the reviewer gate and stops. No decision is recorded, nothing is archived, and the terminal state stays `Awaiting_reviewer`.

[Read or download the pending trace](evidence/supplier_pending.json)

The showcase's replay control for this case reads *"Continue — no reviewer decision in this trace"*. A visitor replaying a recording is never offered wording that implies a reviewer already acted.

## What I built

- **Typed boundary:** frozen Pydantic models, `extra="forbid"`, Decimal money with an explicit rounding policy. Malformed input fails before any check runs and never becomes a case.
- **Deterministic validation:** six invoice checks (line arithmetic, total reconciliation, service period, invoice chronology, code uniqueness, required documents) and eight contract cross-checks (supplier, contract, purchase order, currency, validity window, rate card, spending limit, duplicate register).
- **Bounded proposals:** at most four structured proposals may enter verification. A proposer that exceeds its budget, or raises, forfeits all of them — and the case still reaches a reviewer.
- **Evidence verification:** only case-bound computed observations are admitted, and an admitted finding is restated from that evidence.
- **Human authority:** an explicit `Awaiting_reviewer` state and an allowlist. A submission with hard check failures cannot be approved at all — it must be corrected or rejected.
- **Traceability:** ordered immutable event records, provenance on every export, and a generated state catalog that a test keeps in sync with the code.
- **Assurance:** executed control probes covering supplier instructions in free text, unauthorised reviewers, unsupported charges, invented narrative, smuggled privilege fields, over-budget proposals and a failing adapter.

## Evidence scope — what these traces do and do not show

The JSON files are copies of the showcase's exported traces. Each carries its source reference and provenance.

- `ts_rel` is an **ordinal event position**, not measured elapsed time.
- The AI roles are **scripted stand-ins**. The demo makes no live model call, so it demonstrates a control under the fixture's conditions and measures nothing about a live model's reliability.
- Mail, contract access, the duplicate register and the archive are **in-memory fixture adapters**. There is no mailbox, ERP, authentication or durable storage integration, and no Excel file is parsed.
- The instruction-pattern detector carries **two signatures**. It is an illustration of quarantine, not a comprehensive prompt-injection detector. The control that actually holds is structural: supplier free text never reaches the proposal adapter or the decision.
- Test and scenario counts measure **this reference runner only** — not any larger private codebase.
- A decision approves or rejects a submission for downstream processing. **It never executes a payment.**

The source implementation is maintained in a private repository. The traces here make specific behaviours inspectable without implying that the full source is publicly available. This is a reference implementation, with no claim of production deployment, regulatory classification or compliance certification.

[Back to my profile](README.md) · [Discuss the work](mailto:agh961235@gmail.com)
