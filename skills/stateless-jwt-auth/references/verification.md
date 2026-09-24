# Authentication Verification Scenarios

Use these scenarios for the authentication behavior affected by an implementation change. Initial implementation must establish the complete contract; ordinary changes select relevant regressions and reuse evidence for unchanged mechanisms. Exercise observable decisions, not merely the presence of configuration or method names.

| Scenario | Required observation |
| --- | --- |
| Two users register with the same password | Their encoded records differ because independently generated salts are embedded; both passwords verify correctly and incorrect passwords fail. |
| Password persistence and responses | One complete library-managed record is stored; no separate salt is required, no double hashing occurs, and password records stay out of user responses, logs, and decoded JWT payloads. |
| Successful login | The supplied password is verified before issuance. The token is signed by the application and bound to that user's persisted credential record. |
| Missing, malformed, altered, expired, or unsigned token | Authentication fails with zero user, credential, session, or revocation lookups. Instrument the boundary or repository calls to establish ordering. |
| Wrong algorithm/key, unknown key identifier, issuer, audience, token purpose, or invalid/missing required claims | Authentication fails before authentication-state access; no token-directed key retrieval or signature-only fallback occurs. |
| Unchanged credentials and valid JWT | Signature and claim validation precede the necessary user lookup; binding matches, and resource authorization still applies. No password KDF runs during the bearer request. |
| Request traverses several handlers within one trust boundary | The verified identity and current user record are reused; no duplicate authentication lookup, introspection call, session write, or unsolicited token renewal is introduced. |
| Different replica or restarted process handles the next request | A still-valid token authenticates with consistent keys and current user state, without sticky sessions or process-local token records. |
| Validly signed token with missing or mismatched binding | Access is denied, including when a binding is copied from a different user with the same plaintext password. A missing/invalid binding format can be rejected before lookup. |
| Password change or reset commits | An earlier unexpired token still passes verification with the unchanged application signing key, but is denied access by the credential-binding check. Other users' tokens remain usable. Authentication with the new password produces a usable token. Test through affected entrypoints. |
| Rehash, even with unchanged plaintext | Old tokens fail; any newly issued token uses the persisted new record. Unrelated profile updates leave credentials and existing tokens unchanged. |
| Login races with credential replacement | Verification of the old password never produces a token valid against a newly replaced, unauthenticated credential. |
| Deleted/ineligible user, missing credentials, or unavailable required state | Authentication fails after the cryptographic gate; infrastructure failure never enables signature-only access. |
| Credential caching, if present | A committed credential change is observed across instances without accepting an old token from stale cached state. |
| Key rotation or binding migration, when changed | Retired keys and tokens missing required binding follow the agreed rejection policy; only explicitly approved, bounded transitions remain accepted. |
| Token expiration | Access is rejected without a user lookup or session-cleanup dependency; an expired token cannot renew itself. |

Use synthetic accounts, keys, and passwords in an isolated test environment. Never test by resetting a real user's credentials or exposing existing tokens. If a review cannot execute these cases, identify the missing evidence and implementation gaps rather than declaring the model enforced.
