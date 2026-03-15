# Using Claude Code Effectively: Plan → Develop → Review → Secure

A practical guide based on Claude Code's actual behaviour, covering persona management,
project configuration, and a multi-role development workflow.

---

## 1. Project Configuration with CLAUDE.md

Claude Code automatically reads `CLAUDE.md` from the project root and merges any
`CLAUDE.md` files it finds as it walks up the directory tree. This makes it the right
place to put persistent, always-on context — no flags to pass, no commands to run.

**Key behaviours to know:**

- The directory you launch Claude Code from is treated as the project root
- In a monorepo, launching from `packages/web-app/` will load both that package's
  `CLAUDE.md` and the root-level one — closer files take precedence
- A global `~/.claude/CLAUDE.md` applies across all projects; project-level files
  layer on top of it
- `AGENTS.md` is a different convention (used by OpenAI Codex). Claude Code does not
  read it — use `CLAUDE.md` exclusively

**Recommended layout:**

```
your-project/
├── CLAUDE.md                          # project-wide: stack, conventions, structure
├── .claude/
│   └── personas/
│       ├── web-developer.md           # React/Vite/TS expert persona
│       ├── code-reviewer.md           # review standards and checklist
│       └── security-analyst.md        # threat modelling and vuln analysis
├── src/
└── package.json
```

Put things that are always true in `CLAUDE.md` (stack, folder structure, commit
conventions). Keep role-specific behaviour in persona files and invoke them on demand.

---

## 2. Defining Personas

A persona is a set of instructions that shapes how Claude approaches a task — its
priorities, output style, and workflow. Define one per role as a markdown file.

**What each persona file should contain:**

- The role and its primary goal
- Technologies or frameworks relevant to that role
- A specific workflow to follow (step by step)
- Output standards (what good looks like)

Three personas for a typical development cycle:

### web-developer.md

Focus: TypeScript, React, Vite, Vitest, Playwright, pnpm

Workflow to enforce:
1. Write an implementation plan first (files to touch, design decisions) — wait for
   approval before coding
2. Implement in small slices: 2–5 files, under 300 lines per iteration
3. Write tests immediately after: Vitest for unit logic, Playwright for UI behaviour
4. Propose a `git commit` with a conventional commit message (`feat:`, `fix:`, etc.)

### code-reviewer.md

Focus: correctness, readability, TypeScript strictness, test coverage, maintainability

Workflow to enforce:
1. Summarise what the change does before evaluating it
2. Flag issues by severity: blocking / should-fix / suggestion
3. Check for missing or weak tests explicitly
4. Approve only when blocking issues are resolved

### security-analyst.md

Focus: OWASP Top 10, dependency vulnerabilities, secrets in code, auth and input
validation, CSP and CORS configuration

Workflow to enforce:
1. Identify the attack surface of the change
2. List findings with severity (critical / high / medium / low)
3. Suggest a concrete remediation for each finding
4. Re-evaluate after fixes are applied

---

## 3. Switching Between Roles

Personas are not automatically active — you invoke them by referencing the file in
your message using the `@` mention syntax in Claude Code:

```
@.claude/personas/web-developer.md  Implement the login form
@.claude/personas/code-reviewer.md  Review the changes in src/auth/
@.claude/personas/security-analyst.md  Analyse the authentication flow for vulnerabilities
```

This gives you explicit, deliberate role switching without any session restarts. The
base context from `CLAUDE.md` (stack, structure, conventions) remains active regardless
of which persona you load.

---

## 4. The Full Development Cycle

A single feature moves through four stages, each with its own persona:

```
PLAN & BUILD          CODE REVIEW           SECURITY ANALYSIS       COMMIT & SHIP
──────────────        ───────────           ─────────────────       ─────────────
web-developer.md  →   code-reviewer.md  →   security-analyst.md →   git commit
  Plan                  Summarise             Map attack surface      Conventional
  Implement             Flag by severity      Flag by severity        commit message
  Test (unit + e2e)     Check coverage        Suggest remediation     All files staged
  Propose commit        Approve / block       Re-evaluate after fix
```

Nothing moves to the next stage until the current one is complete. The developer
persona blocks on approval before writing code. The reviewer blocks on blocking issues
before approving. The security analyst re-evaluates after fixes — not just accepts the
first pass.

---

## 5. What Goes Where — Quick Reference

| Content | Location |
|---|---|
| Stack, folder structure, conventions always true | `CLAUDE.md` (project root) |
| Conventions true across all your projects | `~/.claude/CLAUDE.md` |
| Web developer role instructions | `.claude/personas/web-developer.md` |
| Code reviewer role instructions | `.claude/personas/code-reviewer.md` |
| Security analyst role instructions | `.claude/personas/security-analyst.md` |
| One-off task instructions | Inline in your message |

---

## 6. Tips for Keeping It Effective

- **Keep `CLAUDE.md` factual, not instructional.** Stack facts, folder layout, and
  naming conventions belong there. Workflow instructions belong in persona files.
- **Version control everything.** `CLAUDE.md` and `.claude/personas/` should be
  committed. Your team inherits the same AI behaviour automatically.
- **Iterate on persona files like code.** If Claude keeps missing something in a
  review, add it to the reviewer persona. Treat the files as living documents.
- **Don't skip the plan step.** The web developer persona is designed to pause before
  coding. Let it. Catching a wrong approach in the plan costs seconds; catching it
  after implementation costs much more.
