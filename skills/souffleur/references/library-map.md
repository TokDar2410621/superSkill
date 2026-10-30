# Prompt library map

Summary of Anthropic's official Claude Code prompt library
([code.claude.com/docs/en/prompt-library](https://code.claude.com/docs/en/prompt-library)),
52 prompts in 15 groups, verified 2026-10-30. Every description below is
a paraphrase of what the group covers. Fetch the exact prompt text from
the official page at use time; do not reconstruct prompts from memory.

The page also filters prompts by role (Product, Design, Docs, Marketing,
Security, On-call). This map follows the phase and group headings.

## Discover

### Onboard (1 prompt)

Get oriented in a repository you do not know yet: architecture, key
directories, how the pieces connect. Follow up by recording the result
so the agent remembers it every session.

### Understand (6 prompts)

Make sense of existing code: explain an unfamiliar file and its data
flow in a chosen format; locate where a behavior is implemented by
describing the behavior, not the filename; check what would break
before deleting something; trace how a file evolved through its commit
history and why; size a change by listing the files it would touch;
answer a product question from the source code, pitched at the asker's
role.

## Design

### Plan (4 prompts)

Shape work before code moves: plan a multi-file change as a file list
with no edits yet; draft a spec through an interview that covers
implementation, UX, edge cases, and tradeoffs, then writes the spec to
a file; turn meeting notes into tracker tickets with acceptance
criteria; list error states, empty states, and edge cases a happy-path
design tends to skip.

### Prototype (2 prompts)

Go from image to working code: build a clickable prototype from a
mockup, matching layout and states; implement a design from a
screenshot, then render, compare against the original, and fix the
differences in a loop.

## Build

### Implement (7 prompts)

Write code the codebase's way: follow an existing implementation
pattern for a new piece; add missing documentation matching the style
already in the file; add a small feature defined by its inputs and
outputs; build a small standalone tool from scratch and open it;
work an issue end to end from its number, including tests; find and
update wording everywhere it appears, variants included, while leaving
tests and history alone; draft a document by learning structure and
voice from a folder of past examples.

### Test (3 prompts)

Close the loop on tests: write tests, run them, and fix failures in one
go; drive implementation from tests written first; fill coverage gaps
by reading the coverage report and targeting the lowest-covered files
up to a stated threshold.

### Refactor (4 prompts)

Change code at scale with a definition of done: migrate a whole pattern
to a new API, listing every affected call site first; port a module to
another language while preserving its public API and test behavior;
optimize against a stated metric and target value; fix a precisely
described visual bug (element, measurement, viewport).

### Review (5 prompts)

Catch problems before they cost more: review uncommitted changes
reading full files, not just the diff; review a PR with the whole
codebase in context; summarize a Terraform plan in plain language and
flag what could cause problems before applying; run a security review
in a subagent so the main session stays clean; pre-check a document
against a named list of concerns before it goes to a human reviewer.

### Steer (3 prompts)

Correct the agent without restarting: name the exact constraint it
missed and ask for a different approach; narrow an over-broad change
to a stated scope and undo the rest; turn a repeated correction into
a written rule so it stops happening.

## Ship

### Git (3 prompts)

Move changes through version control: resolve merge conflicts and
explain what was kept from each side; generate a commit message from
the diff, matching the repo's existing style; open a PR that implements
a ticket found in the issue tracker.

### Release (2 prompts)

Prepare a release: draft release notes from git history between two
references, grouped by feature, fix, and breaking change; write a CI
workflow from its trigger and its steps, matched to the project's build
and test commands.

## Operate

### Debug (3 prompts)

Start from symptoms: find why a named test fails and fix it; investigate
a reported error at a named location, with logs or stack traces pasted
in; fix a build error at its root cause and verify the build succeeds.

### Incident (3 prompts)

Work production problems with evidence: correlate logs, recent deploys,
and config changes to find the most likely cause of a fresh outage;
translate a console screenshot into the exact commands that fix the
failing resource; ask a plain-English question over connected logs,
showing both the generated query and its result.

### Data (2 prompts)

Answer one-off questions from files: read a data file, find the key
patterns, and write the result as a page with charts; read performance
data, pick the underperformers, and generate new variations inside a
stated constraint (for example a character limit).

### Automate (4 prompts)

Make repeat work reusable: turn a recurring multi-step task into a
named skill; write a hook that runs an action on a trigger event;
connect a tool via MCP so the agent reads its data directly instead of
receiving pastes every session; close a session by summarizing what
was done and proposing what to record for next time.

## Prerequisites some prompts need

- Issue-tracker prompts: tracker connected via MCP or connector, or
  the `gh` CLI authenticated.
- Log, warehouse, and ad-platform prompts: the data source connected,
  or the exported file handed over directly.
- Screenshot-comparison prompts: a render and screenshot capability
  (a browser tool or a Playwright MCP server).
- Subagent review prompts: no extra setup; the built-in general-purpose
  subagent handles them.

## Make-it-stick mechanics (Claude Code)

These exist in Claude Code. On another agent, use its equivalent or
skip them.

- `/init` to record project facts (test commands, architecture) in
  CLAUDE.md.
- Output styles to pin a preferred explanation format or audience level.
- `/command` skills to turn a working prompt into a repeatable command.
- `/goal` to keep the agent iterating toward a measurable target.
- `/verify` to check a finished change against the running app.
- `/code-review` for the one-command version of the pre-commit and PR
  reviews.
- Plan mode for plan-first work; rewind to restore code and
  conversation before a clean retry.
- `/memory` to review what the agent recorded.
