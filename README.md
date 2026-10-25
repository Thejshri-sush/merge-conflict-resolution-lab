# Merge Conflict Resolution Lab

A hands-on Git lab: resolve a merge conflict when two developers edit the same line of `appsettings.json` in different branches, and ensure the correct configuration value is retained for the target environment.

## Scenario

| Branch | Developer | New value | Target |
|---|---|---|---|
| `feature/prod-config` | Dev A | `ProdDB` | Production |
| `feature/staging-config` | Dev B | `StagingDB` | QA |

Both edit the same `ConnectionString` line relative to the common ancestor (`DevDB`). The second merge triggers a real conflict.

## What This Demonstrates

- Triggering a real merge conflict
- Inspecting three stages: `git show :1:appsettings.json` (base), `git show :2:appsettings.json` (ours), and `git show :3:appsettings.json` (theirs)
- Manual conflict resolution by removing conflict markers
- JSON validation before staging
- Staging and committing the merge
- Bonus: `git merge --abort` and `git checkout --theirs`

## Branches

| Branch | Purpose |
|---|---|
| `main` | Final state with resolved merge (`ProdDB` retained) |
| `feature/prod-config` | Dev A — `ProdDB` |
| `feature/staging-config` | Dev B — `StagingDB` |
| `scratch/abort-demo` | Bonus: abort and theirs demonstration |

## Reproduce It Yourself

```bash
git clone <this-repo-url>
cd merge-conflict-resolution-lab
git log --oneline --all --graph
```

Replace `<this-repo-url>` with your actual repository URL.

## Strategy Comparison

| Strategy | When | Risk |
|---|---|---|
| Manual merge | Most conflicts | Low |
| `git merge --abort` | Panic button | None |
| `git checkout --ours` | HEAD is correct | High |
| `git checkout --theirs` | Incoming version is correct | High |
| `git mergetool` | Complex conflicts | Medium |
| Split configuration | Prevention of repeated conflicts | Low |

## Key Principle

**The decision is environment-driven, never mechanical.** The correct value depends on where the merge lands (production vs. staging), not on chronology.

## Best Practices (Prevention)

- Split configuration by environment: `appsettings.Production.json` and `appsettings.Staging.json`
- Move secrets out of Git; use environment variables or a key vault
- Use `.gitattributes` for sensitive files where appropriate
- Make small, frequent merges; rebase `main` into feature branches regularly
- Use `CODEOWNERS` for configuration files
- Add CI validation: run `python -m json.tool appsettings.json` on every pull request

## License

MIT — see `LICENSE`.
