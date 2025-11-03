# Spatiotemporal gene expression dynamics reveal the reproductive signaling regulators in Norway spruce
This repository provides the scripts, data and docker environments required to reproduce the analysis performed in our manuscript titled "Spatiotemporal gene expression dynamics reveal the reproductive signaling regulators in Norway spruce" that is currently under Revision. 
Below are the instructions to perform the analysis starting with setting up the R environment for the analysis using our pre-built docker containers, separate for each part of the analysis for an isolated clean environment. This way you do not have to R or any packages yourself.

## Software requirements
- Docker (v20+)
- Git

## Docker container for R environment and packages
Git clone this repository, via the commands below. This way the scripts and data structure is preserved.

```bash
git clone https://github.com/giacomellolab/SpruceProject.git
cd SpruceProject
```

Now pull our docker containers which are needed for each of our analysis. Each analysis is marked by one sub-folder under the scripts folder of the GitHub repository and scripts are placed inside these sub-folders. Below are the docker containers for each analysis and the commands to pull them.

For [clustering and bracts vs scales analysis](scripts/clustering_and_BvS-analysis) performed in this study, type the command below (via terminal in the parent directory) to pull the docker container enclosing the environment for this analysis.
```bash
docker pull yuvaranimasarapu/r-env-spruce:clusteringv1.0
```

For the analysis with [stdeconvolve](scripts/stdeconvolve), pull the docker environment by running the command below:
```bash
docker pull yuvaranimasarapu/r-env-spruce:stdeconvolvev1.0
```

Now below are the steps on how to run the docker containers along with R scripts for each of the analysis.

## Raw Data
The fastq files from the sequencing data can be extracted from the GEO project repository *GSE288244*.

## Clustering and Bracts vs Scales Analysis
The 16 clusters mentioned in the manuscript are identified through the clustering analysis. All the scripts used in this analysis are numbered in the same order as they appear in the analysis pipeline. The scripts are found [here](scripts/clustering_and_BvS-analysis). 

In order to run the Rscripts for this analysis, we need to use the docker container which encloses it's R environment. Type the following in your terminal (within R console or terminal app) to run the docker container.
The count matrices and corresponding brightfield images needed as input for these scripts are accessed from our Data Mendeley repository with currently reserved [DOI:10.17632/b7fppw63v8.1](https://data.mendeley.com/preview/b7fppw63v8?a=0a093701-dffc-4dd8-bdab-bb372579088) that will be made public upon publication. Each file/object is clearly described in the description part of this repository for easier understanding.

```bash
docker run --rm -v $(pwd):/SpruceProject -w /SpruceProject yuvaranimasarapu/r-env-spruce:clusteringv1.0 \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/1_LoadData_Filtering.Rmd', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/2_Normalisation_filteredData.Rmd', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/3_ClusterAnalysis.Rmd', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/4_Visualisation_Plots.Rmd', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/5_MarkerGene_analysis.Rmd', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/6_bracts_vs_scales.Rmd', output_dir = 'results')"
```

The scripts are numbered in the order of analysis starting with data processing, QC, normalization, clustering, DE analysis, and lastly followed by gene expression between the bracts and scales.
Each markdown script depends on the results from the previous one — for example:
- 1_LoadData_Filtering.Rmd produces a cleaned dataset after loading the count matrices and images in R.
- 2_Normalisation_filteredData.Rmd loads the cleaned (filtered) seurat object and performs normalization (SCTransform) on it.
- 3_ClusterAnalysis.Rmd uses the normalized dataset, performs integration and clustering on it.
- 4_Visualisation_Plots.Rmd creates plots that were used to visualize the clustering results.
- 5_MarkerGene_analysis.Rmd extracts the cluster markers and performs the DE analysis.
- 6_bracts_vs_scales.Rmd extracts the DE genes from the bracts and scales comparisons.

## STdeconvolve
In order to run the Rscripts for this analysis via its docker container, type the following in your terminal (within R console or terminal app).

The seurat objects needed to re-run this analysis are available on our Data Mendeley repository [DOI:10.17632/b7fppw63v8.1](https://data.mendeley.com/preview/b7fppw63v8?a=0a093701-dffc-4dd8-bdab-bb372579088). Detailed descriptions of the analysis and objects are also provided in this repository.

```bash
docker run --rm -v $(pwd):/SpruceProject -w /SpruceProject yuvaranimasarapu/r-env-spruce:stdeconvolvev1.0 \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/<stdeconvolve.Rmd>', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/<stdeconvolve.Rmd>', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/<stdeconvolve.Rmd>', output_dir = 'results')"
```

## Trajectory analysis
In order to run the Rscripts for this analysis, go to the sub-folder [trajectory_analysis](scripts/trajectory_analysis) under scripts and run the markdown files present there. [1_241103_SubLatAcro_reanalysis_final.Rmd](scripts/trajectory_analysis/1_241103_SubLatAcro_reanalysis_final.Rmd) scripts performs trajectory analysis on the lateral organs subset from acrocona bud tissue sections. And the script [2_241103_SubLatFem_reanalysis_final.Rmd](scripts/trajectory_analysis/2_241103_SubLatFem_reanalysis_final.Rmd) produces the results from the trajectory analysis performed on the lateral organs clusters from the female bud tissue sections.

The seurat objects needed to reproduce this analysis can be downloaded from our Data Mendeley repository with currently reserved [DOI:10.17632/b7fppw63v8.1](https://data.mendeley.com/preview/b7fppw63v8?a=0a093701-dffc-4dd8-bdab-bb372579088) that will be made public upon publication.

```bash
docker run --rm -v $(pwd):/SpruceProject -w /SpruceProject <> \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/1_241103_SubLatAcro_reanalysis_final.Rmd', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/2_241103_SubLatFem_reanalysis_final.Rmd', output_dir = 'results')"
```

## ST pipeline docker
If you want to run the [ST pipeline](https://github.com/jfnavarro/st_pipeline/tree/1.7.9) which was used in this study to convert the sequencing fastq files to genecount matrices for the other analyses, you can do so by using our docker container that contains the exact version of the ST pipeline package (v1.7.9) that was used for tha fastq files in this study. TAGGD and STAR (v2.7.1a) are also installed in this container which are required for ST pipeline to run.

```bash
docker pull yuvaranimasarapu/r-env-spruce:st_pipeline1.7.9
```

And then run ST pipeline within this docker container.

```bash
docker run --rm yuvaranimasarapu/r-env-spruce:st_pipeline1.7.9 st_pipeline_run.py -h
```

An example run with dummy 'test' experiment data with fastq files, file1.fastq and file2.fastq, would look like this.

```bash
docker run --rm -v /data:/data yuvaranimasarapu/r-env-spruce:st_pipeline1.7.9 \
--expName test --output-folder /data/out --ids ids_file.txt --ref-map path_to_index \
--log-file log_file.txt --output-folder /results \
--ref-annotation annotation_file.gtf file1.fastq file2.fastq 
```

## DAPseq data 
To run the processing and analysis pipeline for the DAPseq experiments performed on the SPL1 TF in Norway spruce, go to GitHub repository [DAPseq_SPL1_spruce](https://github.com/TeiturAK/DAPseq_SPL1_spruce) where complete details to run the analyis as well as the required scripts are provided.
