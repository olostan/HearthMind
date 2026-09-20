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

## 2. Default trust boundary

Default:
```text
home LAN / private overlay network
        │
        └── HearthMind core + evidence + models
```

No public Internet exposure is required.

## 3. Data classes

Suggested classification:
- PUBLIC: documentation/model metadata;
- INTERNAL: operational metrics;
- HOUSEHOLD: timelines, generic events;
- SENSITIVE: raw media, presence, routines;
- BIOMETRIC: face embeddings/identity enrollment;
- SECRET: tokens/passwords/keys.

Policies can depend on class.

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

## 5. Camera/zone policy

Example:
- driveway: cloud VLM permitted;
- kitchen: cloud only after identity redaction;
- living room: local only;
- private bedroom: semantic analysis disabled.

The policy engine must be executable, not advisory.

## 6. Identity privacy

Known family-member enrollment is explicit.

Unknown visitors:
- may remain transient;
- may receive anonymous ids;
- biometric retention depends on policy.

Never require persistent face storage for all visitors.

## 7. Encryption

Recommended:
- full-disk encryption for the host;
- encrypted transport for remote API;
- secrets in restricted config/secret store;
- database/storage backups encrypted when leaving host.

## 8. Authorization

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

## 9. Audit log

Record:
- privacy-policy changes;
- cloud dispatch;
- identity enrollment/deletion;
- correction author;
- evidence deletion;
- remote login/security events.

Avoid logging raw media content.

## 10. Retention

Configure separately:
- original evidence;
- thumbnails/crops;
- derived embeddings;
- semantic history;
- audit history.

Derived state may outlive media only when policy allows and provenance semantics remain clear.

## 11. Safe defaults

- no public ports;
- cloud disabled;
- minimal logging;
- local models preferred;
- explicit identity enrollment;
- conservative visitor retention;
- evidence deletion disabled until semantics are implemented safely.
