# Large Artifacts

Large generated binaries are intentionally kept **outside ordinary Git history**.

The repository itself contains the executed notebooks, figures, small final result packages, logs, checksum manifests, and reproducibility documentation required to inspect the experiments.

## Experiment 1 — CVC-ClinicDB

| Artifact | Approx. size | Recorded SHA256 |
|---|---:|---|
| `emcadenv.tar.gz` | ~2.0 GB | `715ffde60f8743206302b76ede9b0b7a7f60df7b4dd662b8c924c6d03b1bcdba` |
| `EMCAD_prepared.tar.gz` | ~144 MB | `60f403d95a531e4767b1071c850d2d78d9fbb91f10cba7b5fae9f3858f95e290` |
| `EMCAD_run1_training_output.tar.gz` | ~190 MB | preserved separately |
| `EMCAD_run2_training_output.tar.gz` | ~190 MB | preserved separately |
| `EMCAD_run3_training_output.tar.gz` | ~190 MB | preserved separately |
| `EMCAD_run4_training_output.tar.gz` | ~190 MB | preserved separately |
| `EMCAD_run5_training_output.tar.gz` | ~190 MB | preserved separately |
| `EMCAD_ClinicDB_5Run_Best_Checkpoints.tar.gz` | ~474 MB | `458018fbdf23d6ab935088e3a2fea00a9c85752a0cf90da93a894cd3634c3d60` |

Small final Experiment 1 artifacts remain in `results/experiment_1_clinicdb/`.

Recorded hashes:

```text
4108d6aaa3ad35c38d17776057f80b0189fdc3fd0c427766e9d288a7da426a9c  EMCAD_ClinicDB_5Run_Final_Results.zip
87caa9ffb6874c62e9f66d028b5b52f7814b7fc3c559ccf159879bf841f82ffe  EMCAD_ClinicDB_5Run_Replication_Results.xlsx
```

## Experiment 2 — BraTS-derived neuroimaging adaptation

| Artifact | Approx. size | Recorded SHA256 |
|---|---:|---|
| `EMCAD_BraTS_Part1_Output.tar.gz` | ~629 MB | `786f72c03885cd4df3c6e70d718febe8c9f529a5f24aebb64b8c4dd4d360ea9f` |
| `EMCAD_BraTS_Run1_Part2_State.tar.gz` | ~380 MB | see `evidence/checksums/EMCAD_BraTS_Run1_Part2_SHA256SUMS.txt` |
| `EMCAD_BraTS_Run1_Part3_State.tar.gz` | ~381 MB | `67be2bb0bdab387aea0907a4d7d9683068bf0d445ba03b38f26a6c8347008fe5` |
| `EMCAD_BraTS_Run1_Part4_State.tar.gz` | ~382 MB | `0c4337ad4d283808ae143c516aaa27120a7b64d6f1d6737390436629c39ecb6b` |
| `EMCAD_BraTS_Run1_FINAL_Checkpoints_v2.tar.gz` | ~382 MB | `45d327425911f5f317feb2378a72889394cbde805b1bf9cb204d57e7c1a25a65` |

Small final Experiment 2 artifacts remain in `results/experiment_2_brats/`.

Recorded hashes:

```text
65dce78a871dce635d00834598f406a032d7cc060a5b0cb3c79aebae2bfc3d86  EMCAD_BraTS_Run1_FINAL_Evidence_v2.zip
15c866a2082d316ac500315d6b17183c2321e62f777048753e5493b2bc2aca54  EMCAD_BraTS_Run1_FINAL_Predictions_v2.tar.gz
```

## Pretrained encoder

The prepared PVTv2-B2 pretrained weight is kept outside normal Git history.

```text
80711cd1b37ffba12bec6c7a2a7c54efe2315ed635e6d2055fd51c3e909ede4d  pretrained_pth/pvt/pvt_v2_b2.pth
```

## Verification

Checksum manifests for the preserved artifacts are stored in:

```text
evidence/checksums/
```

## Storage

Recommended storage for the large binaries:

- local/external-drive backup,
- institutional/private cloud storage,
- selected GitHub Release assets,
- Git LFS only when direct versioning of a specific large binary is necessary.

Do not commit the multi-hundred-MB training states or the ~2 GB environment archive into ordinary Git history.
