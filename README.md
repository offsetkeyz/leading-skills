# offsetkeyz-skills

Claude Code plugin marketplace of personal skills.

## Install

```
/plugin marketplace add offsetkeyz/leading-skills
/plugin install find-your-voice@offsetkeyz-skills
/plugin install process-eval@offsetkeyz-skills
/plugin install capture-tribal-knowledge@offsetkeyz-skills
```

Local testing: `/plugin marketplace add ./` from this folder.

## Plugins

| Plugin | Description |
|---|---|
| find-your-voice | Discover your writing voice, package it as a "write like me" skill |
| process-eval | Evaluate any process against cited frameworks (ToC, Lean, ECRS, checklists). Questions first, then a ranked fix list |
| capture-tribal-knowledge | Interview an expert, turn their tacit know-how into a Confluence runbook, a Claude skill, or both |

## Adding a skill

1. `mkdir -p plugins/<name>/.claude-plugin plugins/<name>/skills/<name>`
2. Add `plugins/<name>/skills/<name>/SKILL.md`
3. Add `plugins/<name>/.claude-plugin/plugin.json`
4. Add an entry to `.claude-plugin/marketplace.json`
5. `claude plugin validate .`

## Build a `.skill` bundle (for claude.ai upload)

```
cd plugins/<name>/skills && zip -r ../../../dist/<name>.skill <name>
```

## Evals

Test cases live in `evals/<skill-name>/` at the repo root, not inside the plugin, so they don't ship with `/plugin install` or `.skill` bundles. Same case-folder layout `claude plugin eval` reads; to run with the CLI, copy them next to the skill first:

```
cp -R evals/<name> plugins/<name>/skills/<name>/evals
cd plugins/<name>/skills/<name> && claude plugin eval . --scaffold
rm -rf evals
```
