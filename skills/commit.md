Guide the user through creating a clean, conventional git commit.

Follow these steps:

1. **Show current state** — run `git status` and `git diff --staged` to display what is staged and what is unstaged
2. **Confirm scope** — ask the user to confirm which files should be included in this commit if anything unstaged looks relevant
3. **Draft a commit message** using the Conventional Commits format:
   - Type: `feat`, `fix`, `refactor`, `test`, `chore`, `docs`, `style`, `perf`, or `ci`
   - Scope (optional): the area of the codebase affected, e.g. `feat(auth):`
   - Subject: imperative mood, lowercase, no trailing period, under 72 characters
   - Body (if needed): explain *why*, not *what*; wrap at 72 characters
   - Example: `feat(auth): add JWT refresh token rotation`
4. **Present the message** — show the full proposed commit message and wait for approval
5. **Commit** — once approved, stage any additional files confirmed in Step 2 and run `git commit`

Do not run `git commit` before the user approves the message.
Do not use `--no-verify` or skip hooks.
