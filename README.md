# Hyperthink

**Maximum useful parallel Hermes agents, with one accountable orchestrator.**

`/hyperthink` is a [Hermes Agent](https://hermes-agent.nousresearch.com/docs/) skill that tells the current agent to aggressively use available native subagent capacity for the task, then integrate and verify the results.

Version: **2.1.0**

## Use

```text
/hyperthink research the tradeoffs between these architectures
/hyperthink implement this feature and verify the integrated result
```

Use `/hyperthink` alone to apply it to the current unfinished task.

## What it does

- Fills available capacity with substantive, independent work rather than defaulting to a small team.
- Schedules by dependencies and validates prerequisites before downstream work begins.
- Gives workers compact contracts: context, ownership, acceptance criteria, evidence, and stop conditions.
- Verifies write isolation and centralizes integration of shared resources.
- Budgets combined return context and preserves detailed artifacts separately.
- Uses native completion delivery, bounded recovery, and evidence-based final checks.

It does **not** change your concurrency limits, model/provider, approvals, or configuration. It does not bypass limits through recursive agents or separate processes. Trivial lookups stay direct tool calls, and tightly coupled work retains a single owner.

This is an instruction skill, not an orchestration plugin or an enforced sandbox. The live Hermes tool schema determines available capabilities. No fixed agent count or universal performance improvement is claimed.

## Install

Clone with GitHub CLI (authentication is required while the repository is private):

```bash
gh repo clone Picom-code/hyperthink
```

Copy `SKILL.md`, `templates/`, and `references/` into one `hyperthink/` directory under your **active profile's skills directory**, preserving the layout:

```text
<active-hermes-home>/skills/hyperthink/
├── SKILL.md
├── templates/
│   ├── worker-brief.md
│   └── result-schema.json
└── references/
    ├── research-basis.md
    └── evaluation.md
```

The default Hermes home is `~/.hermes/`; named profiles normally use `~/.hermes/profiles/<profile>/`. If Hyperthink is already installed, update its existing directory instead of creating a duplicate. Do not copy the repository's `.git/` directory into the skill.

Start a fresh chat, or use `/reload-skills` on surfaces that support it, then invoke `/hyperthink`.

## Files

| File | Purpose |
| --- | --- |
| [SKILL.md](SKILL.md) | Instructions loaded by `/hyperthink` |
| [Worker brief](templates/worker-brief.md) | Reusable delegation contract |
| [Result schema](templates/result-schema.json) | Optional JSON return contract |
| [Research basis](references/research-basis.md) | Primary sources, design rationale, and evidence limits |
| [Evaluation scenarios](references/evaluation.md) | Behavioral regression cases and evaluation guidance |

## Validation

The installed skill was checked through Hermes's native slash-command discovery and invocation path. Its JSON output schema was validated with valid and invalid fixtures; source citations were checked; independent instruction reviews found no material issues. These checks are not an end-to-end quality or speed benchmark. The evaluation scenarios describe expected behaviors, not claimed benchmark results.

Only the distributable skill and repository documentation are included here. Local profile configuration, credentials, research caches, and agent transcripts are excluded.
