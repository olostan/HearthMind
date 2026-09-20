# Model Strategy

## 1. Principle

Models are replaceable processors, not the architecture.

HearthMind must continue to function when:
- a model is unavailable;
- a model is upgraded;
- a local model is replaced by a cloud model;
- a cloud provider is disabled;
- hardware capabilities change.

## 2. Model classes

Potential model classes include:
- object/person detector;
- tracker;
- face detector;
- face embedding/recognition;
- OCR;
- pose/action estimator;
- image/text embedding;
- speech-to-text;
- audio event classifier;
- vision-language model;
- text LLM;
- anomaly/pattern model;
- future personalized adapters.

Each class should have a narrow processor contract.

## 3. Local-first routing

Default preference:
1. deterministic/non-generative local logic;
2. small local model;
3. larger local model;
4. optional external model.

Escalation is based on:
- privacy policy;
- uncertainty;
- importance;
- expected quality gain;
- current compute availability;
- cost.

## 4. Model registry

Store for each model:
- logical model id;
- provider/backend;
- upstream model name;
- version/revision;
- quantization;
- license;
- checksum;
- supported modality;
- expected hardware;
- evaluation results;
- status: candidate/production/deprecated.

## 5. Reproducibility

A ModelRun should record:
- model id/revision;
- processor version;
- prompt version where relevant;
- decoding/inference parameters;
- input references;
- configuration hash;
- worker/hardware metadata when useful.

Exact byte-for-byte replay is not always possible with generative models; semantic lineage is still required.

## 6. VLM use

Use VLMs for:
- temporal activity interpretation;
- ambiguous object/action semantics;
- structured episode reconstruction;
- specialized domain passes.

Do not use VLMs for:
- basic timestamp filtering;
- relational joins;
- simple counts/statistics;
- state machine transitions when deterministic logic suffices.

## 7. Multi-pass semantic analysis

For important episodes, separate concerns:
- chronological action pass;
- object/state-change pass;
- domain-specific pass;
- verifier pass.

This reduces one-shot prompt overload and makes disagreements visible.

## 8. Model promotion

A new model becomes production only after:
- evaluation on fixed golden data;
- evaluation on recent drift data;
- resource/latency comparison;
- privacy/license review;
- regression review.

## 9. Hardware abstraction

Backend adapters may include:
- CPU;
- Vulkan;
- CUDA;
- ROCm where supported;
- MLX;
- remote API.

Domain code does not branch on these backends.

## 10. Future training

Training is orchestrated by HearthMind but may run on:
- the home core;
- a temporary Mac/MLX worker;
- an NVIDIA worker;
- optional rented/cloud accelerator.

The scheduler owns the job; the compute node is replaceable.
