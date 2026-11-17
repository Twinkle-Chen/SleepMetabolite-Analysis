COLOCALIZATION AND MULTI-TRAIT OVERLAP ANALYSIS
===============================================

Folder purpose
--------------
This folder contains one consolidated R Markdown document covering five sequential analyses: abf colocalization, nearest-gene annotation, SuSiE colocalization with a local LD panel, LD-based instrument-overlap clustering, and multi-trait HyPrColoc.

Main document
-------------
Colocalization_analysis.Rmd

How to run
----------
1. Run MR as needed to create MR/outputs/metabolite_sleep_primary_fdrmethod0.05.xlsx and MR/outputs/harmonise_data.
2. Open Colocalization_analysis.Rmd and replace project_root <- normalizePath("Path", ...) in the setup chunk.
3. Run the chunks in order. Results are written to Colocalization/outputs.
4. For HyPrColoc, run the LD-overlap clustering section before its final section so that LDOverlap/LDoverlap_results.xlsx is available.

Expected data layout (names can be changed consistently in the setup chunk):

Path/
  Data/
    Metabolite_GWAS/        GCST*.tsv.gz files (GWAS Catalog format)
    Sleep_GWAS/             sleep-trait RData files, grouped by GWAS accession where used
    Reference/              GWAS Catalog export, sleep-trait table, pathway/HMDB tables,
                            significant MR tables, IV lists, and annotation inputs
    LD_Reference/           EUR.bed, EUR.bim, EUR.fam (1000 Genomes phase 3 EUR)
    PhenoScanner/           trait_variants_annotated_1.txt ... _5.txt
  MetaboliteSleep_Code/
    MR/ RepeatMR/ Colocalization/


Run order and code-block purposes
---------------------------------
  coloc-00-setup
    Defines the one-time project root, all relative input locations, package dependencies, and the Colocalization output directory.
  coloc-01-01-select-colocalization-metabolites
    Selects metabolite-level MR findings for colocalization.
  coloc-01-02-configure-paths
    Defines shared paths for metabolite GWAS, sleep GWAS, harmonized MR data, and outputs.
  coloc-01-03-catalog-sample-size
    Extracts sample sizes for metabolite GWAS from the GWAS Catalog export.
  coloc-01-04-long-sleep-duration
    Runs ±500 kb coloc.abf for long sleep duration.
  coloc-01-05-short-sleep-duration
    Runs ±500 kb coloc.abf for short sleep duration.
  coloc-01-06-insomnia
    Runs ±500 kb coloc.abf for insomnia.
  coloc-01-07-morning-person
    Runs ±500 kb coloc.abf for morning person.
  coloc-01-08-snoring-adj-bmi
    Runs ±500 kb coloc.abf for snoring adjusted for BMI.
  coloc-01-09-osa
    Runs ±500 kb coloc.abf for obstructive sleep apnoea.
  coloc-01-10-daytime-sleepiness
    Runs ±500 kb coloc.abf for daytime sleepiness adjusted for BMI.
  coloc-01-11-chronotype
    Runs ±500 kb coloc.abf for chronotype.
  coloc-01-12-sleep-duration
    Runs ±500 kb coloc.abf for sleep duration.
  coloc-01-13-napping
    Runs ±500 kb coloc.abf for daytime napping.
  coloc-02-01-nearest-genes
    Finds genes within 100 kb of significant SNPs and selects the nearest gene.
  coloc-03-01-select-colocalization-metabolites
    Selects metabolite-level MR findings for SuSiE colocalization.
  coloc-03-02-configure-paths
    Defines genomic windows, local LD-panel parameters, and output locations.
  coloc-03-03-catalog-sample-size
    Extracts metabolite sample sizes from GWAS Catalog.
  coloc-03-04-ld-panel-functions
    Defines functions that extract local LD matrices, align alleles, and estimate effective sample size.
  coloc-03-05-locus-coloc-susie-functions
    Defines locus-level SuSiE and coloc.susie functions, result templates, and per-trait orchestration.
  coloc-03-06-long-sleep-duration
    Runs locus-level SuSiE colocalization for long sleep duration.
  coloc-03-07-short-sleep-duration
    Runs locus-level SuSiE colocalization for short sleep duration.
  coloc-03-08-insomnia
    Runs locus-level SuSiE colocalization for insomnia.
  coloc-03-09-morning-person
    Runs locus-level SuSiE colocalization for morning person.
  coloc-03-10-snoring-adj-bmi
    Runs locus-level SuSiE colocalization for snoring adjusted for BMI.
  coloc-03-11-osa
    Runs locus-level SuSiE colocalization for obstructive sleep apnoea.
  coloc-03-12-daytime-sleepiness
    Runs locus-level SuSiE colocalization for daytime sleepiness adjusted for BMI.
  coloc-03-13-chronotype
    Runs locus-level SuSiE colocalization for chronotype.
  coloc-03-14-sleep-duration
    Runs locus-level SuSiE colocalization for sleep duration.
  coloc-03-15-napping
    Runs locus-level SuSiE colocalization for daytime napping.
  coloc-03-16-combine-results
    Combines signal-pair results across traits and writes the best pair per metabolite-sleep combination.
  coloc-04-01-dependencies
    Checks and loads overlap-clustering dependencies.
  coloc-04-02-path-configuration
    Defines the lipid IV table, PLINK reference panel, and output paths.
  coloc-04-03-read-iv-list
    Reads and validates the instrument list for each lipid.
  coloc-04-04-calculate-ld
    Uses PLINK to calculate the pairwise r-squared matrix for all instruments.
  coloc-04-05-pairwise-jaccard
    Calculates LD-based Jaccard overlap for every lipid pair.
  coloc-04-06-hierarchical-clustering
    Clusters lipids with distance 1 - Jaccard and selects the number of clusters.
  coloc-04-07-write-results
    Writes overlap matrices, pairwise tables, cluster assignments, and the Excel workbook.
  coloc-05-01-dependencies
    Checks and loads HyPrColoc dependencies.
  coloc-05-02-path-configuration
    Defines cluster, metabolite, sleep-GWAS, and output paths plus analysis thresholds.
  coloc-05-03-cluster-metadata
    Reads cluster members and maps metabolites to GWAS Catalog and HMDB identifiers.
  coloc-05-04-cluster-loci
    Defines the core hg38 locus for each lipid cluster.
  coloc-05-05-regional-extraction
    Extracts regional summary statistics from sleep and metabolite GWAS with caching.
  coloc-05-06-align-and-hyprcoloc
    Aligns effect alleles, runs HyPrColoc, calculates credible sets, and performs sensitivity analysis.
  coloc-05-07-write-results
    Writes cluster-level and combined HyPrColoc tables, the workbook, and the data dictionary.

Principal outputs
-----------------
- metabolite_<sleep trait>_colocalization_500kb_res.csv: ABF results by sleep trait.
- significantRSID_100kb_genesLIST.xlsx and significantRSID_100kb_NearestGeneLIST.xlsx: SNP-to-gene annotations.
- metabolite_<sleep trait>_colocalization_500kb_susie_res.xlsx and combined SuSiE tables: credible-set-level results.
- LDOverlap/: Jaccard matrix, pairwise results, cluster tables, heatmaps, and clustering workbook.
- HyPrColoc/: cluster-level and combined multi-trait colocalization tables, credible sets, and sensitivity plots.

Interpretation notes
--------------------
- PP.H4 supports a shared causal variant; PP.H3 supports distinct causal variants at the same locus.
- A status of no_credible_set in the SuSiE workflow means that colocalization cannot be evaluated in that window; it is not evidence against colocalization.
- The LD panel and the GWAS samples should come from the same ancestry background.
- Fixed run dates are removed from R Markdown headers and newly generated output names. Dates inside input filenames remain where they identify specific downloaded resources.
