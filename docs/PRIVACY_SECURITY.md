# Privacy and Security Architecture

## 1. Threat model

HearthMind may contain:
- private video/audio;
- children/family identity;
- face embeddings;
- routines/presence;
- object/location history;
- inferred habits;
- access credentials.

Compromise can reveal far more than ordinary smart-home metadata.

Assume both of these failures are possible:
- the HearthMind box itself is compromised;
- an optional external worker or cloud provider is compromised or behaves unexpectedly.

The architecture should therefore minimize blast radius, not merely prevent first compromise.

## 2. Default trust boundary

Default:
```text
home LAN / private overlay network
        │
        └── HearthMind core + evidence + models
```

No public Internet exposure is required.

The home core is the most trusted environment because it holds authoritative state, evidence, and policy.
Trust boundaries are ultimately **per device/process boundary, not merely per LAN segment**.
Every other execution location is less trusted unless explicitly hardened and allowed.

## 3. Data classes

Suggested classification:
- PUBLIC: documentation/model metadata;
- INTERNAL: operational metrics;
- HOUSEHOLD: timelines, generic events;
- SENSITIVE: raw media, presence, routines;
- BIOMETRIC: human face embeddings/identity enrollment, including any retained recurring-visitor human biometric components;
- IDENTITY_PRIVATE: pet appearance profiles/embeddings, recurring visitor appearance profiles excluding human biometric components;
- SECRET: tokens/passwords/keys.

Policies can depend on class.

Retention, export, backup, and off-box processing rules should all be keyed to these data classes.

## 4. Cloud inference policy

Before any external request:
1. identify provider;
2. identify source camera/location;
3. identify data classes;
4. evaluate household policy;
5. minimize/redact;
6. record an auditable ModelRun.

Possible decisions:
- deny;
- local-only;
- allow anonymized crops;
- allow selected frames;
- allow structured context only.

Allow should be explicit, narrow, and auditable. “Provider is enabled” is not sufficient authorization.

## 5. Camera/zone policy

Example:
- driveway: cloud VLM permitted;
- kitchen: cloud only after identity redaction;
- living room: local only;
- private bedroom: semantic analysis disabled.

The policy engine must be executable, not advisory.

Policy should support defaults at multiple layers:
- household;
- camera;
- zone/location;
- person/identity class;
- job/processor type;
- provider/worker class.

## 6. Identity privacy

Known family-member enrollment is explicit.

Known pet enrollment is explicit and should store only the minimum appearance context needed for household re-identification.

Unknown visitors:
- may remain transient;
- may receive anonymous ids;
- biometric retention depends on policy.

Recurring visitors:
- should default to anonymous profile ids if retained;
- should have short retention by default unless household policy opts in to longer retention;
- may be promoted to named enrollment only by explicit user action.

Never require persistent face storage for all visitors.

Strong defaults:
- family-member enrollment is opt-in;
- visitor biometrics are disabled or short-lived by default;
- recurring visitor appearance retention is short-lived by default;
- pet appearance profiles remain household-private and are never exported in raw form by default;
- identity matching failure falls back to unknown/anonymous rather than forced attribution;
- exported logs/debug bundles never contain raw embeddings.

## 7. Encryption

Recommended:
- full-disk encryption for the host;
- encrypted transport for remote API;
- secrets in restricted config/secret store;
- database/storage backups encrypted when leaving host.

Additionally:
- prefer separate encryption domains or credentials for database, evidence storage, and backups when practical;
- rotate or revoke secrets without rewriting historical evidence;
- never hard-code credentials into images, Compose files, prompts, or processor configs committed to Git.

Encryption at rest helps with disk theft and offline access, but does not by itself protect data from a live compromise of the running host. Operational design should reflect that limitation.

## 8. Compromise-minimizing architecture

When the box is hacked, HearthMind should fail in a way that limits further disclosure.

Recommended guardrails:
- least-privilege service accounts per component;
- separate mounts for evidence, database, derived cache, model files, and backups;
- containers/processes run without unnecessary host privileges, device access, or outbound network access;
- remote administration disabled by default except authenticated private channels;
- cloud credentials absent unless the household explicitly enables an external provider;
- worker nodes receive job-scoped inputs, not unrestricted database access;
- derived crops/thumbnails/exports inherit stricter or equal policy to the source evidence.

The goal is to avoid one compromise automatically granting:
- all historical raw video;
- all biometrics;
- all backups;
- all provider credentials;
- unrestricted off-box exfiltration.

## 9. Authorization

Future multi-user access should use household roles/scopes.

Examples:
- administrator;
- adult member;
- child member;
- guest.

Permissions may limit:
- cameras;
- evidence;
- person history;
- security timeline;
- private integrations.

v0.1 may remain simple, but even the first deployable system should avoid anonymous admin access on any interface reachable beyond localhost.

## 10. Audit log

Record:
- privacy-policy changes;
- cloud dispatch;
- identity enrollment/deletion;
- correction author;
- evidence deletion;
- remote login/security events.

Avoid logging raw media content.

Audit logs should prefer tamper-evident append-only semantics where practical.

## 11. Retention

Configure separately:
- original evidence;
- thumbnails/crops;
- derived embeddings;
- semantic history;
- audit history.

Derived state may outlive media only when policy allows and provenance semantics remain clear.

Retention defaults should be conservative:
- evidence retention must be explicit, not silently infinite;
- biometric retention should be shorter and more restricted than generic observations unless opted in;
- recurring visitor identity-private appearance data should expire faster than enrolled household identity data unless explicitly extended;
- pet identity-private appearance data may outlive visitor appearance data but should remain shorter-lived than source evidence by default unless opted in;
- temporary exports, crops, and debug bundles should expire automatically;
- caches must be safe to delete and regenerate.

## 12. External workers and multi-node privacy

Future multi-node execution must preserve the same privacy semantics as single-box execution.

Recommended trust tiers:
1. same-process / same-box worker;
2. same-host isolated worker;
3. same-LAN household-owned worker;
4. overlay/private-network household-owned worker;
5. cloud provider.

As trust decreases:
- dispatch should require more explicit policy;
- raw evidence access should narrow;
- identity/biometric data should be reduced or removed where possible;
- workers should receive short-lived credentials and task-scoped access;
- audit detail should increase.

A borrowed MacBook, GPU workstation, or temporary training node should be attachable only through explicit registration and revocable credentials. Disconnecting that node must not orphan authoritative state.

## 13. Incident response and recovery

Plan for:
- credential rotation after suspected compromise;
- disabling external dispatch quickly;
- revoking worker registrations;
- invalidating exported/debug artifacts where feasible;
- backup restore without losing semantic lineage;
- user-visible disclosure of what categories of data may have been exposed.

Deletion and recovery procedures must preserve the distinction between evidence deletion, derived invalidation, biometric deletion, and model-training-data removal.

## 14. Safe defaults

- no public ports;
- cloud disabled;
- minimal logging;
- local models preferred;
- explicit identity enrollment;
- conservative visitor retention;
- no automatic recurring-visitor-to-enrolled promotion;
- evidence deletion disabled until semantics are implemented safely.

Additionally:
- no remote worker trust by default;
- no biometric export by default;
- no automatic training/export pipeline by default;
- no broad debug bundle generation without redaction.
