# Open Source Project Principles

## 1. Intended identity

HearthMind should become an extensible local-first family intelligence platform, not a vendor-specific camera integration.

## 2. Extension points

Long term:
- source adapters;
- perception processors;
- model providers;
- intelligence modules;
- context tools;
- notification channels;
- Home Assistant integration;
- hardware workers.

## 3. Stable contracts before plugin SDKs

Do not publish a broad plugin API prematurely.

First stabilize:
- Evidence;
- Observation;
- Episode;
- Belief;
- Pattern;
- Insight;
- Processor contract;
- model gateway;
- privacy policy boundary.

## 4. Privacy as ecosystem requirement

Third-party modules must declare:
- data classes consumed;
- external network use;
- models/providers used;
- persisted outputs;
- retention behavior.

The core should enforce permissions rather than trusting documentation.

## 5. Hardware diversity

The project should remain usable on:
- modest x86 mini-PCs;
- newer AMD APUs;
- NVIDIA workers;
- Apple Silicon workers;
- optional cloud services.

No one accelerator should become mandatory for core operation.

## 6. Reproducibility

Recommended extension metadata:
- semantic version;
- model checksums;
- license;
- supported architecture;
- resource requirements;
- input/output contract;
- evaluation results.

## 7. Community data

Do not encourage users to upload private household video to public bug reports.

Prefer:
- synthetic reproduction;
- locally generated debug bundles with redaction;
- structured processor metadata.

## 8. Licensing

The repository currently has no selected license. A license decision should be made before inviting broad external contributions or publishing releases.

Model licenses must be reviewed independently from the HearthMind source license.
