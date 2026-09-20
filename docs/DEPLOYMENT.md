# Deployment and Hardware

## 1. Initial target

Reference first deployment:

- Linux host;
- AMD Ryzen 5 PRO 5650GE-class 6C/12T CPU;
- 16 GB RAM minimum;
- 32 GB recommended;
- integrated GPU;
- NVMe SSD;
- always-on;
- Docker Compose.

Real-time processing is not required.

## 2. Suggested host responsibilities

The first host may run:
- PostgreSQL;
- API/core;
- ingestion;
- scheduler;
- ML worker;
- local model server;
- web UI;
- evidence storage.

## 3. OS

Recommended:
- Debian stable or Ubuntu LTS server;
- no desktop environment required;
- SSH/private overlay-network administration.

## 4. Containerization

Use Docker Engine + Compose initially.

Goals:
- reproducible service versions;
- bounded resources;
- simple upgrades;
- clear mounts/secrets.

Do not require Kubernetes for a single household node.

## 5. Storage

Separate:
- database;
- immutable evidence;
- derived cache;
- model files;
- backups.

Original media growth will likely dominate storage.

Configure retention before assuming unlimited history.

## 6. Hardware acceleration

Use opportunistically:
- VAAPI for AMD video decode where supported;
- Vulkan/local model acceleration where beneficial;
- benchmark CPU vs iGPU per model.

Do not make v0.1 correctness depend on a specific accelerator.

## 7. Expected processing character

Motion-triggered clips already contain elevated semantic density. A one-hour cumulative footage workload may require roughly one to several hours of deep processing on reference hardware depending on model choice and activity complexity.

The product explicitly tolerates backlog.

## 8. Future worker model

Potential future nodes:
- NVIDIA/CUDA vision/VLM worker;
- Mac/MLX heavy inference/training worker;
- newer AMD APU worker;
- optional cloud model.

Scale by routing independent jobs/layers to appropriate workers, not by splitting one model across weak Ethernet-connected machines.

## 9. Networking

Default:
- LAN/private overlay only;
- no public service exposure;
- authenticated TLS/API when crossing trust boundaries.

## 10. Backups

At minimum:
- PostgreSQL backup;
- configuration/policy backup;
- identity enrollment metadata backup if desired.

Evidence backup policy is user-defined due to size.

## 11. Upgrade strategy

Measure before buying hardware.

Use telemetry to determine whether bottleneck is:
- decode;
- detector;
- VLM;
- transcription;
- storage;
- database;
- training.

Then add hardware optimized for that bottleneck.
