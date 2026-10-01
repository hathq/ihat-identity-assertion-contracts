# iHAT Identity Assertion Contracts

Closed Rust contracts for short-lived device identity assertions issued by an identity authority and verified by consumers. Consumers pin issuer, audience, trust keys, and trusted time and verify the complete context. The wire carries pairwise references rather than account IDs, global subjects, session IDs, credentials, or private keys.

`DeviceIdentityAssertionV1` carries a service-specific pairwise subject, registered device ID, public reference to a non-exportable device key, posture revision, and monotonic subject/service/device/session revocation epochs. Maximum TTL is 300 seconds; privileged consumers may require a shorter lifetime.

`CurrentDeviceStatusV1` proves authoritative current state for the same context. TTL is at most 30 seconds. It binds exactly to the assertion nonce, issuer, audience, service, subject, device, proof-key reference, posture, and four epochs. Its signing key can be pinned independently. After signature verification, consumers must call `current_status_matches_assertion` and retain the nonce in durable one-use state.

The crate also owns the bounded authority-host wire v1. `AuthorityRequestV1` and `AuthorityResponseV1` reject unknown fields and share command JCS digests, authentication-free prepared digests, four-byte big-endian framing, and Ed25519 evidence/FreshUv/response verification. Maximum wire size is 262144 bytes, evidence count is 16, response TTL is 30 seconds, and FreshUv/evidence TTL is 120 seconds. Consumers use the strict decoder and exact binding verifier.

`IssueCurrentDeviceIdentityEvidence` accepts the signed endpoint configuration's service, pairwise subject, device, and audience, plus the current sender-key fingerprint and one-use identity nonce/proof ID. TTL is `1..=30` seconds. The authority resolves the authoritative device identity-session slot. `IssueDeviceIdentityEvidence` remains a separate command requiring an explicit `session_ref`.

`EstablishDeviceIdentitySession` establishes or explicitly rotates that slot. `expected_current_session` is a closed tagged enum: `Absent` or `Present { session_ref, session_epoch }`. `Absent` succeeds only for an absent slot; `Present` requires exact reference/epoch CAS. The authority generates the new session ID internally.

```bash
cargo test --locked --offline
cargo clippy --locked --offline --lib -- -D warnings
```
