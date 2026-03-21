# Ultrapowers

Customized workflow skills and agents for development—similar to superpowers but tailored specifically for this use case.

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

## Directory Structure

```
ultrapowers/
├── skills/                    # Available skills
│   ├── brainstorming/
│   │   └── SKILL.md
│   ├── writing-plans/
│   │   └── SKILL.md
│   ├── adversarial-review/
│   │   └── SKILL.md
│   ├── requesting-code-review/
│   │   ├── SKILL.md
│   │   └── code-reviewer.md     # Template for dispatch
│   ├── quick-iteration/
│   │   ├── SKILL.md
│   │   └── references/
│   ├── skillkit/
│   │   ├── SKILL.md
│   │   ├── knowledge/           # Knowledge base for skillkit
│   │   ├── scripts/            # Python scripts for creation/validation
│   │   ├── references/
│   │   └── tests/
│   ├── subagent-driven-development/
│   │   ├── SKILL.md
│   │   ├── implementer-prompt.md
│   │   ├── spec-reviewer-prompt.md
│   │   └── code-quality-reviewer-prompt.md
│   ├── systematic-debugging/
│   │   ├── SKILL.md
│   │   └── *.md                # Supporting documents
│   ├── releasing/
│   │   ├── SKILL.md
│   │   └── references/
│   └── ...
└── agents/                     # Subagent definitions
    ├── code-reviewer.md
    └── bug-hunter.md
```

## Usage

### Invoking Skills

```bash
# Direct from Claude Code
/Skill brainstorming
/Skill writing-plans
/Skill quick-iteration
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

## Usage Examples

### Example 1: New Feature

```
User: "I want to build a PDF export feature for reports"

Agent: [invoke brainstorming]
- Clarify requirements: PDF format? what data? destination?
- Propose 2-3 approaches
- Present design
- User approves

[invoke writing-plans]
- Create implementation plan with detailed tasks

[invoke subagent-driven-development]
- Dispatch implementer per task
- Review spec compliance + code quality after each task

[invoke requesting-code-review]
- Final review before merge

[invoke finishing-a-development-branch]
- Decide: PR? merge? branch management?
```

### Example 2: Quick Fix

```
User: "Add email validation to registration form"

Agent: [invoke quick-iteration]
- Step 1: Capture intent - validate email format
- Step 2: Write brief plan (3-5 bullets)
- Step 3: Dispatch subagent reviewer
- Step 4: Fix issues from review
- Step 5: Implement

[invoke requesting-code-review]
- Review before commit
```

### Example 3: Debugging

```
User: "API keeps returning 500"

Agent: [invoke systematic-debugging]
- Phase 1: Root cause investigation
  - Read error messages carefully
  - Reproduce consistently
  - Check recent changes
  - Gather evidence

- Phase 2: Hypothesis formation
- Phase 3: Fix validation
- Phase 4: Prevention

[If more help needed, dispatch bug-hunter agent]
```

## Superpowers Integration

Some skills in this repo are customizations of existing superpowers skills:

| Ultrapowers Skill | Original Superpowers |
|-------------------|---------------------|
| `brainstorming` | `superpowers:brainstorming` |
| `writing-plans` | `superpowers:writing-plans` |
| `adversarial-review` | `superpowers:adversarial-review` |
| `requesting-code-review` | `superpowers:requesting-code-review` |
| `quick-iteration` | Custom (not in superpowers) |
| `skillkit` | Custom (not in superpowers) |
| `subagent-driven-development` | `superpowers:subagent-driven-development` |
| `systematic-debugging` | `superpowers:systematic-debugging` |
| `releasing` | `superpowers:releasing` |
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

Private—for internal use only.
