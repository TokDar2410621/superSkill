# superSkill: repo conventions

- Skills live flat under `skills/<name>/`. At 10 skills, regroup them into category folders (`skills/<category>/<name>/`), then update `plugin.json`, both README sections, and the Gemini install command.
- Every skill appears in `.claude-plugin/plugin.json`'s `skills` array and in both language sections of `README.md`, its name linked to its `SKILL.md`.
- Every skill is either user-invoked (`disable-model-invocation: true` in `SKILL.md` plus `policy.allow_implicit_invocation: false` in `agents/openai.yaml`) or model-invoked (neither). Every skill carries an `agents/openai.yaml`.
- A skill reaches another by naming the Skill tool: `Call the Skill tool with "grilling"`. Only model-invoked skills can be reached this way.
- After touching a manifest, run `claude plugin validate . --strict`. Bump `version` in `plugin.json` for each release: managed installs update only when it changes.
- Adapted skills keep their upstream credit in `README.md` and `LICENSE`.
- No em-dashes in any prose. Rewrite the sentence with a comma, colon, period, or parentheses.
