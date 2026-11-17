REPEATED MR ANALYSIS
====================

Folder purpose
--------------
This folder contains the consolidated R Markdown document for the repeat MR analysis. It documents the PhenoScanner/PheWAS summary and re-estimates MR after removing prespecified potentially pleiotropic instruments.

Main document
-------------
RepeatMR_analysis.Rmd

How to run
----------
1. First run the required MR steps that create MR/outputs/harmonise_data and the significant MR table.
2. Open RepeatMR_analysis.Rmd and replace project_root <- normalizePath("Path", ...) in the setup chunk.
3. Run the chunks in order. Results are written to RepeatMR/outputs.

Inputs
------
- Data/PhenoScanner/trait_variants_annotated_1.txt through trait_variants_annotated_5.txt.
- Data/Reference/repeatMR_removedIVs.xlsx.
- MR/outputs/metabolite_sleep_primary_fdrmethod0.05.xlsx.
- MR/outputs/harmonise_data/*_har.RData.

Code-block purposes
-------------------
  repeat-mr-00-setup
    Defines the shared project root, data locations, package dependencies, and RepeatMR output directory.
  repeat-mr-01-01-phenotype-summary
    Combines the five PhenoScanner exports and summarizes significant secondary phenotypes of the instruments.
  repeat-mr-02-01-repeat-mr
    Reloads harmonized data, removes specified instruments, and repeats MR estimators plus plots and result assembly.

Principal outputs
-----------------
- Phenotype_summary_sigres_IVs.xlsx.
- Phenotype_summary_sigres_IVs_uniqueTraits.csv.
- mr_res/metabolite_sleep_mr_repeat_bindres.csv and .xlsx.

The document omits fixed run dates from headers and newly generated output names. Dates inside original downloaded input filenames are retained because they are part of the source-data identifiers.
