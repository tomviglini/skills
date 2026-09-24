# Cryptographic Choices and Lifecycle

Read the relevant sections when selecting or changing cryptographic mechanisms, credential binding, keys, or credential-update behavior. These are abstract requirements and selection criteria, not language-specific implementation examples.

Documentation reviewed: **2026-09-23**. Recommendations must be checked against the consuming project's resolved libraries and deployment constraints when those choices change. No application implementation or cryptographic interoperability test is supplied by this skill.

## Password records

Prefer Argon2id for new password storage. An existing suitable scrypt implementation can remain after reviewing its parameters; preserve a supported password format rather than forcing a library migration merely to match a preference. Benchmark memory and latency under realistic authentication concurrency. Framework defaults alone do not establish adequate work factors. OWASP currently gives Argon2id at 19 MiB, two iterations, and parallelism one as one baseline, and lists alternative configurations, including scrypt choices. Use the maintained [password storage guidance](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) when selecting parameters.

Let the password library generate the salt and encode it with its algorithm, parameters, and derived hash in a single stored value. Preserve that value exactly. At login, the library reads the embedded salt to verify the supplied plaintext; generating a new salt and comparing two newly created encoded records is incorrect. The salt is not a secret key, and an application signing key is not a substitute for password salting.

As a concrete framework precedent, AdonisJS core **7.4.0** with `@adonisjs/hash` **10.1.1** was inspected locally: its scrypt driver generates a random salt (16 bytes by default), serializes salt, parameters, and hash into PHC, and extracts them during verification. This was source inspection, not a runtime test or a recommendation to copy its default work factors. The [official hashing documentation](https://docs.adonisjs.com/guides/security/hashing) describes this format and library-managed verification. Other frameworks may meet the same contract through their native APIs; do not require AdonisJS or a matching dependency version.

## JWT signatures and keys

Signature and key guidance reviewed: **2026-09-24** against the sources below; no runtime interoperability test was performed.

Use asymmetric signatures to separate issuance authority from verification. Keep the private signing key available only to authorized issuers; services that only verify tokens receive public keys. ES256 (ECDSA P-256 with SHA-256) is a suitable choice when supported. Preserve another suitable asymmetric algorithm when required by the project's libraries or interoperability contract; this skill does not mandate ES256 or a library migration. See [JWA signing algorithms](https://www.rfc-editor.org/rfc/rfc7518.html#section-3).

HS256 shares the same secret between issuance and verification, so every verifier can also create accepted tokens. It does not meet this skill's separation requirement. Retain it only under an explicit project exception or resolved migration choice, with all secret holders trusted to issue tokens and at least 256 cryptographically random key bits. Do not turn an existing symmetric contract into an unapproved migration or silently treat it as equivalent to public-key verification.

Distribute public verification keys through trusted configuration or a public-only key set whose source is configured by the deployment. Public keys are not secret, but their provenance must be authenticated. Keep verification independent of private-key configuration and issuer availability once trusted keys are loaded; do not fetch keys from token-supplied locations. Pin algorithms to their intended key types, and never accept public-key bytes as an HMAC secret. See [JWT security guidance](https://www.rfc-editor.org/rfc/rfc8725.html#section-3).

Pin accepted algorithms, key types, issuer, audience, token purpose, and claim requirements in server configuration. Require expiration and define a maximum lifetime and bounded clock tolerance; do not inherit an arbitrary example lifetime. Reject unsigned tokens. Treat token headers and claims as untrusted until verification, and allow a key identifier to select only from a bounded, configured key set. Never fetch a token-supplied key URL or use a subject to retrieve a signing key from the user database.

Illustrative identifiers for the [issuer and audience claims](https://www.rfc-editor.org/rfc/rfc7519.html#section-4.1):

| Claim | Example | Meaning |
| --- | --- | --- |
| `iss` | `app-auth` | The component that issued the token at login. |
| `aud` | `app-api` | The API intended to receive and accept the token. |

Both roles can belong to the same backend. For an API access token, the audience identifies the receiving API, not the mobile or browser client carrying it. Validate these values against trusted configuration alongside the signature, before database access. Audience validation prevents accepting a token intended for another API even when its signature is valid.

Keep signing, credential-binding, and data-encryption key purposes separate. Use distinct keys or a maintained, documented derivation that separates purposes; do not reuse asymmetric private-key bytes as an HMAC secret. Signing and binding secrets must be independently managed from stored password records. Use the deployment's secret facility and separate environments. [OWASP key management](https://cheatsheetseries.owasp.org/cheatsheets/Key_Management_Cheat_Sheet.html#key-usage) explains key-purpose separation.

## Binding tokens to stored credentials

The binding is an application-defined JWT claim, not a standard JWT feature or something password salting automatically provides. Its purpose is detecting a credential change, not proving that each bearer request knows the plaintext password. A stolen token remains usable until expiry or a relevant revocation event.

Use a framework's existing credential-binding facility only after verifying equivalent behavior. Otherwise, compute an HMAC-SHA-256 through a maintained cryptographic library with a dedicated server secret over a stable, unambiguous encoding of:

- A fixed application/purpose and binding-format version.
- The immutable user identifier, including tenant scope when identifiers are not globally unique.
- The entire stored encoded password record.

Keep field boundaries unambiguous and the encoding identical during issuance and validation. Store the full resulting authentication tag in the signed JWT using a documented encoding. Check its format and length, recompute it from the current record, and compare in constant time. The binding secret should contain at least 256 random bits. Neither the stored password hash, its extracted salt, nor a plain unkeyed hash of the password record belongs in the token as a replacement for this binding.

The application-level use of HMAC here is a design choice of this skill. [Django's credential-dependent session invalidation](https://docs.djangoproject.com/en/6.0/topics/auth/default/#session-invalidation-on-password-change) is a precedent for an HMAC over stored password data, but its session mechanism does not implement this JWT claim. Use standard library primitives; do not invent a signature algorithm or concatenate an application secret and password into a homemade signing key.

Although ES256 signature verification needs only a public key, full authentication in this model also needs credential-binding verification and current user state. A service holding only a public key cannot independently enforce credential-change revocation. Keep that second stage at a trusted authentication boundary rather than distributing password records to every service.

## Credential changes, caches, and migration

Changing the encoded record changes its binding. This includes resets, algorithm/work-factor upgrades, and hashing the same plaintext again with a fresh salt. The old JWT's application signature can remain valid; access fails at the credential-binding stage. Do not rotate the shared signing key to revoke one user's access. Coordinate a successful login's optional rehash and token issuance so the token uses the persisted, verified result. During a concurrent password reset, do not issue a token bound to the new record merely because an older credential passed verification. A token bound to the old record must no longer grant access once the change commits.

Ordinary profile edits must not rewrite password records. Do not manually rotate an embedded salt: a replacement record requires hashing the supplied plaintext password through the password library. Logout without the plaintext password cannot safely be implemented by arbitrarily rewriting that record. A separate revocation version, device registry, or denylist would extend this model and needs a deliberate project decision; it cannot replace the required credential binding without an explicit exception.

Use authoritative credential reads by default. A time-to-live cache by itself introduces a revocation delay; it does not meet immediate validation against current credentials. Any cache must preserve freshness across instances when credential updates commit. If required state is unavailable, fail closed. Requests already authenticated before a change may complete; document stronger cancellation needs separately rather than promising retroactive cancellation.

When adopting the binding in a system with existing JWTs, identify compatibility and logout effects before changing the contract. Tokens missing the required binding must be rejected under the new contract. An explicitly approved, bounded legacy migration may be recorded as a temporary exception; never silently allow signature-only fallback.

Key rotation is separate from credential changes. Define whether retiring a signing or binding key intentionally invalidates its tokens or uses a bounded overlap. Accept old keys only while authorized, never to recover from a credential mismatch or known compromise. Resolve keys from trusted configuration before user-state access; arbitrary token key identifiers must not cause database or network lookups.

## Distributed validation and stateless limits

JWT carries verifiable issuance facts with the request. Local verification can avoid token introspection round trips, and stable key distribution allows replicas to accept the same token without replicating per-session records. [RFC 9068](https://www.rfc-editor.org/rfc/rfc9068.html#section-1) describes local JWT access-token validation and interoperability in OAuth deployments. It is a precedent for these benefits, not a requirement to introduce OAuth or a claim that this application's private binding conforms to that profile.

An edge proxy is a useful public-key-only verifier: it can reject invalid bearer tokens before they consume backend authentication or database work. Give it trusted public keys and the expected algorithm, issuer, destination audience, purpose, and time constraints. It needs no signing key, password records, binding secret, or issuer call to perform this preliminary check. Forward tokens that pass to the backend's authentication boundary, which must still enforce the complete contract before protected access. An old token can pass the edge's signature checks after a password change and still be denied by the backend's credential-binding check. Public-key filtering is a valid limited role; do not require credential-state access at the edge merely because the full access decision requires it.

Use standard subject, issuer, audience, and time claims unless an existing client contract requires a coordinated migration. Claims are snapshots: roles, permissions, account eligibility, and credential validity do not update inside an already issued token. Make their freshness policy explicit. Reuse the required current-user read for relevant mutable fields when practical, and keep resource-specific authorization where it belongs. [OWASP REST security](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html#jwt) discusses local token verification and the disconnect between a token and current account state.

Within one service, pass an authenticated context to downstream handlers instead of revalidating at every layer. Across service boundaries, a receiver must independently establish trusted provenance and intended audience; never trust client-supplied identity headers. Where a trusted authentication boundary handles credential freshness, secure its downstream propagation and prevent routes that bypass it. Do not distribute password records or binding secrets to otherwise independent services merely to gain local signature verification.

The required credential read deliberately trades completely offline validation for revocation after credential changes. Removing that read, introducing a stale cache, or falling back to signature-only verification during an outage would change the contract. Signature rejection remains local even when user storage is unavailable; successful authentication requires current state. Evaluate actual costs when making performance claims: token bandwidth, signature computation, and the remaining user lookup still exist. JWT is not inherently faster than every alternative, and a bearer JWT provides neither one-time use nor replay prevention by itself.

## Encryption and transport

A signed JWT normally has readable claims. Keep passwords, stored password records, and secrets out of it; signing does not provide confidentiality. Use HTTPS and the consuming client's appropriate credential storage. Browser cookies require suitable cookie flags and CSRF protection; script-accessible bearer storage has different exposure. Preserve the actual transport contract instead of imposing one storage mechanism on every client. The distinction between signed and encrypted JWTs is specified in [RFC 7519](https://www.rfc-editor.org/rfc/rfc7519.html).

When the task actually requires reversible encryption of other data, prefer authenticated encryption such as AES-256-GCM through a maintained library, with a separate key, non-repeating nonces per key, and authentication-tag verification. Do not add encryption to the JWT or the password record just to satisfy this skill. A requirement to hide JWT claims calls for standard JWE and an explicit interoperability decision. See [OWASP cryptographic storage](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html).
