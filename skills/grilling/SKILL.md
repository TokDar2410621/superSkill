---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Each round puts the frontier to the user, every question carrying your recommended answer. Then wait for the user's answers before the next round.

Before the first round, call the Skill tool with "stop-slop". Write every question, option, and description by its rules.

## Asking a round

Ask each round through your harness's **question tool**: the built-in tool that puts multiple-choice questions in front of the user and returns their picks. Known names, verified 2026-10-20:

| Harness                       | Question tool         | Questions per call | Options per question |
| ----------------------------- | --------------------- | ------------------ | -------------------- |
| Claude Code, Claude Agent SDK | `AskUserQuestion`     | 4                  | 2-4                  |
| Gemini CLI                    | `ask_user`            | 4                  | 2-4                  |
| Codex CLI (Plan mode)         | `request_user_input`  | 3                  | 2-3                  |
| OpenCode                      | `question`            | uncapped           | uncapped             |
| VS Code Copilot               | `vscode_askQuestions` | uncapped           | uncapped             |
| Copilot CLI                   | `ask_user`            | unknown            | unknown              |
| Cursor                        | `AskQuestion`         | unknown            | unknown              |

Any other tool with the same job counts as your question tool.

One round is one call:

- **Cap**: ask as many frontier questions as the tool accepts per call, 4 when its limit is unknown or uncapped. When the frontier is wider, ask first the questions whose answers unblock the most of the tree; the rest wait for the next round.
- **Recommendation**: your recommended answer is the first option, its label ending in "(Recommended)", its description saying why.
- **Options**: mutually exclusive, within the tool's option limit. The user reaches any other answer through the tool's free-text choice. Turn on multi-select only for choices that combine.
- **Header**: 12 characters at most.

Some free-text answers are a question back or a doubt. Answer it, then put the question again in the next round.

## Without a question tool

When the harness has no question tool, or the tool refuses the call (Codex outside Plan mode, a non-interactive run), ask the whole frontier as text. Number each question and give your recommended answer:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Word each question so "yes" accepts your recommended answer.

## Between rounds

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.
