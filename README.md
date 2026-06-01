# DEG analysis: acidic pH stress in *E. coli* MG1655

Differential gene expression (DEG) analysis comparing wild-type *Escherichia coli* K-12 MG1655 under **acidic pH stress (pH 5.5, HCl-acidified)** versus a **matched pH 7 control**, using the [PRECISE-1K](https://github.com/SBRG/precise1k) RNA-seq compendium from [iModulonDB](https://imodulondb.org).

> **Main finding:** Acidic pH stress induces a strong, induction-biased transcriptional response in *E. coli* MG1655 (78 up vs 19 down DEGs). Top up-regulated genes (***asr***, ***ydeO***) are consistent with the canonical acid-stress response, while four of the top five down-regulated genes (***malE***, ***malK***, ***lamB***, ***malM***) reveal coherent repression of the maltose uptake regulon.

---

## Comparison setup

| group | condition | sample IDs | strain | temperature | media / carbon |
|---|---|---|---|---|---|
| control | WT pH 7 | `p1k_00809`, `p1k_00810` | MG1655 | 37 °C | M9 / glucose |
| treatment | WT pH 5.5 (HCl) | `p1k_00815`, `p1k_00816` | MG1655 | 37 °C | M9 / glucose |

All four samples come from the same **yTF 3** project of PRECISE-1K, minimizing strain, media, and batch effects. Each condition has n = 2 biological replicates.

---

## Methods

1. **Input** — PRECISE-1K log-TPM expression matrix (4,257 genes × 1,035 samples, mean-centered on log2 scale).
2. **Sample selection** — extract the four columns above.
3. **Per-gene statistics**
   - `log2FC = mean(treatment) − mean(control)` (the matrix is already on log2 scale, so subtraction is correct)
   - `p-value` from Welch's *t*-test (`scipy.stats.ttest_ind(..., equal_var=False)`)
4. **DEG criteria** — `|log2FC| ≥ 1` AND `p-value < 0.05` → classified as `up`, `down`, or `ns`.
5. **Annotation** — b-numbers mapped to gene names and gene products via the official PRECISE-1K `gene_info.csv` (based on NCBI RefSeq NC_000913.3).

> **Caveat:** With n = 2 replicates per group, the Welch *t*-test has limited statistical power. Results are interpreted as a **condition-specific expression shift screening**, not a definitive high-powered DEG analysis.

---

## Results summary

| direction | b-number | gene name | gene product | log2FC | p-value |
|---|---|---|---|---:|---:|
| up | b1597 | ***asr*** | acid shock protein | +6.48 | 5.76e-03 |
| up | b2375 | ***yfdX*** | protein YfdX | +6.27 | 3.66e-03 |
| up | b2374 | ***frc*** | formyl-CoA transferase | +6.08 | 1.07e-02 |
| up | b2085 | ***yegR*** | uncharacterized protein YegR | +5.91 | 1.72e-04 |
| up | b1499 | ***ydeO*** | transcriptional dual regulator | +5.71 | 1.26e-04 |
| down | b4035 | ***malK*** | maltose ABC transporter ATP-binding subunit | −1.80 | 4.28e-02 |
| down | b4036 | ***lamB*** | maltose outer membrane channel / λ receptor | −1.65 | 2.53e-02 |
| down | b4034 | ***malE*** | maltose ABC transporter periplasmic binding protein | −1.61 | 3.93e-02 |
| down | b4037 | ***malM*** | maltose regulon periplasmic protein | −1.54 | 5.62e-03 |
| down | b1113 | ***ldtC*** | L,D-transpeptidase YcfS | −1.53 | 2.81e-02 |

### Interpretation guardrails

- ***asr*** and ***ydeO*** provide clear support for acid-stress response activation.
- ***yfdX*** lies in an acid-resistance-associated genomic neighborhood; not claimed as an established acid-resistance factor.
- ***frc*** interpreted conservatively as oxalate/formate-associated metabolism, not a direct acid-resistance gene.
- ***yegR*** is uncharacterized; no functional claim is made.
- Coherent repression of ***malE/K/M*** and ***lamB*** (4 of top 5 down hits) suggests **operon-level repression of the maltose uptake regulon** under acidic pH, even though both conditions used M9 + glucose (not maltose) as the carbon source.

---

## Repository structure

```
deg_ph_stress/
├── README.md                                           This file
├── analysis.ipynb                                      Reproducible Jupyter notebook
├── data/
│   └── e_coli_precise1k_expression.csv                 PRECISE-1K log-TPM matrix
├── metadata/
│   ├── e_coli_precise1k_samples.csv                    Sample metadata (1,035 samples)
│   └── gene_info.csv                                   b-number → gene name / product
├── results/
│   ├── DEG_WT_pH55_HCl_vs_WT_pH7.csv                   Full DEG table (4,257 genes)
│   ├── top5_significant_up_down_WT_pH55_HCl_vs_WT_pH7.csv
│   ├── top5_up_down_WT_pH55_HCl_vs_WT_pH7.csv          (legacy, kept for history)
│   ├── volcano_WT_pH55_HCl_vs_WT_pH7.png               Volcano plot (600 dpi)
│   ├── comparison_setup_WT_pH55_HCl_vs_WT_pH7.png      Slide-ready setup table
│   └── top5_table_WT_pH55_HCl_vs_WT_pH7.png            Slide-ready top-5 table
└── a.txt                                               Original planning notes
```

---

## Reproducing the analysis

Python 3.10+ with the following packages:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install pandas scipy matplotlib jupyter
```

Run the notebook end-to-end:

```bash
jupyter nbconvert --to notebook --execute --inplace analysis.ipynb
```

Or open `analysis.ipynb` in JupyterLab / VS Code / Cursor and run all cells. All result files in `results/` will be regenerated.

---

## Data sources & citations

- **PRECISE-1K** (expression matrix, sample metadata, gene annotations) — SBRG, UCSD
  Lamoureux *et al.*, *Nucleic Acids Research* (2023). [`SBRG/precise1k`](https://github.com/SBRG/precise1k)
- **iModulonDB** — [`imodulondb.org`](https://imodulondb.org)
- **Reference genome** — NCBI RefSeq [NC_000913.3](https://www.ncbi.nlm.nih.gov/nuccore/NC_000913.3) (*E. coli* K-12 MG1655)
