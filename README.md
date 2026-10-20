# superSkill

[Français](#français) · [English](#english)

## Français

Tu décris une idée à ton IA et elle fonce : elle code ou planifie avant d'avoir compris ce que tu veux. superSkill l'oblige à t'interroger d'abord. Elle pose ses questions par rounds, sous forme de menus à choix, sa recommandation en premier, et n'avance qu'une fois que vous êtes d'accord sur le plan.

### Les skills

**Tu les lances**

- [grill-me](skills/grill-me/SKILL.md) : un interrogatoire sans relâche pour affûter un plan ou une décision.

**L'IA les lance (toi aussi)**

- [grilling](skills/grilling/SKILL.md) : la méthode d'interrogatoire. Des rounds de questions à choix via l'outil de questions de ton agent, en texte quand il n'en a pas.
- [stop-slop](skills/stop-slop/SKILL.md) : retire les tics d'écriture IA d'un texte.

### Installer

Choisis une seule méthode par agent, sinon chaque skill apparaît en double.

**Claude Code**

```bash
claude plugin marketplace add TokDar2410621/superSkill
claude plugin install superskill@superskill
```

Tape ensuite `/superskill:grill-me` et décris ton plan.

**Codex**

```bash
codex plugin marketplace add TokDar2410621/superSkill
codex plugin add superskill@superskill
```

Codex affiche les menus en mode Plan seulement. Dans les autres modes, grilling pose ses questions en texte.

**GitHub Copilot CLI**

```bash
copilot plugin marketplace add TokDar2410621/superSkill
copilot plugin install superskill@superskill
```

**Gemini CLI**

```bash
gemini skills install https://github.com/TokDar2410621/superSkill.git --path skills
```

**Cursor, OpenCode, Windsurf, Amp et les autres**

```bash
npx skills@latest add TokDar2410621/superSkill
```

Pour mettre à jour : `npx skills@latest update`.

### Crédits

- grill-me et grilling sont adaptés de [mattpocock/skills](https://github.com/mattpocock/skills) (MIT, Matt Pocock). Changements : menus de questions natifs de chaque agent, plafond de questions par round, version texte de secours, écriture stop-slop.
- stop-slop vient de [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop) (MIT, Hardik Pandya). Seul changement : un em-dash retiré d'un exemple, pour respecter la règle 6 du skill.

Licence : [MIT](LICENSE).

## English

You describe an idea and your AI charges ahead: it codes or plans before it understands what you want. superSkill makes it interview you first. It asks in rounds of multiple-choice menus, its recommendation listed first, and moves only once you both agree on the plan.

### Skills

**You run these**

- [grill-me](skills/grill-me/SKILL.md): a relentless interview to sharpen a plan or design.

**Your AI runs these (so can you)**

- [grilling](skills/grilling/SKILL.md): the interview method. Rounds of multiple-choice questions through your agent's question tool, plain text when it has none.
- [stop-slop](skills/stop-slop/SKILL.md): removes AI writing patterns from prose.

### Install

Pick one route per agent, or every skill shows up twice.

**Claude Code**

```bash
claude plugin marketplace add TokDar2410621/superSkill
claude plugin install superskill@superskill
```

Then type `/superskill:grill-me` and describe your plan.

**Codex**

```bash
codex plugin marketplace add TokDar2410621/superSkill
codex plugin add superskill@superskill
```

Codex shows the menus in Plan mode only. In other modes, grilling asks in plain text.

**GitHub Copilot CLI**

```bash
copilot plugin marketplace add TokDar2410621/superSkill
copilot plugin install superskill@superskill
```

**Gemini CLI**

```bash
gemini skills install https://github.com/TokDar2410621/superSkill.git --path skills
```

**Cursor, OpenCode, Windsurf, Amp and the rest**

```bash
npx skills@latest add TokDar2410621/superSkill
```

To update: `npx skills@latest update`.

### Credits

- grill-me and grilling are adapted from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT, Matt Pocock). Changes: each agent's native question menus, a per-round question cap, a plain-text fallback, stop-slop writing.
- stop-slop comes from [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop) (MIT, Hardik Pandya). One change: an em-dash removed from an example, to follow the skill's own rule 6.

License: [MIT](LICENSE).
