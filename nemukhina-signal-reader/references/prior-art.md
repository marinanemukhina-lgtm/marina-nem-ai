# Prior-Art Gate and Adopt/Adapt Map

Use this reference before proposing a new mechanism, architecture, formalism, or algorithm inside Nemukhina Signal Reader.

## Rule

Do not build a custom mechanism merely because it can be described from first principles. First ask whether an existing formalism or implementation already solves the relevant subproblem better.

Classify each candidate as:
- `ADOPT`: use the existing formalism directly as the conceptual basis;
- `ADAPT`: preserve its core mechanism but generalize the domain or interfaces;
- `COMBINE`: join two established mechanisms because the task spans both;
- `LEVERAGE`: use as a calibration, testing, or control layer;
- `REFERENCE`: keep as prior art/background, but do not import its machinery;
- `BUILD`: create a new mechanism only when a material gap remains after the prior-art check.

## Current map

### Higher-order belief and epistemic planning — ADOPT
Use established multi-agent epistemic planning / Dynamic Epistemic Logic for statements of the form:
- A knows X;
- A believes B knows X;
- actions change what agents know or believe;
- plans depend on higher-order belief states.

Relevant implementations:
- `FrancescoFabiano/deep` — Dynamic Epistemic Logic-based multi-agent planner with heuristic search.
- `sysulic/MEPK` — multi-agent epistemic planner based on higher-order belief change.
- `QuMuLab/pdkb-planning` — Proper Doxastic Knowledge Bases and epistemic planning compilation.

Do not recreate epistemic state-transition logic in prose when these formalisms fit.

### Epistemic calibration in LLM multi-agent planning — LEVERAGE
Use the EPC-AW pattern when execution can be correct but planning fails because agents are miscalibrated about who knows what.

Relevant implementation:
- `wzhSteve/EPC-AW` — repair framework for epistemic miscalibration in LLM-based multi-agent planning (ICML 2026).

Add an explicit check:
`Is the failure caused by wrong world-state inference, or by wrong beliefs about agent knowledge/capability?`

### User / counterpart mental modeling — ADAPT
Use ToM-SWE as prior art for persistent user modeling and consultation from interaction history.

Relevant implementation:
- `OpenHands/ToM-SWE` — three-tier memory, session analysis, user profiles, and agent consultation for software-engineering agents.

Adapt beyond software engineering. Do not assume its psychological profile is ground truth; preserve Signal Reader's provenance and uncertainty discipline.

### Strategic impression shaping — ADOPT + COMBINE
Use inverse planning to infer hidden goals from observed actions.
Use inverse-inverse planning when an agent may deliberately choose actions to shape an observer's inference.

Relevant implementation:
- `kach/acting-as-inverse-inverse-planning` — optimizes actions against a simulated inverse planner so the observer forms a desired interpretation.

Always consider both directions when incentives justify it:
`state/goal -> action -> observer inference`
and
`desired observer inference -> chosen action`.

### Causal analysis — ADAPT / LEVERAGE
Do not invent a bespoke causal engine when the task is standard causal discovery or intervention estimation.

Relevant implementation:
- `DMIRLAB-Group/CausalAgent` — conversational multi-agent causal-analysis pipeline, with data checks, causal structure learning, MCP-based algorithms, post-processing, and reporting.

Use Signal Reader to decide *what causal question to ask and whether the evidence is contaminated*; defer standard causal-estimation logic to established causal methods.

### Uncertainty-sensitive action / information gathering — ADOPT principle
Before acting, estimate uncertainty and decide whether to exploit the current best hypothesis or gather more information.

Use the general principle:
`act only if decision confidence is adequate; otherwise choose the lowest-cost, highest-value information-gathering move`.

Do not claim a novel exploration rule merely because it is expressed as a custom score.

## Build threshold

Create a new integration mechanism only if all of the following hold:
1. no established method covers the required object;
2. simple composition of established methods is insufficient;
3. the missing behavior is decision-relevant;
4. its inputs, outputs, failure modes, and evaluation can be specified;
5. the result is labeled as a working synthesis, not automatically as a novel theory.
