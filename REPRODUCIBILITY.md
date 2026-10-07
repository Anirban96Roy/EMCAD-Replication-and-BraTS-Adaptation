# Reproducibility

This document records the configuration and evidence needed to inspect or reproduce the EMCAD screening experiments.

## 1. Pinned EMCAD source

Official EMCAD source commit used throughout the workflow:

```text
26c9c31f731f749b62c5fe83f44376dac75f3aa8
```

The same value is preserved in:

```text
evidence/environment/EMCAD_AUTHOR_COMMIT.txt
```

## 2. Legacy environment

The main author-compatible environment used for the EMCAD experiments was:

| Package | Version |
|---|---|
| Python | 3.8.20 |
| PyTorch | 1.11.0+cu113 |
| torchvision | 0.12.0+cu113 |
| torchaudio | 0.11.0+cu113 |
| NumPy | 1.22.4 |
| timm | 0.6.12 |
| mmcv-full | 1.7.2 |
| ptflops | 0.7.2.2 |

The complete package list is stored in:

```text
evidence/environment/EMCAD_ENVIRONMENT.txt
```

The PVTv2-B2 pretrained encoder weight used by the prepared EMCAD repository was recorded as:

```text
80711cd1b37ffba12bec6c7a2a7c54efe2315ed635e6d2055fd51c3e909ede4d
```

See:

```text
evidence/environment/PVT_V2_B2_SHA256.txt
```

---

## 3. Experiment 1 — CVC-ClinicDB reference replication

### Dataset split

Author-style split:

- 489 training images
- 61 validation images
- 62 test images
- Total: 612

The exact image-level split is preserved in:

```text
evidence/environment/ClinicDB_author_split_manifest.csv
```

### Model

- Encoder: PVTv2-B2
- Decoder: EMCAD
- Binary segmentation
- Decoder kernel sizes: `[1, 3, 5]`
- LGAG kernel size: `3`
- Expansion factor: `2`

### Training protocol

The reference replication used the pinned author code and author-style training configuration, including:

- input size: 352 × 352
- AdamW optimizer
- initial learning rate: 5e-4
- batch size: 8
- 200 epochs
- gradient clipping: 0.5
- multi-scale rates: 0.75, 1.0, 1.25
- cosine learning-rate scheduling
- best checkpoint selected using validation Dice

Five independent training runs were completed.

### Final replication result

| Metric | Mean ± SD |
|---|---:|
| Dice | 94.12 ± 0.23% |
| IoU | 89.45 ± 0.35% |
| Sensitivity | 95.65 ± 0.32% |
| Specificity | 99.62 ± 0.03% |
| Precision | 93.26 ± 0.48% |
| HD95 | 10.10 ± 2.39 |

Published ClinicDB Dice for PVT-EMCAD-B2:

```text
95.21%
```

Replication difference:

```text
-1.09 percentage points
```

Run-level metrics are stored in:

```text
results/experiment_1_clinicdb/clinicdb_5run_results.csv
```

---

## 4. Experiment 2 — BraTS-derived neuroimaging adaptation

### Dataset

Medical Segmentation Decathlon:

```text
Task01_BrainTumour
```

The downloaded archive was verified with:

```text
MD5: 240a19d752f0d9e9101544901065d872
```

Dataset metadata:

- 484 canonical labelled patients
- modality 0: FLAIR
- modality 1: T1w
- modality 2: T1-Gd
- modality 3: T2w

### EMCAD input adaptation

Three channels were selected to preserve the three-channel PVTv2-B2 input:

```text
FLAIR + T1-Gd + T2
```

Binary Whole Tumor target:

```text
WT = label > 0
```

### Patient-level split

Seed:

```text
2026
```

Split:

- 388 train
- 48 validation
- 48 held-out test

Patient leakage check:

```text
PASS
```

The held-out test set consisted of complete 3D volumes and was not used for model selection.

### Preprocessing

For each selected MRI channel:

1. identify the non-zero brain region,
2. calculate brain-region mean and standard deviation,
3. apply z-score normalization,
4. clip to `[-5, 5]`,
5. map to `[0, 1]`,
6. keep non-brain voxels at zero.

Training/validation used prepared 2D axial-slice caches. The final held-out evaluation reconstructed predictions across each complete 3D test patient volume.

### Training

One logical training run was continued across four Kaggle notebooks:

- epochs 1–50
- epochs 51–100
- epochs 101–150
- epochs 151–200

Core configuration:

- architecture: PVTv2-B2 + EMCAD
- binary output: 1 channel
- image size: 352 × 352
- batch size: 8
- initial LR: 5e-4
- weight decay: 1e-4
- gradient clipping: 0.5
- multi-scale rates: 0.75, 1.0, 1.25
- seed: 2026
- total epochs: 200

Best validation checkpoint:

```text
Epoch: 139
Validation Dice: 0.8977
```

The remaining epochs did not exceed the epoch-139 validation Dice.

### Final held-out test

The single final test evaluation used the best validation-selected checkpoint.

| Metric | Result |
|---|---:|
| Patient-macro Dice | 0.8613 ± 0.1079 |
| Patient-macro IoU | 0.7700 ± 0.1469 |
| Global voxel Dice | 0.8984 |
| Global voxel IoU | 0.8155 |
| HD95 | 16.862 ± 21.851 |
| Evaluated patients | 48 / 48 |
| Empty predictions | 0 |

The final visualization notebook independently regenerated predictions from the saved epoch-139 checkpoint and reproduced the completed Part 5 metrics to approximately `1e-5`, providing an additional consistency check.

---

## 5. Evidence and integrity files

### Environment / source evidence

```text
evidence/environment/
├── ClinicDB_author_split_manifest.csv
├── EMCAD_AUTHOR_COMMIT.txt
├── EMCAD_ENVIRONMENT.txt
└── PVT_V2_B2_SHA256.txt
```

### Checksum manifests

```text
evidence/checksums/
├── EMCAD_NOTEBOOK1_SHA256SUMS.txt
├── EMCAD_ClinicDB_5Run_Final_SHA256SUMS.txt
├── EMCAD_BraTS_Part1_SHA256SUMS.txt
├── EMCAD_BraTS_Run1_Part2_SHA256SUMS.txt
├── EMCAD_BraTS_Run1_Part3_SHA256SUMS.txt
├── EMCAD_BraTS_Run1_Part4_SHA256SUMS.txt
└── EMCAD_BraTS_Run1_FINAL_v2_SHA256SUMS.txt
```

### Execution logs

```text
evidence/logs/
├── clinicdb_testing_aggregation.log
├── emcad-clinicdb-reference-replication.log
├── EMCAD_BraTS_Part1_stdout.log
├── EMCAD_BraTS_Run1_Part2_stdout.log
├── EMCAD_BraTS_Run1_Part3_stdout.log
├── EMCAD_BraTS_Run1_Part4_stdout.log
├── EMCAD_BraTS_Run1_Part5_FINAL_v2_stdout.log
└── EMCAD_Visualization_stdout.log
```

---

## 6. Terminology

Experiment 2 retains the EMCAD architecture but retrains/adapts it on a different neuroimaging dataset.

Therefore, the most precise description is:

> **cross-domain adaptation / evaluation of generalization behavior**

rather than strict zero-shot domain generalization.

ClinicDB and BraTS also use different modalities, targets, and evaluation units. Their Dice values should therefore not be directly subtracted and interpreted as a single "generalization gap."
