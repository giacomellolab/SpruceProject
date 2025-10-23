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

For [trajectory analysis](scripts/trajectory_analysis), docker container for the R environment is pulled via:
```bash
docker pull <>
```

Now below are the steps on how to run the docker containers along with R scripts for each of the analysis.

## Clustering and Bracts vs Scales Analysis
The 16 clusters mentioned in the manuscript are identified through the clustering analysis. All the scripts used in this analysis are numbered in the same order as they appear in the analysis pipeline. The scripts are found [here](scripts/clustering_and_BvS-analysis). 

In order to run the Rscripts for this analysis, we need to use the docker container which encloses it's R environment. Type the following in your terminal (within R console or terminal app) to run the docker container.

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
1_LoadData_Filtering.Rmd produces a cleaned dataset after loading the count matrices and images in R.
2_Normalisation_filteredData.Rmd loads the cleaned (filtered) seurat object and performs normalization (SCTransform) on it.
3_ClusterAnalysis.Rmd uses the normalized dataset, performs integration and clustering on it.
4_Visualisation_Plots.Rmd creates plots that were used to visualize the clustering results.
5_MarkerGene_analysis.Rmd extracts the cluster markers and performs the DE analysis.
6_bracts_vs_scales.Rmd extracts the DE genes from the bracts and scales comparisons.

## STdeconvolve
In order to run the Rscripts for this analysis via its docker container, type the following in your terminal (within R console or terminal app).

```bash
docker run --rm -v $(pwd):/SpruceProject -w /SpruceProject yuvaranimasarapu/r-env-spruce:stdeconvolvev1.0 \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/<stdeconvolve.Rmd>', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/<stdeconvolve.Rmd>', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/<stdeconvolve.Rmd>', output_dir = 'results')"
```

## Trajectory analysis
In order to run the Rscripts for this analysis via its docker container, type the following in your terminal (within R console or terminal app). 1_241103_SubLatAcro_reanalysis_final.Rmd scripts performs trajectory analysis on the lateral organs subset from acrocona bud tissue sections. And the script 2_241103_SubLatFem_reanalysis_final.Rmd produces the results from the trajectory analysis performed on the lateral organs clusters from the female bud tissue sections.

```bash
docker run --rm -v $(pwd):/SpruceProject -w /SpruceProject <> \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/1_241103_SubLatAcro_reanalysis_final.Rmd', output_dir = 'results')" && \
  R -e "rmarkdown::render('scripts/clustering_and_BvS-analysis/2_241103_SubLatFem_reanalysis_final.Rmd', output_dir = 'results')"
```




