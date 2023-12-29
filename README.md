# BATCH-FLEX-Shiny Tutorial
BatchFLEX: Comprehensive software for assessing and correcting batch effects, comparing batch correction methods, and exporting corrected matrices for downstream analysis

# Introduction
BatchFLEX is an intuitive Shiny App that can be viewed as a web interface through shiny.io or accessed locally by downloading and installing an app.R or docker container, or by installing an R-package. BatchFLEX is designed with a clean user interface and easy to follow steps to prompt users to input a data matrix and meta file, to select and implement the desired method of batch correction, to assess the impact of the batch correction method using side-by-side comparisons of pre and post corrected graphs and statistics, and finally to export a corrected matrix and accompanying diagnostics for downstream analysis. The intuitive user interface allows for hands-on evaluation and selection of the most optimal batch correction method for any dataset and for users with any background. BatchFLEX is the most comprehensive batch correction and evaluation tool and can easily be implemented into any pipeline using the R-package wrapper. Notable features of BatchFLEX include implementation of a wide variety of batch correction methods such as ComBat, ComBatSeq, Harman, LIMMA, RUVg, and Mean centering, incorporation of multiple different types of evaluation methods such as PCA, cluster analysis, gene-variable association analysis, scree plots, SVA, uMAP, RLE, boxplots, and heatmaps, and inclusion of a simple and comprehensive export feature for use of corrected matrices in downstream analysis and for record keeping to justify the correction method selection for publications. 

BatchFLEX can be accessed through shiny.io at (https://shawlab-moffitt.shinyapps.io/batch_flex/). Additionally, a companion R package can be found at (https://github.com/shawlab-moffitt/BATCHFLEX), which includes source code, example data, and an installation guide.

# Installation
The BatchFLEX suite can be downloaded (cloned) and installed through the GitHub repository. The downloaded file can be unzipped to a destination folder, which should be set as the working directory or file path. Of note, some of the example files (e.g., gene set files) use relative paths, so the program may fail to identify the file if a working directory is not properly set. The suite was developed in R version 4.1. 

*	**Install BatchFLEX suite GitHub repository**
    * git clone https://github.com/shawlab-moffitt/BATCHFLEX

*	**Download and unzip repository https://github.com/shawlab-moffitt/BATCHFLEX**
    * Set working directory to BatchFLEX folder
    * Install required R packages
    * Suite of tools was built on R version 4.2
    * R script for package installation is provided in the “1-Getting_Started” folder

# Load Example Data
BatchFLEX includes a sample dataset within the Shiny app, which was generated by harmonizing expression data originating from the ImmGen project. Users can access the sample dataset by simply selecting the “Load Example Data” button. In this dataset, study is considered the batch effect and cell type is considered the variable of interest. 

<img width="288" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/c99b05b0-a054-4367-b9eb-8045213e0df5">

# Input User Data
BatchFLEX provides an intuitive method for users to input and preview a matrix and meta file and to apply common preprocessing steps such as log 2 + 1 transformation, quantile normalization, Trimmed Mean of the M-values, or UpperQuartile normalization. 

<img width="276" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/70ab2e85-67b5-4767-9b2d-cbed56156ac1">

Key Input Files
1. Required Files
    * Gene expression file where the gene symbols are in the first column and the sample names are in the first-row header.
    * Meta information file that consists of a matrix with the first column the sample name. The remaining columns should contain the batch information and any variables of interest.
    	
2. Optional Files
    * Housekeeping Gene List is an optional housekeeping gene list for the RUVg correction method. The gene list should be a tab separated file with a single column of genes. 
    * Gene set of Interest is an optional list of genes for a pathway of interest that can be used with the gene set enrichment analysis. 

Matrix and meta files should be formatted similarly to the example datasets shown below. The matrix file should contain genes in the first column and sample IDs in the first row. The meta file should contain sample IDs in the first column and any accompanying meta information in subsequent columns. The sample IDs should match between the matrix and meta files. The meta file should include at least one column of known technical effects and at least one column of known biological effects. BatchFLEX defaults to selecting the second column of the meta file as the batch effect and the third column as the variable of interest. This can easily be changed by the user by simply selecting a different column using the dropdown menus, however, if only a single batch effect is being corrected, having this in the second column can save the user a lot of unnecessary clicking. 

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/a33dab7e-a465-4093-8142-e78803aaced0">

# Assessment of the Batch Effect
Uncorrected data can be visualized using PCA, which can be annotated by assigning color or shape to any criteria in the meta file or can be viewed with a user specified number of clusters generated by k-means. Users can use this feature to assess whether the samples are clustering according to batch or according to the variable of interest. In the example below, a clear batch effect can be seen as the samples are largely clustered according to batch with very little mixing of colors (batch).  BatchFLEX also generates a multiple components PCA plot, a scree plot, and a table displaying the contribution of variance for a user specified condition. The multiple components PCA plot can be colored by a user specified condition. By default, the multiple components plot displays the top 5 principal components, however, this can be adjusted by the user. These plots provide more details about the PCA analysis, which is important because the batch effect may not be found primarily in PC1 and PC2.

<img width="432" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/e63b6b50-3b62-4eff-8c10-14d3d6796707">

<img width="432" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/b66f31fb-9eae-41f5-9478-575ecfad44c7">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/fd96bb8b-5403-43e3-9ecd-a6fa51aaab15">

<img width="274" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/4c021622-f0bb-41b5-ac61-53f9743ecf0b">

BatchFLEX runs cluster analysis of k-means generated clusters using Elbow, Silhouette, and Dunn plots. The optimal number of clusters is indicated on both the Silhouette and Dunn plots by a dashed vertical line and is indicated by selecting the cluster number at the elbow of the curve in the Elbow plot. This analysis can help users determine if the optimal number of clusters correlates well with the biological variable of interest. Occasionally, the elbow plot can be difficult to interpret when the pivot point is more gradual. BatchFLEX avoids this issue by including multiple methods of cluster analysis. In BatchFLEX, clustering can also be analyzed using a heatmap, which can be annotated according to the meta data. By default, BatchFLEX plots the top 1000 genes by mean absolute deviation. The user can choose to plot more genes if desired, however, this may affect loading time. Additionally, the user can choose to use the coefficient of variance or the variance as alternatives to the mean absolute deviation. The heatmap of the example data shows that the batch effect is driving a significant amount of clustering.

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/41ed8c52-2ab7-441a-a442-521171ff9629">

<img width="435" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/649d4086-a5d8-4cb9-97d5-777d0653ebfe">

Users can also visualize unwanted variation derived from the batch effect using the sample-wise relative log expression plot, which can be colored according to the meta data. This plot shows the median (dot) and interquartile range (grey line) for each sample. The whiskers are set at 1.5 times the interquartile range and any outliers are indicated as a dot outside the whiskers. Users should use this plot to assess whether the batch effect changes the overall median of the data and whether the batch effect introduces significant variability. In the uncorrected RLE plot shown below, the median of GSE15907 (orange) differs significantly from the other studies in this dataset. GSE112876 (blue) and GSE37448 (green) also appear to be much more variable compared to the other studies in the dataset. These details hint that a batch effect is present in the dataset and may need to be removed to help improve the detection of biological differences in downstream analyses. Additionally, the explanatory variables plot, and the probability density plot are two methods included in BatchFLEX to broadly assess whether each gene is associated more with the batch effect or with the variable of interest. The explanatory variables plot displays the distribution of R-squared values across all genes for each user selected variable and the probability density plot displays the posterior probability that each gene is associated with the latent variable or the user selected variable of interest as measured by SVA. By default, the explanatory variables plot displays the top 10 variables by percent variance explained. The user can select the method that SVA uses to generate the latent variables.  In the explanatory variables plot shown below, a significant amount of variance is being explained by the batch effect, which further supports the need for batch correction. Although the probability density plot appears to show a much more muted impact by the batch effect, it does appear to show that a few genes are highly associated with the batch. In our example dataset, each of these plots support the need for batch correction.

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/a7fbb8d2-ec0b-41fd-bb16-954a9dea6cc6">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/5da3c42c-459f-4940-83c0-96b3d08b97fe">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/003935ca-f563-4c10-a136-b88cb36c99b3">

BatchFLEX can determine the impact of a batch effect or biological variable of interest at an individual gene level using a boxplot and using statistical tests such as the Wilcoxon rank-sum test, the t-test, the Kruskal Wallis test, and ANOVA. The boxplot also allows users to assess if outliers are present at the gene level. Users can inspect genes of interest that are expected to remain relatively constant across samples to identify a batch effect. In the example below, beta actin was selected as a housekeeping gene, which should have little variability across samples, however, according to the ANOVA test, beta actin differs significantly between batches, indicating a probable batch effect. Alternatively, users can use GSEA to assess the batch effect and to ensure that differences are still detectable in the variable of interest. BatchFLEX comes preloaded with approximately 100,000 gene sets, but also includes a user upload option for any gene sets not included within BatchFLEX. 

<img width="471" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/6ae2c15c-27e0-4b64-b04a-1d9203446ace">

<img width="461" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/e36674de-ce48-4360-a5db-0c69953b7ba3">

# Batch Correction Decision Tree
If correction is desired, BatchFLEX includes a wide variety of well documented and commonly used batch correction methods such as Limma, ComBat, Mean-Centering, ComBatSeq, Harman, and RUVg. Batch correction can easily be applied to the input matrix by simply selecting the desired batch correction method from a drop-down menu. Users can use Figure 16 as a guide for selection of an appropriate batch correction method. After the batch correction method is selected, the UI automatically updates with user selected inputs specific for the chosen batch correction method. A compilation of all user selectable inputs for each batch correction method can be seen in Figure 17. Generally, users will only need to select the column(s) with the batch information and the column with the variable of interest. RUVg is an exception and requires a list of housekeeping genes to use as a baseline. BatchFLEX includes a few different housekeeping gene lists that users can use with RUVg, however, the user also has the option to upload their own housekeeping gene list if desired. 

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/99ee5df1-5f9b-4163-aa8d-8a85e3318268">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/e86e4356-85dc-49bb-8761-ca279900ebc5">

BatchFLEX then automatically adjusts the data matrix according to the selected correction method and generates a new updated matrix that is fed into each of the assessment methods described above. Users should confirm that the corrected matrix is changing when selecting a batch correction method. 
<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/d95ec9f8-4264-4eed-937b-05f44889d0fd">

# Assessment of the Batch Correction
The corrected and uncorrected visualizations and statistical analyses are rendered side-by-side for easy comparison. With BatchFLEX, it is easy to determine whether the batch effect has been removed and to ensure that the biological variable is maintained. In Figure 19, a comparison PCA plot with the study (batch) data annotated by color, the samples are clustered by batch in the uncorrected PCA plot but are more intermingled in the corrected PCA plot. This indicates that batch is having a less profound impact on the variance in the corrected dataset. This this further emphasized in Figure 20, which is colored according to cell type. In this figure, similar cells are not grouping together in the uncorrected PCA plot but are grouping together better in the corrected PCA plot. 

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/0335ad8b-255c-4cdc-adea-aca05ae9ea83">

<img width="465" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/fd179604-7d07-41ec-863e-c24333383a49">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/38a9efcc-de06-4262-8e73-b93b7dab67b0">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/3a2295d4-0bc8-438e-bf27-8be4f295b14f">

Because this dataset is complex, the cluster analysis is less informative and does not consistently align well with the biological variable of interest. However, the silhouette plot changing from an optimal cluster of 6 (the number of studies included in the example data) to 2 (lymphoid and myeloid) is a step in the right direction. Also, the comparison heatmap appears to show less clustering based on the study with a more diverse group of studies within each cluster.

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/91893032-9781-4318-94a5-0202cfca9a74">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/2f881064-ca6f-4045-9272-2e4c3deb8715">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/671e45eb-64f9-48d2-8f48-8d81811223be">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/23f089f1-05f9-40b5-8ec8-1652aefa4136">

Figure 25 is a comparison relative log expression plot that is colored by study (batch) and shows that the median and variability effects from the batch have been removed following correction. GSE112876 (blue) and GSE37448 (green) are much less variable and have medians that more closely match the remaining studies in the dataset. The effect of batch correction is also shown in Figure 26, which shows a significant reduction in the variance explained by the batch. Importantly, the variance explained by the variable of interest is still maintained. This indicates that the batch correction performed well in this instance and differences resulting from the variable of interest should still be detectable in downstream analyses. These results are further echoed in Figure 27, which shows that the probability that a gene is associated with the latent variable is reduced, but the probability that a gene is associated with the variable of interest is maintained. 

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/aaba0d13-2f20-4d95-b71d-3ea9fba5c11e">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/70a1a39d-62ce-4644-bb98-43557441edc5">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/3d5f11dc-9932-4032-baaa-6f940eecc2fd">

In Figure 28, the median expression of the housekeeping gene, beta actin, has been corrected and no longer differs significantly between batch (ANOVA p = 1). In Figure 29, gene set enrichment analysis was performed using the HSIAO housekeeping gene set, which showed a significant difference in enrichment in the uncorrected dataset, but no significant difference in the corrected dataset. The expression of KIT is shown in Figure 30, which is highly expressed in stem cells. A significant difference in expression is seen in both the uncorrected and corrected data sets (ANOVA p < 2.2e-16) with stem cells expressing the highest levels of KIT. Additionally, in Figure 31, the expression of MYB is shown, which should be upregulated specifically during T and B cell development. After correction, precursor T and B cells still display the highest levels of MYB expression. These results highlight that changes due to the variable of interest are maintained post correction. Cumulatively, these results show that the batch correction method was successful at removing the batch effect and maintaining the variable of interest, indicating that an appropriate level of correction was obtained and over or under correction did not occur.  

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/27f50b2c-6e9c-476d-8cbc-4201e4dc388b">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/71ad59f3-141c-42b7-a9bf-98a4d37fbb26">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/daf93dd3-756d-43f5-aaaf-8438344b2007">

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/56aa67b9-6e1f-4b04-874b-14c2d09aa196">

# On-the-fly Comparisons
Users can easily switch between batch correction methods by simply selecting a different method from the drop-down menu. BatchFLEX will automatically update, allowing on-the-fly comparisons of batch correction methods to ensure that the most optimal method is chosen for a particular dataset. 

<img width="468" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/0091a1bd-11d9-4f3e-a0d4-f8a6c737f0c5">

# File Export
After the user is satisfied, BatchFLEX can easily export the updated matrix for downstream analysis and any desired diagnostic figures for posterity. These files can then be downloaded as a single ZIP file for easy data management. Additionally, if desired, a full report can be generated as an HTML or PDF. To generate the ZIP file, users should simply click the save button for any desired plots, graphics, or matrices. After all files have been selected, the user can move to the file export tab and can designate a name for the ZIP file. Users can also use the drop-down box to confirm that the files have been saved to the ZIP folder and can remove any undesired files from the folder. After the user is satisfied, all files in the folder will be zipped into a single folder and downloaded. 

<img width="462" alt="image" src="https://github.com/shawlab-moffitt/BATCH-FLEX-ShinyApp/assets/89986836/1ed52211-e71b-4d60-8244-c46cb5ed076e">

























































