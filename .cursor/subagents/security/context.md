# Security — context

## Scope

- Security review, recommendations, and alignment with policy.
- Do **not** own: implementing fixes (defer to frontend/backend/devops).

## File patterns

- Auth and permissions: `**/auth/**`, `**/middleware/**`, `**/guards/**`
- Sensitive handling: any file touching secrets, PII, payments, or credentials
- Dependencies: `package.json`, `**/requirements*.txt`, `**/Cargo.toml`, `go.mod`
- Config that may contain secrets or security flags: `**/config/**`, `*.env.example`

## References

- See `../shared/architecture.md` for trust boundaries and data flow; `../shared/coding-standards.md` for secure coding expectations.
