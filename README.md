# Agent Motivation

Your agent hit one error and quietly rewrote your task into something easier. This skill stops that.

Agent Motivation keeps an agent on the user's real target when uncertainty, missing examples, or repeated failures make easier familiar work tempting. It works with any agent or harness that loads skills — it is not tied to one tool.

It is not a pep-talk skill. It is motivation as operating discipline.

```text
An unknown task is not permission to swap the task.
Failure means learn, change method, try again.
No bluff.
```

## Install

```bash
npx skills add erdemuk/agent-motivation
```

Explicit skill name:

```bash
npx skills add erdemuk/agent-motivation --skill motivation
```

That is the whole setup. The skill activates on its own when a task turns unfamiliar, underspecified, or failure-prone.

## Why

Agents drift toward patterns they already know. You have seen the symptoms:

> "This doesn't seem to be supported, so I used a simpler approach instead."

> "The new API looks limited, so I fell back to the old integration path."

> "Done, should be good now."

This skill pushes the opposite behavior: inspect first, preserve constraints, try small probes, switch tactics after failure, and verify before claiming completion.

| Drift | Motivation |
| --- | --- |
| Swap the request for a familiar adjacent task | Hold the user's exact target |
| Say "unsupported" too early | Inspect docs, source, examples, logs, or real outputs |
| Repeat the same failed attempt | Keep the lesson, change tactic |
| Silently drop part of a multi-part request | Re-read the original message, check every sub-request |
| Claim "done" from confidence | Show test, screenshot, diff, log, response, or source evidence |

## How It Works

The skill encodes persistence as rules, not inspiration. "Be persistent" fades after a few turns; "write an attempt log after the second failure" does not, because an agent can count even when its judgment is drifting. The core mechanisms:

- **Route discipline** — before pivoting away from the requested approach, list what was asked, what was tried, what each attempt proved, and what remains untried. Change route only on evidence, never on comfort.
- **Escalation ladder** — first failure: fix the assumption. Second: mandatory attempt log, switch tactic. Third same external blocker: report it with evidence. Untried list empty: report and stop, without substituting an easier task.
- **Verify against the original message** — "done" is checked against what the user actually wrote, sub-request by sub-request, not against a reworded memory of it.
- **Safety valve** — persistence never justifies retrying destructive or irreversible actions harder. Those stop and ask.

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
Motivation: The same assertion failed twice, so the assumption is wrong. Attempt log written. Next: inspect the schema builder and trace where the message is composed.
```

### Premature Completion

```text
User: Make sure this actually works before saying it is done.
Drift: Done, should be good now.
Motivation: Validation passed with the targeted test. The diff only touches the requested files, and the failing case now returns the expected output.
```

## Prompt Snippets

Pair the skill with explicit instructions when a task is likely to trigger drift:

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
No. Repeated failure means switch tactic. Repeated external blockers, or an empty untried list, mean report the evidence and stop. And it never pushes the agent to retry destructive or irreversible actions.

**Does it conflict with other skills?**
No. It changes how the agent pursues a task, not which tools or skills it uses. It stacks with domain skills.

**What does it cost?**
One short file, loaded only when the task matches. Roughly a thousand tokens when active; nothing but the one-line description otherwise.
