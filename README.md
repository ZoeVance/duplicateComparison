# Data and code for analysis for "Ohnologs and SSD paralogs differ in genomic and expression features related to dosage constraints"

This repository contains the required code and data to produce results presented in <REF>. 
Additional software used:
* [CAIcal](http://genomes.urv.es/CAIcal/)
* [IUPred](https://iupred2a.elte.hu/)
* [Gprofiler](https://biit.cs.ut.ee/gprofiler/page/docs)


## Code

Analysis code used to process/generate data in this study; in the analysisCode/ directory

**WGD_vs_SSD_final_clean.ipynb:** Python code including data cleaning and processing, some statistical analysis and figure production

**Duplicate_age_regression.ipynb:** Regression analysis generating values for each gene feature adjusted for age

**Random_forest_classifier_final:** Code generating a random forest classifier and estimating feature importance


## Input data

Datasets required to generate outputs in this study in combination with code above; in the inputDatasets/ directory. References for the source files present are given in <REF>

**segments/:** Syntenic segments generated in Nakatani et al. 2021; map/ directory contains files genomic coordinates in extant genome for each segment, seg/ directory contains files with genes (Ensembl protein ids; file with gene ids in suppData/) contained within each segment; the HUMAN.txt files are used in this study in each case

***.tpms.tsv:** Expression data from sources given in <REF>

***v99\*.txt:** Files downloaded from Ensembl biomart

**cdsSequence.fa:** Longest CDS sequence associated to the gene id given in the header

**exonCoords.txt, exonEnds.txt:** Genomic location of exons by transcript ID

**GenicIntolerance_v3_12Mar16.txt:** Source file for RVIS values

**gnomad.v2.1.1.lof_metrics.by_gene.txt:** Source file for missense Z score and pLI

**GTEx_Analysis_2017-06-05_v8_RNASeQCv1.1.9_gene_median_tpm.gct:** Median TPM values per tissue from GTEx v8

**hsapiens.Pairs.\*.2R.txt:** Ohnolog pairs from Singh et al. at cutoffs of varying stringency

**HuRI_2_10_2019.tsv:** PPIs from the Human Interactome

**ohnologs_2010.csv:** Ohnolog pairs from Makino and McLysaght 2010

**phi_scores.txt:** Source file for Phi scores

**shet_estimates.csv:** Source file for S<sub>het</sub> scores

## Generated data

Data created in the course of this study; in the suppData/ directory.

**geneFeatures.db:** SQLite database fill with gene features used for between-group comparisons and paralog pair status used for group assignment

_Tables_:
* gene_features:
Table including all coding genes obtained from Ensembl and features assigned either from Ensembl or other datasets (see Input Data and <REF> for details)
  * **id:** gene id as given in sequence file headers in dipteraTranslations_processed/
  * **trans_id:** Ensembl ID for the transcript used for estimating other features (e.g. transLen); longest transcript
  * **prot_id:** Ensembl ID for the translation used for estimating other features (e.g. CAI); longest translation
  * **name:** Associated gene name from Ensembl - used to cross reference to some external datasets (e.g. CDS length)
  * **chrom:** Chromosomal gene location
  * **start:** Genomic start coordinate (GrCH38)
  * **end:** Genomic end coordinate
  * **strand:** Strand the gene is located on (1/-1)
  * **genLen:** Genomic length
  * **cdsLen:** Length of coding sequence for longest translation
  * **transLen:** Length of longest transcript
  * **transCount:** Number of alternative transcript isoforms
  * **intCount:** Number of introns in longest transcript
  * **intCov:** Proportion of longest transcript covered by introns
  * **intAvg:** Average intron length in longest transcript
  * **gc:** % GC content for the gene
  * **gc3:** % GC content at the third codon position using longest translation
  * **domains:** Number of Pfam domains annotated for longest translation
  * **u_domains:** Number of unique Pfam domain types
  * **max_exp:** Maximal expression from GTeX and additional developmental expression datasets
  * **ess:** Cellular essentiality from Wang et al.
  * **PPIs:** Number of protein-protein interaction pairs gene is listed in from the Human Interactome 
  * **EvoTol:** Evolutionary tolerance score
  * **loftool_percentile** LoF tool percentile rank
  * **Phi:** Probability of haploinsufficiency
  * **mis\_Z\_score:** Missense Z\-score
  * **pLI\_score:** Probability of LoF intolerance
  * **s\_het:** S<sub>het</sub> score
  * **RVIS:** Residual variation intolerance score
  * **CAI:** Codon adaptation index, calculated on longest translation
  * **IntDisProp:** Proportion of longest protein determined to be intrinsically disordered
  * **EvolRate:** Evolutionary rate, given as *dN/dS*
  * **specificity:** Expression specificity calculated on 
  * **youngestNode:** Youngest duplication node associated with this gene (per Ensembl)
  * **oldestNode:** Oldest duplication node associated with this gene
  * **dupCat\_2010:** Duplicability category based on 
  * **dupCat\_singh:** Duplicability category based on Singh et al. 2019
  * **dupCat\_2020:** Duplicability category based on ohnologs determined in this work from the reconstruction of Nakatani et al. 2021
  * **dupCat\_maj:** 'Majority rules' category assignment from previous three assignments
  * **dupCat\_all:** Category assignment if all 3 datasets agree on assignment, otherwise null
* paralogClassification:
All paralog pairs (and singleton genes), listed with various information and classifications
  * **id:** Ensembl gene ID
  * **para:** A paralog of id
  * **ageNode:** LCA of the paralogs according to Ensembl
  * **dN:** dN values between paralogs
  * **dS:** dS values between paralogs
  * **singleton:** F or NULL, whether the row represents an unduplicated gene (all previous columns will be NULL)
  * **retro:** T if paralog pair is classed as originating from a retroduplication, F or NULL otherwise
  * **preVertebrata:** T if paralogs arise from a duplication that occurred before the Vertebrata node according to ageNode, F otherwise
  * **ohno2010:** T if paralog pair is classed as originating from WGD according to the Makino and McLysaght dataset, F otherwise
  * **ohnoSingh:** T if paralog pair is classed as originating from WGD according to the Singh et al. dataset, F otherwise
  * **ohno2020:** T if paralog pair is classed as originating from WGD according to the dataset generated in this study, F otherwise

**overRep_\*_final.csv:** Over-represented GO terms as defined by gProfiler for each duplicability category; SSD, WGD and singleton (Sing). Version with a third column containing the p-value is also provided.

**underRep_\*_final.csv:** Under-represented GO terms as defined by gProfiler for each duplicability category; SSD, WGD and singleton (Sing). Version with a third column containing the p-value is also provided.

**genesInSeg_coding.txt:** Coding genes present in syntenic segments from Nakatani et al. 2021, Ensembl gene ids 

**ohnologsWholeGenome_gapAllowed8_minBlock3.txt:** Ohnolog pairs derived from syntenic segments in Nakatani et al. 2021 according to method described in <REF>

