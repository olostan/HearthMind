# Security Policy

HearthMind processes unusually sensitive household data: private video/audio, identity, face embeddings, routines, presence, and inferred household state. Security and privacy failures therefore have unusually high impact.

## Reporting vulnerabilities

Until a dedicated security contact is published, please report vulnerabilities privately to the repository owner rather than opening a public issue containing exploit details or household-data exposure.

## Security principles

### Local-first trust boundary
Core functionality must operate without exposing household evidence to the public Internet.

### Least privilege
Each component should receive only the credentials, files, camera scopes, and model access it requires.

### Secrets
Secrets must not be:
- stored in Git;
- printed in logs;
- embedded in container images;
- returned by APIs;
- included in model prompts unless explicitly required and policy-approved.

### Biometric data
Face embeddings and identity mappings are sensitive.
- encrypt storage where practical;
- avoid logging vectors;
- restrict API access;
- support deletion/rotation;
- never send biometric vectors to external providers unless explicitly configured.

### External model providers
Cloud inference requires:
1. privacy policy authorization;
2. context minimization;
3. clear provider identity;
4. auditable model-run records.

### Media access
Original evidence should not be publicly addressable by default. API access should use authenticated authorization and short-lived access mechanisms when remote viewing is supported.

### Auditability
Security-sensitive actions should be auditable:
- external inference dispatch;
- identity enrollment/correction;
- evidence deletion;
- policy changes;
- account/role changes.

### Network exposure
The default deployment should expose no service directly to the public Internet. Remote access should be provided through a private network/VPN or a deliberately configured authenticated gateway.

## Non-goal

HearthMind v0.1 is not a certified security, medical, childcare, or emergency-response system. Insights must not be treated as guaranteed real-time safety alerts.
