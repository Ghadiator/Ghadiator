# Ali Ghader

**AI governance and assurance, grounded in functional safety.**

I build systems where AI proposals pass through explicit checks, human review gates and an auditable decision trail. My focus is turning governance requirements into mechanisms that can be inspected and tested.

My background is functional safety. I bring that discipline to consequential AI: define the boundary, enforce the control, and retain the evidence.

## Featured work · Automated Supplier Submission Review

A reference implementation that reviews recurring service-provider billing submissions against an approved contract, rate card, purchase order, supporting documents and spending limit — and makes every control on that path inspectable.

The workflow runs end to end: an authorised buyer opens a document request, deadline reminders are prepared, a submission is parsed into a typed model and linked to prior versions, deterministic checks run, figures are cross-checked against the approved contract, unusual cases enter a bounded proposal step whose findings must survive verification, and a **named reviewer** approves, rejects or requests a correction. A corrected version keeps its identity, increments its version and is revalidated from the start.

| Governance question | What the implementation demonstrates |
| --- | --- |
| What constrains the workflow? | Frozen Pydantic models with `extra="forbid"`, Decimal money, an explicit state catalog and a closed transition table. |
| Can a proposal proceed without evidence? | Fourteen deterministic checks run before any proposal step, and a verifier admits only case-bound computed findings. |
| Can correct numbers carry an invented story? | No. A verified finding is **restated from the checked evidence**; the proposal's own wording is discarded. |
| Where does a person intervene? | The run suspends in an explicit `Awaiting_reviewer` state for an allowlisted reviewer. Hard check failures cannot be approved at all. |
| Can a decision be reconstructed? | Ordered immutable event records, replayable traces, and provenance on every exported trace. |

**[Open the live showcase](https://ghadiator.github.io/)** · **[Read the case study](governance-as-code.md)**

The showcase replays the workflow across its connected architecture, pausing at every human gate. Each run below is selectable there under **Recorded run**, and its executed trace is served live:

| Recorded run | What it shows | Trace |
| --- | --- | --- |
| Incorrect rate → correction → approval | Two review gates, each with its **own** recorded decision — `REQUEST_CHANGES`, then `APPROVE` after the corrected version is revalidated from the start | [live](https://ghadiator.github.io/data/scenarios/supplier_rate_correction.json) · [repo copy](evidence/supplier_rate_correction.json) |
| Invented narrative replaced by evidence | A proposal carries correct figures and a fabricated account. The verifier keeps the checked numbers and **restates the finding**; the invented wording reaches neither the review package nor the supplier draft | [live](https://ghadiator.github.io/data/scenarios/supplier_invented_narrative.json) · [repo copy](evidence/supplier_invented_narrative.json) |
| Review gate holds an unresolved case | Terminal state stays `Awaiting_reviewer`. No decision recorded, nothing archived — and the replay offers no wording implying a reviewer acted | [live](https://ghadiator.github.io/data/scenarios/supplier_pending.json) · [repo copy](evidence/supplier_pending.json) |

The live traces are what the deployed site itself reads; the repo copies are byte-identical snapshots. All thirteen recorded runs are in the [live catalog](https://ghadiator.github.io/data/scenarios/index.json).

### What actually runs, and what is a fixture

Everything above executes: the state machine, the arithmetic, the contract cross-checks, the verifier, the reviewer gate and the correction loop, covered by a test suite whose pass count is tied to a digest of the sources it ran against.

These are **fixtures, not integrations** — the demo is hermetic and makes no network call:

- The proponent, opponent and Judge roles are **scripted stand-ins**. No live model is called, so nothing here measures model behaviour.
- Contract snapshots, the duplicate-invoice register, the reminder outbox and the archive are **in-memory**. There is no ERP, mailbox, or durable storage integration.
- The reviewer allowlist is a **reference authorization check**, not a production login system.
- Approval accepts a submission for downstream processing. **No payment is ever executed.**
- Framework mappings (EU AI Act, ISO/IEC 42001, NIST AI RMF) are conceptual engineering references. They are not a conformity assessment, a certification, or a claim that this use case is high-risk under Annex III.

**Built with:** Python · Pydantic · pytest · TypeScript · React / Next.js

The engineering source is maintained in a private repository. The traces published here make specific behaviours inspectable without implying the full source is publicly available.

## What I'm working on

- Making governance visible through interactive architecture and evidence views.
- Testing failure modes: unsupported model claims, invented narrative, unauthorised reviewers and adapter failures.
- Connecting functional-safety thinking with practical AI assurance.

I'm interested in **AI governance, AI assurance and responsible AI engineering roles** where I can work across requirements, implementation and verification.

[Get in touch](mailto:agh961235@gmail.com)
