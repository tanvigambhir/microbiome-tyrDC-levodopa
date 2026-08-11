# Microbiome-TyrDC-Levodopa Analysis

## Overview
This project investigates how individual differences in gut microbiome 
composition affect response to Levodopa (L-DOPA), the primary treatment 
for Parkinson's disease. Specifically we are asking whether dietary and 
demographic factors predict the abundance of key microbial genes responsible 
for L-DOPA degradation in the gut — and whether these patterns vary across 
patient populations in ways that could explain heterogeneous treatment outcomes.

This work is motivated by a critical clinical problem: even when Parkinson's 
patients take L-DOPA alongside carbidopa (which is supposed to protect L-DOPA 
from peripheral degradation), up to 56% of the drug still fails to reach the 
brain. A key reason is that gut bacteria — particularly Enterococcus faecalis — 
degrade L-DOPA using a tyrosine decarboxylase enzyme (TyrDC) that carbidopa 
is 200 times less potent against than the human enzyme it was designed to block.

## Hypotheses
Based on existing literature we are testing three specific hypotheses:

**Hypothesis 1 — pH and PPI use:**
TyrDC expression in E. faecalis is pH-dependent and is higher at lower pH. 
Proton pump inhibitors (PPIs/antacids) raise gastric pH and may therefore 
reduce TyrDC activity. We hypothesize that patients on PPIs will show lower 
tyrDC gene abundance compared to patients not on PPIs.

**Hypothesis 2 — pH and fermented food consumption:**
Fermented foods lower gut pH through lactic acid production, which could 
enhance TyrDC activity. We hypothesize that patients with higher fermented 
food consumption (yogurt as proxy) will show higher tyrDC gene abundance.

**Hypothesis 3 — Protein intake and L-Tyrosine availability:**
TyrDC's preferred substrate is L-Tyrosine, a dietary amino acid abundant in 
protein-rich foods. Higher protein intake means more substrate available for 
TyrDC activity. We hypothesize that patients with higher protein intake will 
show higher tyrDC gene abundance and potentially worse L-DOPA treatment outcomes.

## Dataset
**Wallen et al. 2022** — *Metagenomics of Parkinson's disease implicates the 
gut microbiome in multiple disease mechanisms* (Nature Communications)

- 490 Parkinson's disease patients and 234 healthy controls
- Deep shotgun metagenomic sequencing
- Rich metadata including dietary frequency (protein intake, grain intake, 
  yogurt consumption), demographics (age, sex, race), medication history 
  (including antacid/indigestion drug use), and disease-related variables
- Publicly available at: https://zenodo.org/records/7246185
- Pre-processed MetaPhlAn4 and HUMAnN3 outputs available in source data

## Pipeline
1. **Quality filtering** — fastp to remove low quality reads and adapter sequences
2. **Species identification** — MetaPhlAn4 to identify bacterial species composition
3. **Functional gene quantification** — HUMAnN3 to quantify tyrDC gene abundance 
   (K18933) and nitroreductase gene abundance across all samples
4. **dadh SNP calling** — Bowtie2 to align reads against dadh gene reference, 
   bcftools to call variant at position 506 (Arg506 = active, Ser506 = inactive)
5. **Statistical analysis** — Stratify all measurements by dietary and demographic 
   metadata to test hypotheses

Note: The Wallen dataset pre-processed source data already contains MetaPhlAn4 
and HUMAnN3 outputs, allowing immediate exploratory analysis before full 
pipeline implementation on raw reads from NCBI SRA (BioProject PRJNA834801).

## Repository Structure 
 - data/ # metadata and processed outputs from Wallen dataset
- scripts/ # analysis scripts
- results/ # figures and summary statistics

## References
- Wallen et al. 2022, Nature Communications
- Maini Rekdal et al. 2019, Science
- Zimmermann et al. 2019, Science  
- Verdegaal et al. 2026, Nature Microbiology
- Jiang et al. 2024, Translational Psychiatry
