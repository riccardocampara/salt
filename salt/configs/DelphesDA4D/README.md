# Delphes DA / DA4D GN2 b-tagging training

This directory contains the Salt configurations used for the paired GN2-style
binary b-versus-light training comparison between:

- z-only deterministic-annealing vertexing (**DA z-only + IP significance**)
- four-dimensional deterministic-annealing vertexing
  (**DA4D reconstruction + IP significance**)

These are the exact two configurations associated with the comparison ROC figure
`roc_da_vs_da4d_ip.png`.

## Configuration files

| File | Reconstruction and training variant | Associated Salt run |
|---|---|---|
| `gn2_da_z_only_ip_full.yaml` | DA z-only with impact-parameter significance | `salt_runs/da_z_only_ip_full/salt_20260917-T090627` |
| `gn2_da4d_ip_reco_full.yaml` | DA4D reconstructed-vertex inputs with impact-parameter significance | `salt_runs/da4d_ip_reco_full/salt_20260917-T091936` |

The evaluated test outputs used to create the ROC comparison were:

```text
salt_runs/da_z_only_ip_full/salt_20260917-T090627/ckpts/
epoch=012-val_loss=0.15914__test_pp_output_test.h5

salt_runs/da4d_ip_reco_full/salt_20260917-T091936/ckpts/
epoch=011-val_loss=0.16046__test_pp_output_test.h5
```

The `full` suffix is part of the historical configuration and run name. These
are the configurations actually used for the ROC comparison.

## Comparison definition

The DA z-only and DA4D trainings use:

- the same binary b-versus-light flavour definition
- the same event-level train/validation/test split
- the same selected test jets
- the same Salt/GN2-style training framework
- impact-parameter information in both configurations

The comparison ROC uses:

```text
Signal:     flavour_label == 0  (b jets)
Background: flavour_label == 1  (light jets)
Score:      salt_pb
```

## Test-sample alignment

Before computing the ROC, the evaluation script verifies that DA z-only and
DA4D test outputs correspond jet-by-jet through:

```text
eventNumber
jet_index
flavour_label
```

The script stops if the two test outputs are not identically aligned. The ROC
therefore compares the same ordered test-jet sample rather than two independent
test samples.

The working points evaluated in the comparison are b-jet efficiencies of:

```text
0.60
0.70
0.77
0.85
```

## Input data

The Salt inputs are the matching UPP-produced train, validation, test,
normalisation, and class-definition files from the paired DA z-only and DA4D
preprocessing chains.

The companion `umami-preprocessing` repository contains the preprocessing
configurations and class mapping.

## Running locally

Run the configuration corresponding to the locally available dataset variant:

```bash
salt fit --config salt/configs/DelphesDA4D/gn2_da_z_only_ip_full.yaml
salt fit --config salt/configs/DelphesDA4D/gn2_da4d_ip_reco_full.yaml
```

For evaluation, use the checkpoint from the matching run and the matching UPP
test HDF5 file.

## Reproducibility and data policy

This directory stores configuration and documentation only. Do not commit:

- raw or preprocessed HDF5 datasets
- ROOT or Delphes input files
- Salt checkpoints (`.ckpt`)
- Salt test-output HDF5 files
- `salt_runs/`, `lightning_logs/`, or local result directories
- machine-specific paths or credentials

To reproduce the DA-versus-DA4D ROC faithfully, preserve the paired datasets,
flavour mapping, event split, model setup, and jet-by-jet test-sample alignment.
