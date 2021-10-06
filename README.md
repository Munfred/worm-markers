# worm-markers

### This is a work in progress and the code may be messy/buggy/not work at all.
If you have questions please contact Eduardo da Veiga Beltrame - edaveiga@caltech.edu - https://munfred.com 



**The best working tutorial notebook is this one:** `2021_10_05_interactive_+_static_swarmplot_Finding_C_elegans_neuron_markers_with_the_CeNGEN_dataset.ipynb`

**Which you can open directly in Colab:** https://colab.research.google.com/github/Munfred/worm-markers/blob/master/2021_10_05_interactive_%2B_static_swarmplot_Finding_C_elegans_neuron_markers_with_the_CeNGEN_dataset.ipynb



### Summary
This is a simple computational workflow for using single cell RNA sequencing (scRNAseq) data to identify promising marker gene candidates in C. elegans. This workflow leverages the scvi-tools package (https://scvi-tools.org), which uses deep learning to create a probabilistic model of gene expression in single cells. The workflow is composed of three steps: 

1) Training the scVI model on scRNAseq data;

2) Performing differential expression (DE) between the cell type of interest and all other cells;

3) Visualizing the relative expression levels of top DE genes across all cell types to identify promising candidates with an interactive plot.


### Workflow Description
In principle, there are only two features that make a “good” marker gene for a given cell type: having high specificity to the cell type of interest and having high expression. We reasoned that these features should be possible by using annotated scRNAseq data that includes the cell types of interest. 

By repeatedly sampling each cell, scVI allows the inference of transcript “rates”, corresponding to the fractions of transcripts sampled in a given cell that belong to each gene. This is akin to the normalized gene expression that is used in other scRNAseq processing frameworks. However, it is an inference instead of an empirically estimated value  obtained by for example dividing the number of transcripts sequenced in each cell by the total. We refer to it as the expression rate of a gene or simply expression of a gene.

We conceived the following three step workflow to look for high expression and high specificity genes using scVI:

**i) Train the scVI model to integrate the datasets of interest.**

scVI creates a latent representation of each cell that is independent of cell size/sequencing depth (the number of transcripts seen in that cell) and of batch (which experimental run a sample came from). Thus, cells that are closer in the latent representation should be more biologically similar (that is, express the same genes in similar quantities). 

**ii) Perform DE on the cells of interest to select top genes by DE probability**

Once the scVI model has been trained, the second step is to select the cells of interest and perform DE using the scVI change mode. To visualize specificity and expression rate on the target cells we can make a scatter plot of the posterior predictive p-value  vs. expression frequency for each gene. 

**iii) Visualize the relative expression across all tissues to select marker candidates.**

Once the p-values for each gene are computed, the genes with the lowest p-values can be selected for visualizing their expression in every cell type using a “swarm plot”. This is a vertical scatter plot of the expression frequency on each cell type normalized against the expression in the target cell, repeated for each gene. Then the expression on the cell type of interest is one, and for a highly specific gene it should be close to zero in all other cell types. Inspection of this plot quickly reveals whether there are clear marker gene candidates.

<img width="1433" alt="Screen Shot 2021-06-02 at 00 21 08" src="https://user-images.githubusercontent.com/12504176/120440506-7a28a100-c338-11eb-8d40-e7f7e45e6a91.png">
