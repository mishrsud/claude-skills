# claude-skills

A reusable collection of Claude Code skills and personas for development projects.

## Purpose

This repo provides a shared library of Claude Code configuration — personas, skills, and workflow guides — that can be dropped into any project to establish consistent AI-assisted development practices.

## Structure

```
claude-skills/
├── personas/                        # Role-specific instruction sets
│   ├── web-developer.md             # TypeScript/React/Vite developer
│   ├── code-reviewer.md             # Severity-driven code review
│   └── security-analyst.md          # OWASP-focused security analysis
├── skills/                          # Reusable slash commands
│   ├── plan.md                      # Generate an implementation plan
│   ├── review.md                    # Review code changes
│   ├── secure.md                    # Analyse code for vulnerabilities
│   └── commit.md                    # Draft and create a conventional commit
├── docs/
│   └── claude-code-workflow-guide.md
└── README.md
```

## Personas

Persona files define how Claude should approach a task — its priorities, output style, and enforced workflow. Invoke a persona by referencing its file with the `@` mention syntax in Claude Code.

### `personas/web-developer.md`

A TypeScript/React/Vite developer that enforces a plan-first, test-after discipline.

```
@personas/web-developer.md  Implement the login form
```

**Enforced workflow:** Plan → wait for approval → implement in slices → write tests → propose commit

### `personas/code-reviewer.md`

A code reviewer that categorises findings by severity and blocks approval until all blocking issues are resolved.

```
@personas/code-reviewer.md  Review the changes in src/auth/
```

**Enforced workflow:** Summarise → flag by severity (blocking / should-fix / suggestion) → assess test coverage → approve or block

### `personas/security-analyst.md`

A security analyst covering OWASP Top 10, secrets, auth, input validation, and transport security.

```
@personas/security-analyst.md  Analyse the authentication flow for vulnerabilities
```

**Enforced workflow:** Map attack surface → list findings by severity (critical / high / medium / low) → concrete remediations → re-evaluate after fixes

---

## Skills (Slash Commands)

Skills are slash commands that trigger a focused workflow. Copy skill files into your project's `.claude/commands/` directory or globally into `~/.claude/commands/` to make them available.

| Skill | Command | Purpose |
|---|---|---|
| `skills/plan.md` | `/plan` | Generate an implementation plan; blocks on approval before any code |
| `skills/review.md` | `/review` | Review staged or specified changes with severity-labelled findings |
| `skills/secure.md` | `/secure` | Analyse a file, directory, or diff for security vulnerabilities |
| `skills/commit.md` | `/commit` | Draft a conventional commit message and commit after approval |

### Installing skills

**Project-level** (available within one project):
```
cp claude-skills/skills/*.md your-project/.claude/commands/
```

**Global** (available across all projects):
```
cp claude-skills/skills/*.md ~/.claude/commands/
```

---

## The Full Development Cycle

A single feature moves through four stages:

```
/plan  →  web-developer.md  →  /review  →  /secure  →  /commit
Plan       Implement + test     Review       Analyse      Ship
           (slices)             findings     findings
```

Nothing moves to the next stage until the current one is complete.

---

## Installing Personas

Copy persona files into your project's `.claude/personas/` directory:

```
cp claude-skills/personas/*.md your-project/.claude/personas/
```

Commit the `.claude/` directory so your whole team inherits the same behaviour automatically.

---

## Further Reading

- [`docs/claude-code-workflow-guide.md`](docs/claude-code-workflow-guide.md) — detailed guide covering CLAUDE.md configuration, persona design, role switching, and the full development cycle
