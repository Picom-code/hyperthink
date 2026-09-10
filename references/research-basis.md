# Research basis for Hyperthink

Verified against live primary sources on 2026-09-10. This document separates published guidance/evidence from the skill's design choices. There is no established universal best topology or agent count; the scaling study's results depend on alignment between task structure and coordination.[4]

## Core decisions and supporting evidence

### Maximum useful parallelism, not maximum headcount at any cost

Anthropic reports that multi-agent research particularly benefits breadth-first, independently explorable tasks, while shared-context work and heavy dependencies can be poor fits. It recommends explicit effort scaling and clear divisions of responsibility.[3]

The revised scaling paper reports controlled comparisons across 260 configurations and six benchmarks, with relative performance changes ranging from gains on decomposable financial reasoning to losses on sequential planning. Its abstract identifies capability saturation, multi-agent overhead in tool-heavy tasks, and greater error propagation without centralized verification.[4]

**Design choice:** retain the user's aggressive fan-out preference—actively fill ready, substantive lanes—while treating coupling, duplication, throttling and the lack of independent work as reasons not to manufacture workers. Neither the paper nor vendor reports prove that this installation's maximum concurrency is its optimal count for every task.

### One accountable parent and explicit worker contracts

OpenAI distinguishes a manager that keeps ownership of the final response from a handoff that transfers conversational ownership; it advises narrow specialist jobs and warns that premature splitting adds complexity without necessarily improving the workflow.[5]

Anthropic's production guidance explicitly calls for each worker to receive an objective, output format, tool/source guidance and clear task boundaries, because vague requests produced duplicate research and gaps.[3]

**Design choice:** use native Hermes children as bounded helpers, with a parent-owned dependency/ownership map and compact evidence-return contracts. The worker-brief template encodes this while preserving the parent as integrator and final verifier.

### Context and artifact fidelity

Hermes children do not inherit the parent conversation; workspace project context may be embedded, but the parent must provide the relevant facts and instructions. Hermes supports optional JSON output contracts with validation and one bounded correction attempt; its docs advise requiring only fields the parent will read.[1]

Anthropic recommends persisted artifacts with lightweight references to avoid repeated lossy summarization and excess context copying.[3]

**Design choice:** pass minimum sufficient context, explicit relevant skill names, exact input references and past failure facts; persist large outputs in separately owned paths. Structured output is an interface contract, never evidence that a claim is true.

### Aggregate return context and recoverable state

Anthropic's context-engineering guidance separates focused worker contexts from the parent synthesis and recommends compaction, structured note-taking and subagents according to task characteristics.[8]

**Design choice:** budget aggregate worker returns rather than assigning every member of a large swarm a large report. Keep exact artifacts outside the chat, distinguish task/attempt identity and accepted state, and reload authoritative constraints after compaction. The article's illustrative summary lengths are not imposed as universal limits.

### Validate before propagation; bound recovery and refinement

Microsoft recommends validating agent output before passing it onward, surfacing errors, isolating shared resources, and setting a cap and fallback for checker/refinement loops.[6]

**Design choice:** a callback changes a task to returned, not accepted; dependent work starts only after input validation. Preserve accepted branches, recover only failed work and descendants invalidated by changed inputs, and bound execution retries separately from quality-refinement loops. These are task-local orchestration rules, not changes to Hermes runtime limits.

### Trust does not increase through delegation or JSON

OpenAI warns that untrusted text can affect downstream agent behavior and that structured output/isolation reduce but do not eliminate injection risk.[7]

**Design choice:** keep trusted worker instructions separate from source material, retain provenance through summaries, preserve existing approvals, and never treat a child's assertion as authorization. Prompt-scoped read-only instructions are not an enforced filesystem sandbox.

### Verify real requirements, not superficial review

The MAST study identifies failures involving system design, inter-agent alignment and verification. Its discussion gives an example where a reviewed chess program compiles but fails actual game-rule behavior; it explicitly warns that a verifier alone is not a silver bullet.[9]

**Design choice:** evaluate the actual integrated user outcome. Independent reviews are useful, but neither agreement nor a superficial compilation check replaces behavioral tests and source verification. Do not copy the study's small intervention results into a claim about this skill.

### Actual Hermes lifecycle, not copied SDK syntax

The live Hermes reference documents default consolidated batch completion, optional independently delivered groups, cancellation ownership, non-resumable child execution after a crash, child-process cleanup, and optional worktree isolation with fallback to a shared checkout.[1]

**Design choice:** the live tool schema wins over examples in web documentation. Do not add unsupported arguments, assume worktree isolation, treat queued steering as delivered, or claim a continuing server based on a child's PID. Finish independent work and yield for normal completion rather than polling.

### Outcome verification and controlled iteration

Anthropic recommends evaluating actual outcomes, starting with small representative cases, inspecting trajectories to find recurring failures, and checking the final state of mutating workflows rather than enforcing one predetermined sequence of steps.[3]

**Design choice:** the parent checks integrated artifacts and original evidence, handles uncertain external writes by readback before retry, and uses the regression scenarios in `references/evaluation.md`. A passed slash-loader or schema test is packaging verification, not a measured quality improvement.

## Evidence limitations

- The Anthropic article is a production engineering report with model/task-specific internal evaluations, not a universal optimality result.[3]
- OpenAI and Hermes docs specify design/runtime guidance; they do not experimentally validate this skill.[1][5]
- The scaling source cited here is explicitly **v3 (revised 8 April 2026)**. The parent verified its abstract directly; its high-level findings support task-dependent orchestration, not an exact worker-count rule. An independent research lane read v2; its older configuration counts, fit metrics and numerical heuristics were deliberately not carried into this skill's recommendations.[4]
- MAST provides empirical failure analysis and case-study interventions, not a controlled universal ranking of agent frameworks or a proof that a specific verifier topology is optimal.[9]
- Skill-specific choices such as a ready-work frontier, parent-owned tracker, exact file ownership and optional compact return schema are engineering synthesis. No speedup, cost reduction or accuracy gain is claimed for `/hyperthink` without a task-matched evaluation.
- Web extraction may silently return a partial or stale document. For runtime details, compare the live browser page with the installed schema, not just a search snippet.

## Sources

[1] https://hermes-agent.nousresearch.com/docs/user-guide/features/delegation
[3] https://www.anthropic.com/engineering/multi-agent-research-system — How we built our multi-agent research system
[4] https://arxiv.org/abs/2512.08296v3 — Towards a Science of Scaling Agent Systems (v3)
[5] https://developers.openai.com/api/docs/guides/agents/orchestration — OpenAI: Orchestration and handoffs
[6] https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns — Microsoft: AI agent orchestration patterns
[7] https://developers.openai.com/api/docs/guides/agent-builder-safety — OpenAI: Safety in building agents
[8] https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents — Anthropic: Effective context engineering for AI agents
[9] https://arxiv.org/html/2503.13657v3 — Why Do Multi-Agent LLM Systems Fail? (v3)
