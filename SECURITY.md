# Application security controls

This repository follows three mandatory server-side security boundaries.

## Path Traversal Prevention

Request values must not become filesystem or object-storage paths. Storage keys should be server-generated and allowlisted. Local paths must be resolved and proven to remain below a dedicated base directory before access.

## Local File Inclusion Prevention

Request values must never select executable code, modules, includes, or templates. Dynamic inclusion is prohibited; fixed routing maps and static imports are required. Uploaded files are treated only as untrusted data.

## Server-Side Request Forgery Prevention

Outbound requests use fixed destinations or explicit hostname allowlists. Client-controlled URLs must be scheme/host/port validated, may not target local or private infrastructure, and may not follow an unvalidated redirect.

## File authorization

Path validation does not replace authorization. Downloads must also be tied to a database record and checked against the requesting user, tenant, case, or a short-lived scoped token.

## Verification gate

Security-sensitive changes require negative regression tests and a complete test run. See `CLAUDE.md` for the required cases and commands.

Security references:
- [OWASP Path Traversal](https://owasp.org/www-community/attacks/Path_Traversal)
- [OWASP Local File Inclusion testing](https://owasp.org/www-community/attacks/Testing_for_Local_File_Inclusion)
- [OWASP SSRF Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)
