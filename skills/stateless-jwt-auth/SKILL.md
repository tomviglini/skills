---
name: stateless-jwt-auth
description: "Implement or review password-based, application-issued JWT authentication with revocation after credential changes. Use for password storage, login, token validation, and password resets; excludes unrelated UI and external-provider-only authentication."
---

# Stateless JWT Authentication

Require application-signed JWTs, salted password storage, and token validation against the user's current credentials. These are implementation requirements for projects adopting this model. Framework facilities satisfy them only when their actual behavior covers the contract. Keep the design independent of language and framework; use maintained cryptographic libraries rather than implementing primitives.

Favor stateless processing: store no ordinary access-token or session records, and reject JWTs that fail local signature or claim checks before accessing the user database. Current user state remains necessary for credential-change revocation; do not describe the complete authorization path as database-free.

Keep cryptographic validity separate from eligibility for access. A password change does not break the application's JWT signature. An earlier token may still pass signature and time checks, but must be denied access when its binding no longer matches the user's current credentials. Both stages are required.

## Scope the work

- During initial adoption or an explicit authentication review, trace password persistence, login, token issuance, protected entrypoints, credential changes, and key ownership. Identify which requirements already work and which are missing. Register the project's conditional skill reference through its existing adoption procedure; do not create a separate installer or adoption report.
- During ordinary use, reuse settled choices and inspect only affected flows and their necessary callers. A UI change or an endpoint using an unchanged, compliant authentication boundary does not require another authentication audit.
- For an authorized implementation, complete the missing mechanisms needed by that scope and verify their behavior. Merely installing this skill, choosing a framework, or writing a policy does not establish compliance. A review-only task reports gaps without changing runtime behavior. Do not silently replace an existing identity provider, session system, or public token contract during installation.
- Respect explicit project exceptions and resolved choices. For an unresolved conflict, identify the exact requirement and affected behavior, then ask whether to migrate or record a scoped exception before the disputed change. Do not invent exceptions or call an incomplete implementation compliant. Reuse already authorized migration choices.

Read [cryptographic choices and lifecycle](references/cryptography-and-lifecycle.md) when establishing or changing password hashing, credential binding, algorithms, key rotation, or credential-update behavior. Routine work can reuse documented choices.

## Preserve the benefits of stateless tokens

- Make a request independent of the server instance that issued its token. Replicas use consistent trusted keys, validation rules, and authoritative user state; require neither sticky sessions nor process-local session memory. Restarts and scaling must not invalidate tokens by regenerating keys.
- Verify signatures and self-contained claim constraints locally. Do not add a per-request issuer/introspection call or database token registry to establish whether an application-issued access token was signed correctly. Load trusted verification keys independently of individual bearer requests.
- Use verified claims for stable identity and issuance facts. Keep tokens compact and use standard JWT encodings and claims where appropriate. Do not fetch the same facts again merely because a token was used; current credentials and mutable authorization still require their defined freshness checks.
- Minimize the remaining state work: fetch only required current user fields and reuse the resulting authenticated context within the request. Avoid duplicate signature checks, user reads, and binding checks inside the same trust boundary. Never reuse authentication results across requests in a way that bypasses credential changes.
- Ordinary token use must not require storing the token, updating a session, extending an expiry, or writing activity state. Expiration is checked from the token without an expired-session cleanup job. Product-required audit or abuse-control storage is a separate requirement, not justification for a token registry.
- Reuse a valid token for its agreed lifetime. Do not mint a replacement on every response, extend expiry implicitly, or make an expired access token sufficient authority for renewal. Add refresh/replay-control state only for an explicit lifecycle requirement.
- With asymmetric signing, preserve the separation between issuers and public-key signature verifiers. Full credential-bound authentication still needs the trusted second stage; public-key verification alone must not bypass it. Do not promise offline authentication or database-independent availability for this model.

## Required password storage and login

- Store one complete encoded password record containing the algorithm, work parameters, a cryptographically random salt, and the derived hash, using PHC or an equivalent library-managed format. Generate a fresh salt for each newly hashed password, including replacements. Do not add a second per-user salt field or manually extract and manipulate the embedded salt.
- Use a password-specific, adaptive hashing algorithm with reviewed parameters. Prefer the framework's suitable implementation and configuration. Do not store plaintext or reversibly encrypted login passwords, use a fast general-purpose hash as the password hasher, or double-hash a value already managed by the framework.
- At login, verify the supplied password through the hasher's verification operation, which reads the saved salt and parameters. This happens before issuing a JWT. Preserve effective login rate limits and generic credential failures; do not expose password records in responses or logs.
- Keep plaintext passwords confined to credential-processing operations. Later bearer requests use the existing encoded record for credential binding; they neither request the password again nor rerun the expensive password-hashing operation.

## Required token issuance and credential binding

- Sign JWTs with application-owned key material common to the application's users, separate from user passwords. This is the application-key role; variable names and a supported symmetric or asymmetric scheme are project choices. Signing secrets/private keys stay server-side.
- Include an immutable user identifier, issuer, intended audience, issued-at time, bounded expiration, an explicit access-token type, and a credential-binding claim. Document their names and validation rules. Minimize other claims; never include the password or its stored hash record in a token.
- Bind the token to the entire current encoded password record, including its embedded salt and parameters. Use a maintained framework mechanism with equivalent properties, or the HMAC construction in the reference. A signature plus user-existence check, the salt alone, or an unrelated user version field does not satisfy this contract.
- Derive the binding from the credential state actually authenticated. If login performs a verified password rehash, use the newly persisted record. Never verify an old password and then bind the token to a concurrently replaced credential that was not authenticated.
- Keep access, refresh, password-reset, and email-verification purposes distinct. A valid signature for a different token purpose must not grant access. Do not add refresh tokens or session storage unless the requested product behavior needs them.

## Required request order

Validate required fields for presence, expected type, and accepted value. For example, expiration must be present and valid; checking it only when supplied is insufficient. Use the JWT library or framework's existing validation facilities, adding only checks they do not cover. This contract requires effective validation, not a particular schema format or validation tool, and does not require duplicating checks already guaranteed by the library.

1. **Validate without user-state access.** Parse the bearer credential with bounded input handling and verify the JWT using configured application keys and a fixed algorithm allowlist. Require a valid signature and the expected claim types, issuer, audience, access-token type, subject, binding, and time limits, including expiration and any not-before constraint. Use a bounded clock tolerance. Decoding alone is insufficient.
2. **Reject before the database.** JWTs failing these local checks, including missing, malformed, tampered, expired, or wrong-purpose tokens, must fail before user, password, session, or revocation lookups. Resolve signing keys from trusted configuration or an already trusted key cache, not a user lookup or a token-supplied URL. If the middleware stack also loads a user or session automatically, fix the ordering for these entrypoints.
3. **Read necessary current state.** Only after step 1 succeeds, resolve the verified subject and fetch the user's current password record and applicable account status. Reuse a suitable existing request-scoped lookup rather than querying twice. Reject missing, ineligible, or credential-less users and unavailable required state; do not fall back to signature-only access.
4. **Check credential binding.** Recompute it from the current stored record and compare using a constant-time library operation. Reject a mismatch or missing binding. Then establish the authenticated identity and apply current resource authorization; a JWT does not by itself establish permission.

Apply this boundary consistently to protected HTTP handlers and any other entrypoint accepting these tokens. Expensive domain queries and side effects follow successful authentication. The no-lookup gate concerns authentication state; it does not prohibit independent logging or abuse controls required by the project.

## Required revocation and verification

- After a password change, reset, or rehash commits, deny access on subsequent validations of tokens bound to the previous record, even when their signatures and time claims remain valid. Keep the shared application signing key unchanged for a user's credential change. Never edit only the embedded salt or rehash the stored hash to simulate rotation. Coordinate credential writes and any credential cache so stale state cannot silently keep old tokens usable. Use authoritative reads unless equivalent freshness is established.
- Client-side token deletion does not revoke a copied token. This model revokes by credential change across the user's tokens; per-device or arbitrary per-token revocation requires additional design. Do not introduce that state as an incidental implementation detail.
- For implementation changes, use the project's checks and the relevant cases in [verification scenarios](references/verification.md). In particular, demonstrate that JWTs failing local signature or claim checks cause zero authentication-state lookups and that previously valid tokens fail after the stored credential changes. Reuse unaffected evidence; reviews can identify missing tests without building a new harness.
- Update affected authentication documentation and durable decisions with the actual contract, key roles, credential-change effects, and remaining gaps. Report what was implemented and tested; distinguish static review, automated checks, and unverified deployment behavior.
