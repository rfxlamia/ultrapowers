# Ultrapowers

Customized workflow skills and agents for my development using [superpowers](https://github.com/obra/superpowers) but tailored specifically for **my** use case. You can use it freely btw.

## Overview

This repo contains skills and agents that support end-to-end development workflows, from feature brainstorming through code review and deployment. These are customizations of Claude Code's superpowers, adapted for specific use cases in this project.

## Skills

| Skill | Purpose |
|-------|---------|
| `brainstorming` | Explore ideas into designs/specs before implementation. Mandatory before any creative work. |
| `writing-plans` | Create implementation plan from approved spec. |
| `adversarial-review` | Rigorous, detailed review—essentially being adversarial toward your own work. |
| `requesting-code-review` | Request code review using the `code-reviewer.md` agent. |
| `quick-iteration` | Quick fix/quick feat without lengthy planning. Still has plan + subagent review gates. |
| `skillkit` | Create customizable skills—includes tools and scripts for creation, validation, and security scanning. |
| `subagent-driven-development` | Execute plans by dispatching subagents per task, with two-stage review (spec compliance + code quality). |
| `systematic-debugging` | Systematic debugging—must find root cause before proposing fixes. Used with the `bug-hunter.md` agent. |
| `releasing` | Automate full release workflow: version bumping, changelog generation, git tagging, pushing, and GitHub release creation. |

## Agents

| Agent | Purpose |
|-------|---------|
| `code-reviewer` | Senior code reviewer—reviews implementation against original plan, checks code quality, architecture, and best practices. |
| `bug-hunter` | Debugging specialist—systematically finds root cause before fixing. |

## Main Workflows

### Full Workflow (For new features / complex systems)

```
brainstorming → writing-plans → subagent-driven-development → requesting-code-review → finishing-a-development-branch
```

1. **brainstorming** - Explore ideas, clarify requirements, create design, get user approval
2. **writing-plans** - Convert design into detailed implementation plan
3. **subagent-driven-development** - Execute plan task-by-task with reviews
4. **requesting-code-review** - Final review before merge
5. **finishing-a-development-branch** - Decide how to integrate to main

### Quick Workflow (For small fixes)

```
quick-iteration → requesting-code-review
```

1. **quick-iteration** - Brief plan (3-5 bullets) + subagent review, then implement
2. **requesting-code-review** - Review before commit

### Debugging Workflow

```
systematic-debugging → bug-hunter agent → (return to relevant workflow)
```

1. **systematic-debugging** - Investigate root cause first
2. **bug-hunter** - Specialist agent to help with debugging
3. Once fix is ready, return to appropriate workflow (quick-iteration or full)

## Usage

### Invoking Skills

```bash
# Direct from Claude Code
/brainstorming
/writing-plans
/quick-iteration
```

Or via the Skill tool:

```
[Agent] will detect relevant skills and invoke them automatically.
```

### Using the Workflows

**For new features:**
1. Simply describe the feature you want to build
- Agent will automatically invoke `brainstorming` skill
- Follow the process until design is approved
- Continue to `writing-plans`
- Execute with `subagent-driven-development`

**For quick fixes:**
1. Describe the fix needed
- Agent will invoke `quick-iteration` if appropriate
- Plan (3-5 bullets) will be reviewed by a subagent
- Once approved, implement directly

**For debugging:**
1. Share the error or bug encountered
- Agent will invoke `systematic-debugging`
- Find root cause first
- Then propose fix

## Superpowers Integration

Some skills in this repo are customizations of existing superpowers skills:

| Ultrapowers Skill | Original Superpowers |
|-------------------|---------------------|
| `brainstorming` (enhanced with identify hypothese phase) | `superpowers:brainstorming` |
| `writing-plans` | `superpowers:writing-plans` |
| `adversarial-review` | `superpowers:adversarial-review` |
| `requesting-code-review` | `superpowers:requesting-code-review` |
| `quick-iteration` | Custom (not in superpowers) |
| `skillkit` | Custom (not in superpowers) |
| `subagent-driven-development` | `superpowers:subagent-driven-development` |
| `systematic-debugging` | `superpowers:systematic-debugging` |
| `releasing`                                              | Custom (not in superpowers)               |
| `code-reviewer` (agent) | `superpowers:code-reviewer` |
| `bug-hunter` (agent) | Custom (not in superpowers) |

Differences:
- **skillkit** - Has scripts and tools for automated skill creation/validation
- **quick-iteration** - Lightweight alternative for quick changes
- **bug-hunter** - Specialized debugging agent

## Requirements

- Python 3.12+ (for skillkit scripts)
- Claude Code with Skill tool

## Development

### Setting up skillkit

```bash
cd skills/skillkit
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt  # if any
```

### Validating a skill

```bash
cd skills/skillkit
python3 scripts/validate_skill.py <path-to-skill>
```

### Security scan

```bash
cd skills/skillkit
python3 scripts/security_scanner.py <path-to-skill>
```

## License

WTFPL
