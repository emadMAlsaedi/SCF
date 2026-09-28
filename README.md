# SwarmConvFormer (SCF)

**Swarm-Initialized Convolution Transformer Hybrid with Adaptive Chaotic Swish (ACS) for SDN Attack Classification**

Emad M. Alsaedi¹\*, Rawia Abdulla Mohammed², Zeina Mohammed Saadi¹
¹ Department of Computer Sciences, University of Technology, Baghdad, Iraq
² The Iraqi Ministry of Education, Baghdad, Iraq

---

## Overview

SwarmConvFormer (SCF) is a lightweight hybrid Conv-Transformer architecture for intrusion detection in Software-Defined Networks (SDN), evaluated on the real **InSDN** benchmark. It targets three known failure modes of prior deep IDS architectures:

1. **Initialization wastage** in convolutional stems — addressed with a **Grey Wolf Optimizer (GWO)**-driven pre-training step that initializes the Conv1D stem against an information-theoretic fitness function (Shannon entropy + filter-bank orthogonality + response log-variance).
2. **Gradient crystallization** on minority classes — addressed with a novel **Adaptive Chaotic Swish (ACS)** activation, `f(x) = xσ(αx) + βsin(γx)`, with a learnable per-channel α and a bounded sinusoidal term that keeps the gradient from vanishing.
3. **Naive feature fusion** between local and global representations — addressed with an **asymmetric cross-attention fusion** module, where a ResNet-1D branch queries a parallel Transformer branch for global context.

## Results

All results below are from a fully re-executed, single consistent pipeline, evaluated across **5 independent seeds** ([42, 123, 7, 2024, 31415]) on the real InSDN dataset (343,889 flow records, 65 retained features, 8 classes: Normal, DoS, DDoS, Probe, BFA, Web-Attack, BOTNET, U2R — InSDN has no separate R2L category).

| Metric | Value |
|---|---|
| Accuracy | 0.959 ± 0.025 |
| Macro-Precision | 0.617 |
| Macro-Recall | 0.682 |
| **Macro-F1** | **0.570 ± 0.049** |
| MCC | 0.946 ± 0.032 |
| Parameters | 690,641 (685,329 trainable) |

**Baseline comparison** (same data, split, and evaluation protocol):

| Model | Params (M) | Macro-F1 | MCC |
|---|---|---|---|
| MLP-Baseline | 0.21 | **0.749** | 0.996 |
| 1D-CNN | 0.38 | 0.582 | 0.936 |
| Transformer-Only | 0.44 | 0.825 | 0.831 |
| CNN-Transformer | 0.55 | 0.896 | 0.905 |
| CT + Focal | 0.55 | 0.919 | 0.924 |
| **SwarmConvFormer (ours)** | 0.69 | 0.570 | **0.946** |

> **Honest note:** on this real data, simpler baselines currently achieve a higher macro-F1 than SwarmConvFormer, while SwarmConvFormer remains competitive on MCC. Performance on majority classes (DDoS, Normal, DoS, Probe) is strong (F1 0.925–0.999); performance on severely underrepresented classes (U2R: 3 test samples, BOTNET, BFA) is weak and highly seed-dependent. See the manuscript for full discussion.

## Repository Contents

- `SwarmConvFormer_FIXED.ipynb` — the corrected, end-to-end training and evaluation pipeline (5-seed protocol).
- `SwarmConvFormer_FULL_EXPERIMENTS.ipynb` — extended experiment suite (baseline models, ablation study, activation-function comparison, permutation importance, initialization-strategy comparison, latency/quantization benchmarking).
- `ACS_Analysis.ipynb` — standalone mathematical/quantitative analysis of the ACS activation function (nonlinearity, differentiability, boundedness, dead-zone comparison against ReLU/GELU/Swish/Mish).
- `results/` — raw per-seed JSON results (metrics, classification reports, confusion matrices, GWO convergence history) and summary CSVs for full reproducibility.
- Trained model weights per seed.

## Reproducing the Results

```bash
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute SwarmConvFormer_FIXED.ipynb
```

Each script/notebook prints or saves its outputs (JSON/CSV) so every number in the manuscript can be traced back to a specific completed run. Dataset: [InSDN (Elsayed et al., 2020)](https://github.com/malwaredll/InSDN) — not redistributed here; place the three source CSVs (`Normal_data.csv`, `metasploitable-2.csv`, `OVS.csv`) in `data/` before running.

## Citation

If you use this work, please cite:

```bibtex
@article{alsaedi2026swarmconvformer,
  title   = {Swarm Convolution Former (SCF): Swarm-Initialized Convolution Transformer Hybrid with Adaptive Chaotic Swish (ACS) for SDN Attack Classification},
  author  = {Alsaedi, Emad M. and Mohammed, Rawia Abdulla and Saadi, Zeina Mohammed},
  year    = {2026}
}
```

## License

[Add your chosen license here, e.g., MIT]

## Contact

Emad M. Alsaedi — Emad.M.Abbood@uotechnology.edu.iq
