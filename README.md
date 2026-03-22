# Gossypium ECTO

Entropy-Coupled Trait ODE (ECTO) applied to *Gossypium hirsutum* pangenome breeding trait dynamics.

This repository contains all code to reproduce the results in:

> Rodriguez, A.M. (2026). Domain-invariant transfer of an entropy-coupled ODE framework from psychometric to pangenomic breeding trait dynamics in *Gossypium hirsutum*. 

The ECTO framework was published in:

> Rodriguez, A.M. (2026). An entropy-initiated coupled-trait ODE framework for modeling longitudinal cohort dynamics. *PLoS ONE*. [doi:10.1371/journal.pone.0344090](https://doi.org/10.1371/journal.pone.0344090)

## Overview

ECTO is a minimal system of coupled nonlinear ODEs initialized from Shannon entropy indices. This work applies it (with zero structural modification from its original use in longitudinal psychometric data analysis of a cohort) to structural variant (SV) genotype frequency data from a cotton pangenome study, demonstrating domain-invariant transfer from psychometric to genomic dynamics.

The pipeline:
1. **Entropy preprocessing**: Compute Shannon entropy from binary SV genotype frequency distributions [p(Ref), p(Alt)] across breeding-era cohorts, pooled across trait-linked loci
2. **ODE fitting**: Fit the coupled ECTO system to the resulting entropy trajectories via differential evolution
3. **Validation**: Multistart convergence, baseline comparisons, leave-one-out forecasting, temporal sensitivity analysis

## Data

This analysis requires Supplementary Table S49 from:

> Zhang, Y., Sun, Z., Tian, S. et al. (2026). A pangenome reference and population studies link structural variants with breeding traits in *Gossypium hirsutum*. *Nature Genetics*. [doi:10.1038/s41588-026-02523-z](https://doi.org/10.1038/s41588-026-02523-z)

**To obtain the data:**

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
Repo_ECTO_Gossypium/
├── README.md
├── run_all.py                          # Master replication script (run this)
└── ablation/
    ├── gossypium_ecto_temporal_ablation.py  # Temporal sensitivity analysis (48 date combos)
    └── tefi_trajectories.json               # Entropy input for ablation
```

`run_all.py` executes the full pipeline: entropy preprocessing, ODE fitting (all trait pairings, both c3 configurations), leave-one-out validation, and multistart convergence analysis. Results are written to `results/`.

The `ablation/` directory contains the temporal sensitivity analysis, which sweeps 48 combinations of cohort pseudo-time assignments to confirm that the results are qualitatively stable regardless of date choice.

## Requirements

- Python 3.8+
- numpy
- scipy
- pandas
- openpyxl

## Key Results

- **6 traits** (FL, FS, FM, BW, LP, SI) × **3 breeding cohorts** (founding, intermediate, modern) → Shannon entropy trajectories
- **6 trait pairings** fitted with ECTO under frozen (c3=0) and free c3 configurations
- FS–FM achieves R² = 1.000 on both channels with c3=0 (perfect autonomous fit)
- FL–FS achieves R² = 0.843 (N) and 0.939 (P) with c3=0; P improves to 0.9999 with c3 free
- Temporal sensitivity analysis across 48 date combinations confirms qualitative stability (mean R² = 0.81 N, 0.92 P)
- All configurations outperform flat and linear baselines

## Citation

If you use this code, please cite:

```bibtex
@article{rodriguez2026gossypium,
  author  = {Rodriguez, Anderson M.},
  title   = {Domain-invariant transfer of an entropy-coupled {ODE} framework
             from psychometric to pangenomic breeding trait dynamics in
             \textit{Gossypium hirsutum}},
  journal = {BioSystems},
  year    = {2026},
  note    = {Submitted}
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

Anderson M. Rodriguez
University of Georgia
amr28693@uga.edu
ORCID: [0009-0007-5179-9341](https://orcid.org/0009-0007-5179-9341)

## License

MIT
