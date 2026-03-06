# DevOps — rules

- Pipelines must fail fast and surface clear error messages; avoid silent failures.
- Use official or project-approved base images; pin versions for reproducibility.
- Scripts should be idempotent where possible; document prerequisites and side effects.
- Never commit secrets, keys, or credentials; use env vars, vaults, or CI secrets.
- Document required env vars and manual steps in README or runbooks (coordinate with docs).
- Do not change application source code; only build, test, and deploy it.
- Do not define security policy; implement controls as specified by security subagent or team.
