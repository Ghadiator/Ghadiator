# Ali Ghader

**AI governance and assurance, grounded in functional safety.**

I build systems where AI proposals pass through explicit checks, human review gates and an auditable decision trail. My focus is turning governance requirements into mechanisms that can be inspected and tested.

My background is functional safety. I bring that discipline to consequential AI: define the boundary, enforce the control, and retain the evidence.

## Featured work · Governance as Code

An independently built reference implementation for governed submission review. It connects a Python validation workflow to an interactive frontend that replays recorded cases and exposes the controls behind each decision.

| Governance question | What the implementation demonstrates |
| --- | --- |
| What constrains the workflow? | Typed contracts, explicit workflow states and configurable validation rules. |
| Can an AI claim proceed without evidence? | A procedural verification layer checks support for proposed findings. |
| Where does a person intervene? | A human review gate with recorded pause and resume events. |
| Can a decision be reconstructed? | An append-only event trail, replayable traces and fixture provenance. |

**[Explore the case study](governance-as-code.md)** · [Human review trace](evidence/hitl_pause_resume.json) · [Unsupported-claim trace](evidence/judge_bluff.json)

The showcase connects obligations, controls and evidence using EU AI Act, ISO/IEC 42001 and NIST AI RMF concepts. These are scoped engineering mappings; the reference implementation is not a compliance certification.

**Built with:** Python · Pydantic · LangGraph · FastAPI · TypeScript · React / Next.js · pytest

## What I’m working on

- Making governance visible through interactive architecture and evidence views.
- Testing failure modes, including unsupported model claims and human-review boundaries.
- Connecting functional-safety thinking with practical AI assurance.

I’m interested in **AI governance, AI assurance and responsible AI engineering roles** where I can work across requirements, implementation and verification.

[Get in touch](mailto:agh961235@gmail.com)
