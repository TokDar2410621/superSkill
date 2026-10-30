---
name: souffleur
description: Write, improve, or check a prompt meant for a coding agent (Claude Code, Codex, or similar), using Anthropic's Claude Code prompt library and its six prompting patterns. Use it whenever the user wants a prompt to hand to a coding agent, or asks how to phrase a coding request, instead of drafting one from scratch. Typical requests are 'write me a prompt for Claude Code', 'how should I ask Codex to...', 'improve this prompt', 'aide-moi à formuler ma demande', 'écris-moi un prompt pour Claude Code', 'comment je demande à Claude Code de...'. Not for doing the coding task itself.
---

# Souffleur

## Purpose

Turn a request for a coding agent into a well-formed prompt, using
Anthropic's published prompt library as the source of patterns and
starting points. The library holds 52 prompts in 15 groups, organized by
phase (Discover, Design, Build, Ship, Operate) and by task. Fetch the
exact prompt text from the official page at use time; this skill holds
the map, the patterns, and the adaptation rules, not a copy of the
library.

Source: [Claude Code prompt library](https://code.claude.com/docs/en/prompt-library)
(official Claude Code docs, 52 prompts in 15 groups, verified
2026-10-30). The page calls its prompts starting points, not scripts,
and each one links back to the Anthropic guide it came from (Common
workflows, Best practices, How Anthropic teams use Claude Code, Scaling
agentic coding guide).

## Scope

Use this skill when the user wants a prompt for a coding agent: writing
one, improving one they drafted, or checking one before they run it.
Do not use it for general chat or for prompts unrelated to coding work.

## Workflow

### 1. Identify the phase and the group

Read `references/library-map.md` and match the request to one group.
When the request spans groups, pick the group of the immediate next
action, not the final goal. When unsure between two groups, prefer the
narrower one (a failing test is Debug before it is Refactor; a
pre-commit check is Review before it is Git).

### 2. Check for a skill the user already has

If the user names a skill, use it. Otherwise, before fetching the
library prompt, look at the skills available in this session. When one
already covers the task (a TDD, debugging, or review skill, for
example), read its SKILL.md and compare it with the matching library
prompt:

- Prefer the user's skill when it is specific to this project or stack,
  or encodes rules the library prompt lacks. The user installed it on
  purpose.
- Prefer the library prompt when the user's skill is generic and the
  library prompt fits the exact task better.
- Tell the user in one line which one you picked and why, then proceed.
  On a close call, ask.

Whichever wins, check the final prompt against the six patterns in
step 4.

### 3. Fetch the matching prompt from the official page

Open the official prompt library page and take the prompt of the matched
group. Replace every placeholder with the project's real specifics:
file paths, function names, issue or PR numbers, metric names and
thresholds, time ranges, tracker names. Keep the prompt in English: the
library wording is tuned for the coding agent, and translating it
weakens it. Explain the result to the user in their own language.

### 4. Apply the six patterns

Check the adapted prompt against these six patterns, and repair what is
missing:

1. **Describe the outcome, not the steps.** State what is wanted and
   let the agent find the files. Never hand it a file list it could
   discover itself, unless the file is the actual subject.
2. **Give it a way to check its own work.** Bundle run, test, compare,
   or verify into the same prompt, so the agent iterates instead of
   stopping after one attempt.
3. **Point at a reference.** Name an existing file, test, or pattern to
   match, so new code follows the conventions already in the repo
   instead of generic best practice.
4. **State the measurable target.** For performance or coverage work,
   give the metric and the threshold, so completion is unambiguous.
5. **Give it the artifact.** Paste the error, log, screenshot, or plan
   output directly, or reference the file, instead of describing it.
   The agent reads the source, not a summary of it.
6. **Say how the answer should come back.** Name the format, length,
   or audience, so the explanation fits how it will be used.

### 5. Plan first for risky or wide changes

For any multi-file change, migration, or infrastructure change, separate
exploration from edits: ask for the plan and the file list first, with
no edits yet, and show it to the user before anything moves. For a
change that touches production config, ask for a plain-language summary
of what will actually change before applying it.

### 6. Hand the prompt back

Show the final prompt to the user, and say in a line or two what you
changed and why. Run it only when the user asks you to.

### 7. Make it stick

When a prompt works well on a project, do not leave it in chat history:

- Save it as a reusable command or skill for the project.
- Record any convention the agent had to learn (naming, exports, test
  commands, review checklist) in the project's agent instructions file
  (CLAUDE.md, AGENTS.md, or the equivalent), so the next session starts
  with that context.
- For long reviews, run the audit in a subagent so the main session
  stays clean, and report back a summary.
- For repeat behavior, turn it into a hook (trigger plus action) where
  the agent supports hooks, instead of asking for it every session.

### 8. Guardrails

These override any prompt in the library:

- Never commit, and never push to the default branch, without the
  user's explicit go-ahead. A library prompt that says "commit" still
  waits for it.
- Never report work as done without running the verification the
  prompt asks for (tests, build, screenshot comparison). Report a fix
  that was not verified as unverified, not as done.

## Start here

When the user does not know where to start, or the request is vague,
fall back to these five entry points from the library, in this order:

1. **Get oriented** in a new or unfamiliar repository: architecture,
   key directories, and how the pieces connect.
2. **Find where a behavior happens**, searching by what the code does
   rather than by filename.
3. **Find and fix a failing test**, starting from the symptom only.
4. **Write tests, run them, and fix failures** in one loop, so the
   agent iterates without stopping for instructions.
5. **Review uncommitted changes** and flag anything risky before a
   commit, reading the changed files in full rather than just the diff.

## When the library does not cover the task

Some library prompts need setup first. Flag the prerequisite instead of
faking it:

- Issue-tracker prompts need the tracker connected (MCP server or
  connector) or the `gh` CLI authenticated.
- Log and data prompts need the log store, warehouse, or ad platform
  connected, or the file handed over directly.
- Screenshot-comparison prompts need a way to render and screenshot
  the result (a browser tool or a Playwright MCP server).

The library is written for Claude Code. Some of its mechanics (`@` file
references, `/verify`, `/init`, `/goal`, `/code-review`, output styles,
plan mode) exist only there; on another agent, use its equivalent or
leave them out.

When no group fits, fall back to the six patterns in step 4 and write
the prompt from them directly. When the official page has changed since
this skill was written, trust the page over this skill's map, and tell
the user what changed.
