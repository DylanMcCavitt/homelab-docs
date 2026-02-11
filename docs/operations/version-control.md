---
type: guide
area: operations
status: active
updated: 2026-02-11
---
# Version Control Strategy (Private + Public)

## Model
- Private repo: source of truth, full working notes (still with strict secret guardrails).
- Public repo: generated from explicit allowlist (`public-allowlist.txt`) via `public-export/`.

## One-time setup
1. Initialize git (already done if `.git/` exists).
2. Activate hooks:
   - `scripts/bootstrap-git.sh`
3. Add remotes (example):
   - `git remote add origin <private-repo-url>`
   - `git remote add public <public-repo-url>`

## Daily private workflow
1. Update docs.
2. `git status`
3. `git add -A`
4. `git commit -m "..."`
5. `git push origin main`

## Public publish workflow
1. Build allowlisted export:
   - `scripts/build-public-export.sh`
   - This step also converts Obsidian `[wikilinks](../../wikilinks.md)` to GitHub markdown links and strips Mermaid init directives for compatibility.
2. Sync into your separate public repo checkout:
   - `scripts/sync-public-repo.sh /absolute/path/to/homelab-public`
3. In public repo:
   - `git add -A`
   - `git commit -m "publish homelab update"`
   - `git push origin main`

## Safety controls in place
- `.gitignore` blocks known sensitive artifacts.
- `.githooks/pre-commit` blocks sensitive paths + high-confidence secret patterns.
- Public export uses explicit allowlist only.
