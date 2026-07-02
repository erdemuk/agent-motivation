# Agent Motivation

The internal motivation skill for agents that start avoiding unfamiliar work.

AI Motivation keeps an agent on the user's real target when uncertainty, missing examples, or repeated failures make easier familiar work tempting.

It is not a pep-talk skill. It is motivation as operating discipline.

```text
Unknown task is not permission to swap task.
Failure means learn, change method, try again.
No bluff.
```

## Why

LLMs often drift toward patterns they already know. This skill pushes the opposite behavior: inspect first, preserve constraints, try small probes, switch tactics after failure, and verify before claiming completion.

| Drift | Motivation |
| --- | --- |
| Swap the request for a familiar adjacent task | Hold the user's exact target |
| Say "unsupported" too early | Inspect docs, source, examples, logs, or real outputs |
| Repeat the same failed attempt | Keep the lesson, change tactic |
| Claim "done" from confidence | Show test, screenshot, diff, log, response, or source evidence |

## Examples

### New Route, Not Legacy Route

```text
User: Build this with the new plugin API, not the old integration path.
Drift: The new API seems limited, so I used the old path.
Motivation: I inspected the new API docs and call sites. The old path is easier, but it does not satisfy the request. Next: a minimal probe against the new API.
```

### Repeated Failure

```text
User: Fix this validation failure.
Drift: I reran the same test after another small patch. It still fails.
Motivation: The same assertion failed twice, so the assumption is wrong. Next: inspect the schema builder and trace where the message is composed.
```

### Premature Completion

```text
User: Make sure this actually works before saying it is done.
Drift: Done, should be good now.
Motivation: Validation passed with the targeted test. The diff only touches the requested files, and the failing case now returns the expected output.
```

## Install

```bash
npx skills add erdemuk/agent-motivation
```

Explicit skill name:

```bash
npx skills add erdemuk/agent-motivation --skill motivation
```

Manual layout:

```text
skills/
`-- motivation/
    `-- SKILL.md
```

## Prompt Snippets

```text
Use the motivation skill. Do not replace the requested route with a familiar adjacent implementation unless evidence proves the requested route cannot work.
```

```text
If two attempts fail, keep the target and change the method. Explain what the failure taught you.
```

```text
Before claiming this is impossible, inspect docs, source, examples, logs, or live outputs and show the evidence.
```

## FAQ

**Is this anti-drift?**  
Related, but narrower. It targets familiarity drift: avoiding unfamiliar work by substituting known work.

**Does it motivate the user?**  
No. It motivates the agent through behavior: inspect, research, probe, change tactic, verify.

**Does it force endless retries?**  
No. Repeated failure means switch tactic. Repeated external blockers mean report the blocker and needed input.
