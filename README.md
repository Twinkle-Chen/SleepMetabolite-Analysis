# SleepMetabolite-Analysis
This project examines how genetically predicted plasma metabolite levels relate to ten sleep traits and then uses network, enrichment, pathway, and colocalization analyses to organize and interpret the resulting signals. The primary screening used Mendelian randomization (MR) across 1,400 metabolites and ten sleep traits. Network analysis, enrichment and pathway analysis, and colocalization were subsequently focused on 1,091 plasma metabolites from this panel. The analytical sequence is:

  1. MR
  2. RepeatMR
  3. NetworkAnalysis
  4. EnrichmentPathwayAnalysis
  5. Colocalization

Study design in brief
---------------------
The first stage used bidirectional MR to determine whether genetically predicted metabolite abundance is associated with sleep traits and whether genetically predicted sleep traits are associated with metabolite abundance. Genotypes were used as instrumental variables. The MR design relies on three standard conditions: the instruments must predict the exposure; they must be unrelated to measured or unmeasured confounders; and they must affect the outcome only through the exposure. These conditions reduce, but do not eliminate, confounding in observational comparisons.

Network analysis then represented significant metabolite-sleep relationships as connections among sleep traits through shared metabolites. This step was conducted through the relevant web platform, so this repository contains no local NetworkAnalysis code.

The enrichment and pathway assessment was used to contextualize the MR findings by asking which metabolic categories and pathways were over-represented among the significant results. This work was also performed through the corresponding web platform and is therefore represented by results rather than local scripts. The biological interpretation emphasizes pathways that connect multiple sleep traits and those with the strongest statistical support, which may point toward disease mechanisms or intervention targets.

Colocalization was used to determine whether a metabolite signal and a sleep-trait signal in the same genomic region are more consistent with one shared causal variant than with two distinct causal variants. The nearest genes to implicated loci and their associated phenotypes were also summarized to strengthen gene-level interpretation.

Folder guide
------------
MR/
  Bidirectional two-sample MR from metabolites to sleep traits and from sleep traits to metabolites. Includes clumping, harmonization, IVW/MR-Egger/weighted-median/MR-RAPS estimates, pathway annotation, multiple-testing correction, and lipid sub-pathway Fisher tests.

RepeatMR/
  Sensitivity analysis that removes specified potentially pleiotropic instruments and repeats the MR estimates. It also includes a summary of PhenoScanner/PheWAS associations for the instruments.

NetworkAnalysis/
  Holds outputs from the network platform analysis that maps relationships among sleep traits through significant metabolites. No local executable code was required for this website-based step.

EnrichmentPathwayAnalysis/
  Holds outputs from the website-based enrichment and pathway analyses used to identify biological processes and metabolic pathways enriched among the significant metabolite-sleep findings. No local executable code is included.

Colocalization/
  Contains abf and SuSiE colocalization, nearest-gene annotation, LD-based Jaccard clustering of metabolite instruments, and multi-trait HyPrColoc. These analyses assess shared causal variation and organize connected metabolites into clusters for multi-trait testing.

Files supplied in this package
------------------------------
- MR/MR_analysis.Rmd and MR/README.txt.
- RepeatMR/RepeatMR_analysis.Rmd and RepeatMR/README.txt.
- Colocalization/Colocalization_analysis.Rmd and Colocalization/README.txt.

Each consolidated R Markdown file preserves the order of the original numbered scripts. The original source filename is written as an R comment inside every imported code chunk and is also included in the chunk label. Chinese comments were translated into English. Fixed run dates were removed from document headers and generated output names. Dates that remain inside input filenames are identifiers of downloaded source files.

Common path convention
----------------------
Each R Markdown document defines project_root once in its setup chunk:

  project_root <- normalizePath("Path", winslash = "/", mustWork = FALSE)

Replace "Path" with the local project root. Set it to an absolute path to avoid ambiguity when the working directory changes.

Path/
  Data/
    Metabolite_GWAS/        GCST*.tsv.gz/metabolite summary statistics
    Sleep_GWAS/             Sleep-trait summary statistics in RData or GWAS Catalog format
    Reference/              GWAS Catalog, sleep metadata, pathway annotations, HMDB mappings,
                            significant MR tables, IV lists, and SNP annotation tables
    LD_Reference/           EUR.bed, EUR.bim, EUR.fam
    PhenoScanner/           PhenoScanner exports
  MetaboliteSleep_Code/
    MR/                     MR_analysis.Rmd, README.txt, outputs/
    RepeatMR/               RepeatMR_analysis.Rmd, README.txt, outputs/
    Colocalization/         Colocalization_analysis.Rmd, README.txt, outputs/
    NetworkAnalysis/        Website-derived network results (optional in local execution)
    EnrichmentPathwayAnalysis/ Website-derived enrichment/pathway results (optional in local execution)

Recommended execution order
---------------------------
1. MR/MR_analysis.Rmd: produce harmonized data, directional MR results, and corrected significant tables.
2. RepeatMR/RepeatMR_analysis.Rmd: run the instrument-removal sensitivity analysis.
3. Import the significant results into the network and enrichment/pathway platforms and save their outputs under NetworkAnalysis/ and EnrichmentPathwayAnalysis/.
4. Colocalization/Colocalization_analysis.Rmd: run abf/SuSiE colocalization, LD-overlap clustering, and HyPrColoc.

Software and data expectations
------------------------------
R 4.x is recommended. Required R packages are loaded in the setup chunks and include TwoSampleMR, ieugwasr, plinkbinr, mr.raps, coloc, susieR, vautils, pheatmap, hyprcoloc, openxlsx, readxl, data.table, dplyr, stringr, ggplot2, and cluster. PLINK must be operable through plinkbinr or available on PATH. The local LD panel should match the ancestry of the included GWAS samples.

Summary-statistics, harmonized RData objects, LD reference binaries, and website-generated network/enrichment outputs are not embedded in the R Markdown files. Reviewers should place them in the documented directory structure and change project_root only once per R Markdown document.

Key interpretation rules
------------------------
- MR evidence is probabilistic and depends on the validity of the three instrumental-variable assumptions.
- PP.H4 in colocalization supports a shared causal variant; PP.H3 supports distinct causal variants in the same region.
- A SuSiE result labelled no_credible_set means that the analysis cannot be evaluated in that window, not that colocalization has been disproved.
- The LD-overlap Jaccard index summarizes instrument similarity and should be interpreted as a data-driven grouping aid, not as causal evidence.
