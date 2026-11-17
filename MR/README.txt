METABOLITE-SLEEP MR ANALYSIS
===========================

Folder purpose
--------------
This folder contains one consolidated R Markdown document for the bidirectional two-sample Mendelian randomization (MR) analysis of plasma metabolites and sleep traits, together with pathway annotation, multiple-testing correction, and lipid sub-pathway Fisher tests.

Main document
-------------
MR_analysis.Rmd

How to run
----------
1. Open MR_analysis.Rmd in RStudio.
2. In the setup chunk, replace project_root <- normalizePath("Path", ...) with the path to the reviewer's project root.
3. Run the chunks in their displayed order. The setup chunk sets the working directory to MR/outputs.
4. Ensure all input files follow the data layout documented below. Full summary-statistics files are not distributed with this code package.
5. Paths inside GWAS_sleep_trait.xlsx are resolved automatically: an existing absolute path is used when present; otherwise the referenced filename is searched under Data/Sleep_GWAS.

The code does not contain dates in the R Markdown header or in newly generated output-file names. Dates retained inside input data filenames are identifiers required for locating the original datasets.

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
  mr-00-setup
    Defines the one-time project root, relative data locations, package dependencies, and per-step output folders.
  mr-01-01-prepare-data
    Reads the sleep-trait metadata and initializes the metabolite-to-sleep processing log.
  mr-01-02-harmonise-metabolites-to-sleep
    Selects and clumps metabolite instruments, loads outcome data, harmonizes alleles, and saves intermediate harmonized datasets.
  mr-01-03-run-metabolite-sleep-mr
    Runs IVW, MR-Egger, weighted-median, MR-RAPS, heterogeneity, pleiotropy, sensitivity plots, and assembles directional MR results.
  mr-02-01-prepare-data
    Initializes the reverse-direction MR analysis using sleep traits as exposures.
  mr-02-02-format-clump-sleep-exposures
    Formats and clumps genetic instruments for each sleep trait across the three LD-clumping schemes.
  mr-02-03-harmonise-sleep-to-metabolites
    Harmonizes sleep-exposure and metabolite-outcome summary statistics and stores the harmonized data.
  mr-02-04-run-sleep-metabolite-mr
    Runs the reverse-direction MR estimators, diagnostics, plots, and combined result tables.
  mr-03-01-load-gwas-catalog
    Reads and filters the GWAS Catalog export for the European metabolomics study.
  mr-03-02-merge-mr-with-catalog
    Merges metabolite-to-sleep MR results with reported metabolite names from GWAS Catalog.
  mr-03-03-bind-pathway-annotation
    Standardizes metabolite names and joins pathway, sub-pathway, and protein annotations.
  mr-03-04-select-har3-and-apply-fdr
    Selects the third harmonization scheme and calculates method-wide and sleep-trait-wide Bonferroni and FDR corrections.
  mr-03-05-bind-hmdb-and-ultimate-traits
    Adds HMDB identifiers and original metabolite names, sorts the final table, and writes total/significant result workbooks.
  mr-04-01-load-mr-results
    Loads the reverse-direction sleep-to-metabolite MR result table.
  mr-04-02-bind-pathway-annotation
    Adds pathway and sub-pathway annotations to sleep-to-metabolite MR results.
  mr-04-03-select-har3-and-apply-fdr
    Selects har3 results and applies multiplicity corrections across methods and sleep traits.
  mr-04-04-bind-hmdb-ultimate-traits-and-sample-size
    Adds HMDB IDs, ultimate trait names, and GWAS sample sizes, then writes the final reverse-direction tables.
  mr-05-01-dependencies
    Checks and loads packages required for the Fisher enrichment analysis.
  mr-05-02-path-configuration
    Defines the result, annotation, and Fisher-test output locations.
  mr-05-03-name-normalization
    Provides strict, case-insensitive, and loose metabolite-name normalization functions.
  mr-05-04-read-significant-results
    Reads the significant metabolite-sleep table used to define associated lipids.
  mr-05-05-read-pathway-annotation
    Reads panel pathway annotations and creates the single-lipid background set after excluding ratios.
  mr-05-06-map-metabolites
    Maps significant lipids to background rows using strict-to-loose matching and sub-pathway disambiguation.
  mr-05-07-outcome-sets
    Defines sleep duration, long sleep duration, short sleep duration, and their union.
  mr-05-08-fisher-function
    Constructs 2 x 2 tables and calculates Fisher exact P values, odds ratios, confidence intervals, FDR, and Holm corrections.
  mr-05-09-run-tests
    Applies the Fisher analysis to each outcome and checks the contingency-table counts.
  mr-05-10-sensitivity-analysis
    Repeats the enrichment tests using only lipids with an available MR estimate as background.
  mr-05-11-summary-tables
    Creates overview, wide, contingency, significant-member, and audit tables.
  mr-05-12-write-results
    Exports per-outcome CSV files and the formatted summary Excel workbook.

Principal outputs
-----------------
- clump_data/: clumped instrumental-variable datasets.
- harmonise_data/: harmonized exposure-outcome data in RData and CSV format.
- mr_res/: individual MR results, diagnostics, plots, and combined directional MR tables.
- metabolite_sleep_primary_*.xlsx and sleep_metabolite_primary_*.xlsx: corrected significant result tables.
- ult_har3_all_*_mr_bindres_pathway_HMDBID_ulttrait.xlsx: fully annotated result tables.
- Fisher's test/: Fisher exact-test tables, plots, and summary workbook.

Notes
-----
- The MR estimates use genetic variants as instruments and rely on relevance, independence, and exclusion-restriction assumptions.
- IVW, MR-Egger, weighted-median, and MR-RAPS estimates are retained; heterogeneity, directional pleiotropy, and leave-one-out diagnostics are also produced.
- The Fisher analysis treats all annotated single lipid metabolites as the primary background and provides a sensitivity analysis using lipids with an available MR estimate.
