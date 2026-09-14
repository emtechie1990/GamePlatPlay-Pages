# Claude security instructions

These requirements are mandatory for every code change in this repository.

## Security controls

1. **Path Traversal Prevention**
   - Never pass request-controlled paths directly to filesystem, object-storage, archive, or template APIs.
   - Prefer server-generated identifiers and a database ownership lookup.
   - Decode, normalize, and validate before I/O. Use an allowlist and verify resolved local paths remain under the intended base directory.
   - Reject absolute paths, dot segments, backslashes, NUL bytes, encoded traversal, and unapproved storage-key characters.

2. **Local File Inclusion (LFI) Prevention**
   - Never let request data select a Python module, PHP include, executable, template file, or server-side script.
   - Use a fixed routing map or hard-coded imports. Treat basename extraction as defense-in-depth, not authorization.
   - Uploaded content is data only; never execute or import it.

3. **Server-Side Request Forgery (SSRF) Prevention**
   - Never fetch a complete URL supplied by a client unless the feature explicitly requires it.
   - Prefer fixed provider endpoints or an exact hostname allowlist.
   - Allow only required schemes and ports; reject credentials, IP literals, localhost, private/link-local/loopback destinations, and non-HTTP schemes.
   - Disable redirects or validate every redirect target using the same policy.
   - Do not send credentials to a host selected by client input.

## Required verification

Before declaring work complete:

- Inventory every changed file/network boundary and confirm the rule above is enforced at the final sink.
- Add or retain negative tests for `../`, encoded traversal, Windows separators, absolute paths, NUL bytes, dynamic include/module selection, localhost, cloud metadata, private IPv4, IPv6 loopback, URL credentials, and redirect pivots when applicable.
- Run the repository's complete automated test suite. For Python backends, also run:
  ```bash
  python -m pytest backend/tests -q
  ```
- Report the exact commands and outcomes. Do not weaken a control merely to make a test pass.

References:
- https://owasp.org/www-community/attacks/Path_Traversal
- https://owasp.org/www-community/attacks/Testing_for_Local_File_Inclusion
- https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html
