# Delphes DA / DA4D GN2 b-tagging configurations

Salt training configurations for a paired comparison between:

- z-only deterministic-annealing vertexing (`DA`);
- four-dimensional deterministic-annealing vertexing (`DA4D`).

The input HDF5 files are produced with the matching UPP configurations in the
`umami-preprocessing` companion repository.

## Physics comparison

The DA and DA4D trainings use the same event-level train/validation/test split,
binary b-versus-light flavour labels, model architecture, loss setup and random
seed.

The intended difference is the reconstructed vertex-dependent information
provided to tracks by the DA or DA4D vertex finder.

## Configurations

- `GN2_DA_z_only_IP.yaml`: z-only DA baseline.
- `GN2_DA4D_IP.yaml`: DA4D baseline.

## Data policy

This repository contains configurations and documentation only.

Do not commit:

- HDF5 datasets;
- ROOT or Delphes input files;
- Salt checkpoints (`.ckpt`);
- `salt_runs/` and `lightning_logs/`;
- machine-specific absolute paths.

## Running locally

Set the local dataset paths in the YAML configuration, then run:

```bash
salt fit --config salt/configs/DelphesDA4D/GN2_DA4D_IP.yaml
```
