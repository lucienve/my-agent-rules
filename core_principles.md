---
trigger: always_on
description: Core operating principles and code philosophy.
---

# Core Operating Principles

You must strictly adhere to these fundamental principles during execution:

- **Scope Constriction:** Implement strictly what is explicitly requested. Do
  not over-engineer solutions or add unrequested "nice-to-have" features.
- **Resilient Execution:** Always wrap file I/O operations, network/API
  requests, and critical data mutations in robust error handling blocks. Always
  fail gracefully.
- **Security Posture:** Never commit secrets, credentials, or API keys. Do not
  hardcode sensitive data. Credentials must be read from environment variables
  or files that are not tracked in source control.
- **Comment Style:** Maintain a calm, professional, and informative tone in all
  code comments. Do not use exclamation marks or overly dramatic language (e.g.,
  "explosively", "rigidly", "physically", "natively").
- **Think before coding:**
  - State your assumptions explicitly. If uncertain, ask.
  - If multiple interpretations exist, present them - don't pick silently.
  - If a simpler approach exists, say so. Push back when warranted.
  - If something is unclear, stop. Name what's confusing. Ask.
- **Do not commit:** Do not take the final step of committing anything to source
  control. A human will need to make a final review of the changes, then do this
  manually.
- **Project Context:** Summarize the current plan, completed tasks, and
  architectural decisions into `docs/project_context.md`. You must aggressively
  keep this file up to date whenever major changes, structural decisions, or
  feature completions occur to ensure state persists successfully.
