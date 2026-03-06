# Security — rules

- Report findings by severity; always include concrete location and remediation.
- Do not modify code unless explicitly asked to apply a fix; otherwise only report.
- Prefer readonly or minimal-write mode to avoid unintended changes during audit.
- Check dependency manifests for known vulnerabilities; recommend upgrades or patches.
- Verify no secrets in repo, env, or logs; recommend use of vaults or managed secrets.
- Use project glossary and architecture to avoid misclassifying domain-specific behavior as vulnerability.
- Do not define new security policy without team input; only enforce or recommend based on existing policy.
