# Gossypium ECTO

Entropy-Initiated Coupled-Trait ODE (ECTO) applied to *Gossypium hirsutum* pangenome breeding trait dynamics.

This repository contains all code to reproduce the results in:

> Rodriguez, A.M. (2026). Cross-domain transfer without structural modification: Applying entropy-initiated coupled-trait ODEs to pangenomic breeding trait dynamics in *Gossypium hirsutum*.

The ECTO framework was published in:

> Rodriguez, A.M. (2026). An entropy-initiated coupled-trait ODE framework for modeling longitudinal cohort dynamics. *PLoS ONE*. doi:[10.1371/journal.pone.0344090](https://doi.org/10.1371/journal.pone.0344090)

## Overview

ECTO is a minimal system of coupled nonlinear ODEs initialized from Shannon entropy indices. This work applies it (with zero structural modification from its original use in longitudinal psychometric data analysis of a cohort) to structural variant (SV) genotype frequency data from a cotton pangenome study, demonstrating cross-domain transfer from psychometric to genomic dynamics.

The pipeline:

1. **Entropy preprocessing**: Compute Shannon entropy from binary SV genotype frequency distributions [p(Ref), p(Alt)] across breeding-era cohorts, pooled across trait-linked loci
2. **ODE fitting**: Fit the coupled ECTO system to the resulting entropy trajectories via differential evolution
3. **Validation**: Multistart convergence, baseline comparisons, leave-one-out forecasting, temporal sensitivity analysis

## Data

This analysis requires Supplementary Table S49 from:

> Zhang, Y., Sun, Z., Tian, S. et al. (2026). A pangenome reference and population studies link structural variants with breeding traits in *Gossypium hirsutum*. *Nature Genetics*. doi:[10.1038/s41588-026-02523-z](https://doi.org/10.1038/s41588-026-02523-z)

To obtain the data:

1. Go to https://www.nature.com/articles/s41588-026-02523-z#Sec34
2. Download `41588_2026_2523_MOESM3_ESM.xlsx` from the Supplementary Information section
3. Place it in the repository root directory

This file is copyrighted by Springer Nature and cannot be redistributed.

## Quick Start

```bash
pip install numpy scipy pandas openpyxl
python run_all.py
```

Place `41588_2026_2523_MOESM3_ESM.xlsx` in the same directory as `run_all.py`. Expected runtime: ~3–5 minutes. All results are written to a `results/` directory.

## Repository Structure

```
gossypium_ecto/
├── README.md
├── run_all.py                                    # Master replication script (run this)
├── ablation/
│   ├── gossypium_ecto_temporal_ablation.py       # Temporal sensitivity analysis (48 date combos)
│   ├── gossypium_ecto_temporal_ablation.json     # Full ablation results
│   ├── trait_h_trajectories.json                 # Entropy input (see note below)
│   └── note_on_trait_h_trajectories_input.txt
├── meta_validation/
│   ├── meta_validation_1.py                      # Structural constraint quantification (50k random draws + hat matrix)
│   ├── meta_validation_1.json                    # Full results
│   └── trait_h_trajectories.json                 # Entropy input (see note below)
└── results/
    ├── all_pairs.json                            # All trait pairings, both c3 configurations
    ├── convergence_baselines.json                # Multistart convergence statistics
    ├── loo_validation.json                       # Leave-one-out forecasting results
    ├── primary_fits_FL_FS.json                   # Primary FL–FS fit details
    └── trait_h_trajectories.json                 # Entropy input (see note below)
```

**Note on `trait_h_trajectories.json`:** This file contains the 18 pooled Shannon entropy values (6 traits × 3 cohorts) that serve as input to all analyses. It appears in three directories so that each module (`run_all.py`, `ablation/`, `meta_validation/`) can be run independently without cross-directory dependencies. All three copies are identical.

`run_all.py` executes the full pipeline: entropy preprocessing, ODE fitting (all trait pairings, both c3 configurations), leave-one-out validation, and multistart convergence analysis. Results are written to `results/`.

The `ablation/` directory contains the temporal sensitivity analysis, which sweeps 48 combinations of cohort pseudo-time assignments to confirm that the results are qualitatively stable regardless of date choice.

The `meta_validation/` directory contains the structural constraint quantification analysis. It samples 50,000 random parameter vectors per trait pairing and reports the fraction achieving joint R² thresholds, demonstrating that the ODE's continuous-trajectory constraint restricts the achievable fit region to <0.004% of the parameter volume. A hat matrix analysis confirms effective degrees of freedom of ~2 for the best-conditioned pairings.

## Requirements

- Python 3.8+
- numpy
- scipy
- pandas
- openpyxl

## Key Results

- 6 traits (FL, FS, FM, BW, LP, SI) × 3 breeding cohorts (founding, intermediate, modern) → Shannon entropy trajectories
- 6 trait pairings fitted with ECTO under frozen (c3=0) and free c3 configurations
- FS–FM achieves R² = 1.000 on both channels with c3=0 (perfect autonomous fit)
- FL–FS achieves R² = 0.843 (N) and 0.939 (P) with c3=0; P improves to 0.9999 with c3 free
- Temporal sensitivity analysis across 48 date combinations confirms qualitative stability (mean R² = 0.81 N, 0.92 P)
- All configurations outperform flat and linear baselines

## Citation

If you use this code, please cite:

```bibtex
@article{rodriguez2026gossypium,
  author  = {Rodriguez, Anderson M.},
  title   = {Cross-domain transfer without structural modification: Applying
             entropy-initiated coupled-trait {ODE}s to pangenomic breeding
             trait dynamics in \textit{Gossypium hirsutum}},
  journal = {preprint},
  year    = {2026},
  note    = {Submitted for peer review}
}

@article{rodriguez2026ecto,
  author  = {Rodriguez, Anderson M.},
  title   = {An entropy-initiated coupled-trait {ODE} framework for modeling
             longitudinal cohort dynamics},
  journal = {PLoS ONE},
  year    = {2026},
  doi     = {10.1371/journal.pone.0344090}
}

@article{zhang2026gossypium,
  author  = {Zhang, Yan and Sun, Zhengwen and Tian, Shilin and Wu, Liqiang
             and Gu, Qishen and Ke, Huifeng and Zhang, Guiyin and others},
  title   = {A pangenome reference and population studies link structural
             variants with breeding traits in \textit{Gossypium hirsutum}},
  journal = {Nature Genetics},
  year    = {2026},
  doi     = {10.1038/s41588-026-02523-z}
}
```

## Author

Anderson M. Rodriguez · University of Georgia · amr28693@uga.edu · ORCID: [0009-0007-5179-9341](https://orcid.org/0009-0007-5179-9341)

## License

MIT
