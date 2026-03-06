# DevOps — context

## Scope

- CI config, Docker/infra, deployment scripts, env config.
- Do **not** own: application code (frontend/backend), security policy (security subagent).

## File patterns

- CI/CD: `.github/**`, `.gitlab-ci.*`, `**/Jenkinsfile`, `**/.circleci/**`, `**/azure-pipelines.*`
- Containers: `Dockerfile*`, `docker-compose*.yml`, `**/*.yaml` (K8s, Helm)
- Scripts: `scripts/**`, `**/deploy.*`, `**/build.*`
- Env: `.env.example`, `**/config/**` (infra config only)

## References

- See `../shared/architecture.md` for deployment and environment topology; `../shared/coding-standards.md` for script style.
