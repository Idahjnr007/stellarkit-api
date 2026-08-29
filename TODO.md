# TODO - Issue #397: New Endpoint GET /transaction/:hash/effects

- [x] Inspect existing transaction routes and response/normalization utilities
- [x] Implement GET /transaction/:hash/effects route
  - [x] Validate :hash is 64-char hex before Horizon call
  - [x] Fetch all effects for transaction hash via Horizon
  - [x] Normalize each effect with: effectId, type, account, createdAt, plus type-specific fields (best-effort)
  - [x] Return { success: true, data: { effects: [...], total } }
  - [x] Return 404 with clear message when transaction hash does not exist
- [x] Add/Update tests for the new endpoint (shape + validation + 404 behavior)

- [x] Ensure routing is registered in src/index.js (and docs list if applicable)
- [x] Run targeted unit tests for the endpoint only (no build)
TODO: Add Output Encoding to Error Message Interpolation

Description:
Safely encode all user-provided values interpolated into error messages to prevent malformed JSON responses and unexpected rendering when inputs contain special characters.

Requirements:

- Add a centralized output-encoding/escaping utility for user-controlled values.
- Apply encoding to all error messages that echo:
  - Account IDs
  - Asset codes
  - Policy names
- Ensure the following characters are safely escaped:
  - "<"
  - ">"
  - """
  - "\"
  - "/"
- Preserve the JSON structure of error responses regardless of malicious input.
- Ensure encoded values remain readable enough for debugging while being safe for client rendering.

Tests:

- Add unit/integration tests covering each special character for:
  - Account ID error messages
  - Asset code error messages
  - Policy name error messages
- Test combinations of special characters and malicious payloads.
- Verify every response remains valid JSON and the echoed value cannot alter the response structure.
- Verify existing normal error messages remain unchanged for safe inputs.

Acceptance Criteria:

- No user-controlled value is interpolated into an error response without encoding.
- "<", ">", """, "\", and "/" are properly escaped.
- Malicious input cannot break JSON encoding or client-side rendering.
- All relevant tests pass.
- Existing API error response formats remain backward compatible.

