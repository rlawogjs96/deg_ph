# KEGG analysis: ShinyGO 0.85 (https://bioinformatics.sdstate.edu/go/) [1]

**Condition:** WT *E. coli* K-12 MG1655, acidic pH stress (pH 5.5, HCl-acidified) vs matched pH 7 control (PRECISE-1K, project yTF 3).
**Species / annotation:** *E. coli* K-12 MG1655 (taxonomy ID 511145, EnsemblBacteria).
**DEG input:** 78 up-regulated and 19 down-regulated genes (|log2FC| ≥ 1, p < 0.05); background = all 4,257 analyzed genes.
**ShinyGO settings:** Pathway database KEGG, FDR cutoff 0.05, # pathways 100, pathway size 2–5000, remove redundancy, use pathway DB for gene counts, show pathway IDs.

> Note: the canonical acid-resistance genes induced in this comparison (*asr*, *gadA/B/C/E/W/X*, *hdeA/B/D*) are regulatory or chaperone genes that are not assigned to KEGG metabolic maps, so they do not appear as KEGG enrichment terms even though they are present in the up-regulated DEG list. KEGG enrichment therefore highlights the *metabolic and signaling* subset of the acid response (two-component systems, nitrogen/nitrate respiration), and these terms should be read as complementary to — not a replacement for — the classical acid-resistance genes seen directly in the DEG table.

---

## 1. Up-regulated DEG

### 1) Two-component system (`eco02020`, FDR = 1.2e-05, 18 genes)
Up-regulated DEGs were strongly enriched in two-component signal-transduction systems, indicating that acidic pH activates multiple environmental-sensing and regulatory modules rather than a single response [2], [3]. Notably, the list includes **safA** (the EvgS/EvgA–PhoQ/PhoP connector), **arnB** (L-Ara4N lipid A modification, PhoPQ/PmrAB-regulated), **ompC** (EnvZ/OmpR porin regulation), and the *app* cytochrome *bd*-II genes (**appY**, **appB**, **appC**), together pointing to coordinated envelope/membrane remodeling and respiratory adjustment under low pH [3], [4]. This is consistent with the interpretation that the acid response is organized at the level of regulatory networks and cell-surface adaptation.

### 2) Nitrogen metabolism and Nitrogen cycle (`eco00910`, FDR = 4.4e-06; `eco01310`, FDR = 2.0e-07)
These two terms are driven by the same nitrate/nitrite respiratory gene set — the nitrate reductase complexes **narG/narH/narI** and **narY/narZ**, the nitrate/nitrite transporter **narU**, and the NADH-dependent nitrite reductase **nirB/nirD** — so they are interpreted together as enrichment of **nitrate/nitrite respiratory metabolism** [5]. Up-regulation of these dissimilatory nitrate-reduction genes suggests a shift toward anaerobic/microaerobic respiratory metabolism accompanying acidic growth; *nirBD* additionally contributes to nitrite detoxification. Because these genes are primarily controlled by the FNR and NarXL regulators, this enrichment is read conservatively as respiratory/metabolic reorganization rather than a direct acid-resistance mechanism.

---

## 2. Down-regulated DEG

### 1) Arginine and proline metabolism (`eco00330`, FDR = 5.1e-04, 4 genes)
The only significantly enriched KEGG pathway among down-regulated DEGs was arginine and proline metabolism, driven entirely by the putrescine-utilization (*puu*) operon — **puuA**, **puuB**, **puuC**, **puuD** [6]. Down-regulation of this operon indicates reduced putrescine catabolism under acidic pH. Because polyamine (putrescine) metabolism is closely linked to acid tolerance, reduced putrescine *degradation* is consistent with preserving polyamine pools during acid stress, although this should be interpreted conservatively.

### 2) Maltose uptake regulon (STRING GO enrichment; not captured by KEGG)
KEGG returned only one significant down-regulated pathway, but the STRING-based GO process enrichment (`STRING_enrichmentProcess.csv`) shows strong, coherent repression of the **maltose uptake regulon** — *malE*, *malF*, *malK*, *lamB*, *malM* (Maltose transport, FDR ≈ 2e-05; Maltodextrin transmembrane transport, FDR ≈ 1.4e-05) [7]. This operon-level repression of maltose transport under acidic pH (in M9-glucose medium) is reported here because it is biologically coherent and matches the down-regulated DEG list, even though it did not pass the KEGG enrichment cutoff.

---

## Conclusion

KEGG enrichment analysis showed that up-regulated DEGs under acidic pH were significantly enriched in **two-component systems** and **nitrogen/nitrate respiratory metabolism**, indicating that the acid response is organized through environmental-sensing regulatory networks, cell-envelope/LPS remodeling (*safA*, *arnB*, *ompC*), and a shift toward nitrate/nitrite respiration. The classical acid-resistance effectors (*asr*, *gad* system, *hde* chaperones) were present in the up-regulated DEG list but, being regulatory/chaperone genes, were not captured as KEGG metabolic terms.

Down-regulated DEGs were enriched in **arginine and proline metabolism**, driven by repression of the putrescine-utilization (*puu*) operon, and — from the STRING GO analysis — in **maltose uptake** (*malE/F/K*, *lamB*, *malM*). Together these indicate reduced putrescine catabolism and operon-level repression of maltose transport under acidic pH.

Overall, the KEGG results suggest that acidic pH adaptation in *E. coli* MG1655 involves regulatory and respiratory remodeling (two-component sensing, nitrate respiration, envelope modification) alongside repression of selected catabolic/transport functions (putrescine utilization, maltose uptake), rather than a simple induction of canonical acid-shock genes alone.

> Caveat: with n = 2 replicates per condition, these results are interpreted as a condition-specific expression-shift screening, not a high-powered DEG analysis.

---

## Supporting files (this folder)

- `barplot_up.png`, `barplot_down.png` — KEGG enrichment charts
- `enrichment_up.csv`, `enrichment_down.csv` — enriched-pathway tables
- `geneInfo_up.csv` (geneInfo_down.csv: to be added from the Genes tab)
- KEGG maps (up): `Two-component system_up.png`, `Nitrogen metabolism_up.png`, `Nitrogen cycle_up.png`
- KEGG map (down): `Arginine and proline metabolism_down.png`
- `STRING_enrichmentProcess.csv` — STRING GO process enrichment (source of the maltose-regulon result)

---

## References

> DOIs below were verified against PubMed / publisher records.

[1] S. X. Ge, D. Jung, R. Yao, "ShinyGO: a graphical gene-set enrichment tool for animals and plants," *Bioinformatics*, vol. 36, no. 8, pp. 2628–2629, 2020, doi: 10.1093/bioinformatics/btz931.

[2] K. S. Choudhary et al., "Elucidation of Regulatory Modes for Five Two-Component Systems in *Escherichia coli* Reveals Novel Relationships," *mSystems*, vol. 5, no. 6, pp. e00980-20, 2020, doi: 10.1128/mSystems.00980-20.

[3] J. W. Foster, "*Escherichia coli* acid resistance: tales of an amateur acidophile," *Nat. Rev. Microbiol.*, vol. 2, no. 11, pp. 898–907, 2004, doi: 10.1038/nrmicro1021.

[4] Y. Eguchi, J. Itou, M. Yamane, R. Demizu, R. Utsumi, et al., "B1500, a small membrane protein, connects the two-component systems EvgS/EvgA and PhoQ/PhoP in *Escherichia coli*," *Proc. Natl. Acad. Sci. USA*, vol. 104, no. 47, pp. 18712–18717, 2007, doi: 10.1073/pnas.0705768104.

[5] G. Unden, J. Bongaerts, "Alternative respiratory pathways of *Escherichia coli*: energetics and transcriptional regulation in response to electron acceptors," *Biochim. Biophys. Acta*, vol. 1320, no. 3, pp. 217–234, 1997, doi: 10.1016/S0005-2728(97)00034-0.

[6] S. Kurihara, S. Oda, K. Kato, H. G. Kim, T. Koyanagi, H. Kumagai, H. Suzuki, "A novel putrescine utilization pathway involves γ-glutamylated intermediates of *Escherichia coli* K-12," *J. Biol. Chem.*, vol. 280, no. 6, pp. 4602–4608, 2005, doi: 10.1074/jbc.M411114200.

[7] W. Boos, H. Shuman, "Maltose/maltodextrin system of *Escherichia coli*: transport, metabolism, and regulation," *Microbiol. Mol. Biol. Rev.*, vol. 62, no. 1, pp. 204–229, 1998, doi: 10.1128/MMBR.62.1.204-229.1998.
