# MECE Skill

**Mutually Exclusive, Collectively Exhaustive** — a validation and decomposition skill for AI agents.

Stop shipping specs with overlaps and gaps. MECE ensures every category, requirement, and plan item belongs to exactly one bucket and nothing is left off the list.

## What it does

| Mode | Trigger | Output |
|------|---------|--------|
| **Validate** | Run `/mece` after creating any structured list | Overlap report + gap report + corrected structure |
| **Decompose** | `/mece break down [topic]` | MECE issue tree from scratch |

## Why this matters

Most flawed thinking comes from two sources:
1. **Counting something twice** under different names (overlap)
2. **Missing something entirely** (gap)

This is expensive when you're writing product specs, designing APIs, planning roadmaps, or prompting AI to build features. MECE catches both before they become code, strategy, or shipped product.

## Install

### Quick install (recommended)
```bash
npx skills add https://github.com/uxderrick/mece-skill
```

### Manual install

Clone the repo:
```bash
git clone https://github.com/uxderrick/mece-skill.git
```

Then copy to your agent's skills directory:

| Agent | Command |
|-------|---------|
| **Claude Code** | `mkdir -p ~/.claude/skills && cp -r mece-skill/skills/mece ~/.claude/skills/mece` |
| **Cursor** | `mkdir -p ~/.cursor/skills && cp -r mece-skill/skills/mece ~/.cursor/skills/mece` |
| **VS Code / Copilot** | `mkdir -p ~/.copilot/skills && cp -r mece-skill/skills/mece ~/.copilot/skills/mece` |
| **Codex (OpenAI)** | `mkdir -p ~/.agents/skills && cp -r mece-skill/skills/mece ~/.agents/skills/mece` |
| **Gemini CLI** | `mkdir -p ~/.gemini/skills && cp -r mece-skill/skills/mece ~/.gemini/skills/mece` |
| **Universal (all agents)** | `mkdir -p ~/.agents/skills && cp -r mece-skill/skills/mece ~/.agents/skills/mece` |

### Project-level install
Add to a specific repo instead of globally:
```bash
mkdir -p .agents/skills && cp -r mece-skill/skills/mece .agents/skills/mece
```

## Usage

### Validate existing work
Write your spec, feature list, or plan. Then:
```
/mece
```
The skill reviews your structure and reports:
- **Overlaps** — items that belong to multiple categories
- **Gaps** — scenarios not covered by any category
- **Suggested fix** — corrected MECE structure

### Decompose a new problem
```
/mece break down user authentication
/mece break down our pricing tiers
/mece break down the deployment pipeline
```

### Validate inline (no slash command)
Just ask your AI agent:
> "Check if this list is MECE"
> "Is anything overlapping or missing here?"

The skill auto-triggers on these patterns.

## Who is this for?

- **Vibecoders** — Stop generating code from specs with holes. Validate before you build.
- **Product managers** — PRDs, user stories, and roadmaps that actually hold up in sprint planning.
- **Engineers** — API designs, state management, and architecture with clean boundaries.
- **Founders** — Strategy docs, pitch decks, and business plans with no logical gaps.
- **Consultants** — You already know MECE. Now your AI does too.

## Examples

See [examples.md](skills/mece/examples.md) for full before/after walkthroughs:
- The Protein Problem (everyday intuition)
- The Packing List (collective exhaustiveness)
- Product Feature Spec (PM workflow)
- API Endpoint Design (engineering)
- State Management / Reducers (React/frontend)
- Roadmap Phases (product strategy)

## Background

MECE was created by **Barbara Minto** at McKinsey & Company in the 1960s. It's been the foundational problem-structuring tool at McKinsey, Bain, and BCG for 60+ years.

This skill brings MECE to AI-native workflows — where the cost of overlaps and gaps is amplified because AI builds exactly what you tell it to, including the mistakes.

## License

MIT
