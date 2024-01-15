# The Andes as a semi-permeable geographical barrier: genetic connectivity between structured populations in a colorful and widespread spider 

Fabian C. Salgado-Roa, Carolina Pardo-Diaz, Nicol Rueda-M, Diego F. Cisneros-Heredia, Eloisa Lasso, Camilo Salazar

This reposotitory contains scripts to replicate the results obtained in our study. Most of the input files to run this analyses are available in [zenodo](https://zenodo.org/records/10512159)

## SNP filtering

We used VCFtools and Plink to filter an generate input files for multiple softwares using [This script](https://github.com/fcsalgado/rad_scripts/blob/master/vcftools_filtering.sh).

We also built a script to randomly select one SNP per tag. It can be used for any VCF file. Access [here](https://github.com/fcsalgado/rad_scripts/blob/master/random_snps_rad.sh).

## Genetic variation and population structure

1. To calculate the principal component analysis (PCA), we used the following line from plink (Remember to generate plink files following the previous step)

```bash
plink -bfile gasteracantha_total --pca 4 --out gasteracantha_total
```

Plot in R using as input the output file _.eigenvec_

2. To run DAPC, we use [this script][https://github.com/fcsalgado/rad_scripts/blob/master/DAPC.R]

3. We ran fineRADStructure replicating the lines from [here](https://github.com/millanek/fineRADstructure), using the parameters from the main text.

4. Run Fastructure following [these lines](https://github.com/fcsalgado/rad_scripts/blob/master/run_admixture_FastStructure.sh)

5. To run AMOVA follow [this](https://github.com/fcsalgado/rad_scripts/blob/master/amova.R)

6. We calculate the summary stats based on th following scripts: 

    - [Tajima's D](https://github.com/fcsalgado/rad_scripts/blob/master/tajimasD.sh)
    - [Nucleptide diversity](https://github.com/fcsalgado/rad_scripts/blob/master/population_windowedPi.sh)
    - [Heretozigosity](https://github.com/fcsalgado/rad_scripts/blob/master/he.sh)

## Spatial connectivity and gene flow

All the input files to replicate the analysis of [EEMS](https://github.com/dipetkov/eems) are available [here](https://zenodo.org/records/10512159)

## Phylogenetic inference

1. Run RAxML follow the code [here](https://github.com/fcsalgado/rad_scripts/blob/master/run_raxml.sh)
2. Run FAstME following the code [here](https://github.com/fcsalgado/rad_scripts/blob/master/fastme.txt)

## Demographic modelling

### Validate the number of genetic clusters and test for alternative demographic scenarios

For this step we followed the instructions and recommendations given in the reporsitory of the package [delimitR](https://github.com/meganlsmith/delimitR/blob/master/fullmanual_v2.md), and added some lines to parallelized [Fastsimcoal](http://cmpg.unibe.ch/software/fastsimcoal26/) to make the run faster. The code for this is available [here](https://github.com/fcsalgado/rad_scripts/blob/master/run_delimitR.R). All the input to replicate this analysis is available [here](https://zenodo.org/records/10512159)

### Parameters estimation

After selecting the best model, we estimated the divergence times, population sizes, migration rates and their respective confidence intervals using fastsimcoal26. Confidence intervals for each parameter were obtained by non-parametric bootstrapping by generating 100 datasets of 500 tags by randomly resampling with replacement from the entire dataset. For each dataset we generated a SFS using easySFS and added the monomorphic sites with custom scripts to ensure more precise parameter estimation. For more details go to the methods of the paper. This step was accomplished following these steps: 

1. Select randomly with replacement 500 tags from the whole dataset to estimate parametes. Use the script [create_random_vcfs_tags.sh](https://github.com/fcsalgado/rad_scripts/blob/master/create_random_vcfs_tags)
2. create a SFS per random VCF using the script [sfs_of_randomVCFs.sh](https://github.com/fcsalgado/rad_scripts/blob/master/sfs_of_randomVCFs.sh)
3. add invariable sites to estimate params [add_invSites_SFS.sh](https://github.com/fcsalgado/rad_scripts/blob/master/add_invSites_SFS.sh)
4. Run fsc26 for each dataset with the script [param_est.sh](https://github.com/fcsalgado/rad_scripts/blob/master/param_est.sh)
5. After this run the modification of joana meier [script](https://raw.githubusercontent.com/speciationgenomics/scripts/master/fsc-selectbestrun.sh) to select the best runs, saved as [best_runs.sh](https://github.com/fcsalgado/rad_scripts/blob/master/param_est.sh)
6. in R calculate the confidence intervals and mean, run [calculate_bootstrap.R](https://github.com/fcsalgado/rad_scripts/blob/master/calculate_bootstrap.R)

## Species distribution modelling 

All the scripts and inputs are available [here](https://zenodo.org/records/10512159)

## Effect of abiotic variables on genetic connectivity

For this we followed a workflow described in detail [here](https://academic.oup.com/mbe/article/40/5/msad094/7153257) and [here](https://github.com/evlynpless/SPRUCE-RF/blob/master/SPRUCE_Guide.txt). We limited the environmental sampling to the variables relevant for the distribution of the species (Supplementary Figure 2), and because most of our sampling was around the Andes, we accounted for spatial autocorrelation by adding a Kernel density raster of bandwidth 200 Km as an additional predictor of the migration rate. All the inputs to run this analysis are avilable [here](https://zenodo.org/records/10512159). After running EEMS, we generated the output as follows: 

1. [Generate the kernel](https://github.com/fcsalgado/rad_scripts/blob/master/generate_kernel.R)
2. Extract variable values for the localities used in EEMS with [this script](https://github.com/fcsalgado/rad_scripts/blob/master/extract_values_variables_eems.sh)

Finally we run the random-forest model as specified in [this script](https://github.com/fcsalgado/rad_scripts/blob/master/run_RF_spruce.R)

## Raw reads 

Raw reads are deposited in the [SRA](link). The sequences are coded following [this key](https://zenodo.org/records/10512159). The number of the individuals match the codes from supplementary table 1 individual_code


