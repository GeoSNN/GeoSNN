GSNN code documentation 
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.7833891.svg)](https://doi.org/10.5281/zenodo.7833891)

Table of Contents
1. Introduction
2. Dependencies
3. Hardware Requirements
4. Optimization
a. Preparing your dataset
b. Instructions
c. Functions and Usage
d. Tuning and Configuration
5. Inference
	a. Instructions
	b. Single sample inference
	c. Batch inference
	d. Visualizing Individual feature functions
6. Support
7. Citation
----------------------------------------------------------------------------
----------------------------------------------------------------------------

1. Introduction

This documentation is for the supplemental code of the paper: 
K. Youssef, K. Shao, S. Moon & L.-S. Bouchard Landslide susceptibility modeling by interpretable neural network.  Communications Earth & Environment https://doi.org/10.1038/s43247-023-00806-5

It demonstrates how to implement the SNN optimization pipeline for landslide susceptibility modelling, and provides instructions on using a trained SNN model for inference and visualization. 

The optimization process consists of four main steps: tournament ranking (backwards elimination and forward selection) and neural network training (teacher MST network training and SNN network training). This document provides information on the dependencies, hardware requirements, instructions, usage, tuning, and configuration.


2. Dependencies

- Implemented using MATLAB 2022b

- Requires the Statistics and Machine Learning Toolbox

- The oldest tested MATLAB version is 2021a

- Should work on earlier MATLAB versions 2017a or later (not tested yet)

*Earlier versions of MATLAB might require the deep learning toolbox


3. Hardware Requirements

- Minimum recommended RAM: 64GB

This code heavily utilizes parallel processing. It is designed for workstations with a high number of CPU cores.  However, it has been successfully tested on a laptop with i7-11800H Intel CPU and 64GB RAM, using 4 CPU cores, and took approximately 90 minutes to run under these settings. 

If more RAM is available, the number of utilized CPU cores can be increased for faster processing. 

If you have less than 64GB RAM, you can try reducing the number of utilized CPU cores.

To set the number of utilized CPU cores, use the command parpool(n) before you run the script, where n is the number of utilized CPU cores.


4. Optimization

a. Data Preparation

Prepare your data as follows:

An accompanying dataset for this demonstration ("GSNN_Demo_Data.mat") can be downloaded from https://dataverse.ucla.edu/dataset.xhtml?persistentId=doi:10.25346/S6/D5QPUA.

To prepare a dataset for your own data:

Save your feature maps in an NxRxC array called geo_features, where N is the number of features, R is the number of rows, and C is the number of columns.

Save your ground truth map in an RxC logical array called geo_targets, where pixels with a value of 1 are landslides, and non-slide pixels have a 0 value.

Save your exclusion map in an RxC logical array called geo_exclude, where the areas you want to include in your study are assigned pixel values of 1, and the areas you want to exclude from your study are assigned pixel values of 0 (e.g., rivers).

b. Instructions

	1. Download the dataset "GSNN_Demo_Data.mat" from https://dataverse.ucla.edu/dataset.xhtml?persistentId=doi:10.25346/S6/D5QPUA.

	1. Adjust the number of utilized CPU cores for parallel processing based on your available RAM by using the `parpool(n)` command, where `n` is the number of utilized CPU 	cores. Larger `n` = faster processing.

	2. Run the provided `GSNN_DEMO` script for the full pipeline. You can also call the functions individually in the order specified in section 6.
		
	3. A window for loading the dataset will appear. Select the dataset 

	4. Perform the optimization multiple times with different initial conditions and select the model with the highest AUC.

	5. A window for saving the trained SNN model will appear once the optimization is complete.

c. Functions and Usage

The optimization process consists of executing the following functions in the specified order:

	1. Data preparation – Processing the data for optimization (`GSNN_Data_Prep`)

	2. Tournament ranking - Backwards elimination (`GSNN_TR1`)

	3. Tournament ranking - Forward selection (`GSNN_TR2`)

	4. Teacher MST network training (`GSNN_MST`)

	5. SNN network training (`GSNN_SNN`)

d. Tuning and Configuration

The provided example in the demonstration (`GSNN_DEMO`) has been tuned with a fixed seed for the random generator to ensure repeatability. For optimal performance and customization when applying the method to your own data, it is essential to understand the algorithm implementation described in the associated paper. Use the provided demonstration as a guideline and adjust the parameters in the code to tune the model for your specific application.

The main parameters you need to adjust are the following:

- Number of SNN training iterations: Monitor the SNN training convergence plot and specify enough iterations for your application. If the convergence plot plateaus before reaching the desired accuracy, repeat the optimization process using different initial conditions.

 - Number of models for Tournament ranking - Backwards elimination: The demo value (4000) is based on the number of features in the dataset (15). Increase this number if you have more than 15 features.

- Teacher MST tuning: Based on the complexity of your dataset, the MST can be tuned as follows:

	1. Adding more stages

	2. Adjusting the number of neurons in each stage

	3. Adjusting the number of epochs in each stage

	4. Adjusting the maximum number of validation fails in each stage

For more information on the MST algorithm, please refer to the MST papers included in our paper references.


5. Inference

a. Instructions

The second part of the demo script contains demonstrates how to use the SNN model for inference and how to plot the additive functions of the individual features. 

	1. A window for loading the trained SNN model will appear. Select the saved SNN model.

	2. A window for loading the dataset will appear. Select the included dataset "GSNN_Demo_Data.mat" for this demonstration.

b. Single sample inference

The first example demonstrates how to calculate susceptibility for an individual location using the SNN model.

c. Batch inference

The second example demonstrates how to calculate susceptibility for the entire dataset in a vectorized manner using the SNN model.

d. Visualizing Individual feature functions

The third example demonstrates how to calculate the additive functions of the individual features for the entire dataset in a vectorized manner and plots the functions. Note that adding these functions is equal to the susceptibility and will produce the same result as the previous example. 


6. Support

For questions and comments, please email: [land.slide.snn@gmail.com](mailto:land.slide.snn@gmail.com)


7. Citation

PLEASE ACKNOWLEDGE THE EFFORT THAT WENT INTO DEVELOPMENT BY REFERENCING THE PAPER:

K. Youssef, K. Shao, S. Moon & L.-S. Bouchard Landslide susceptibility modeling by interpretable neural network. Communications Earth & Environment: https://doi.org/10.1038/s43247-023-00806-5 
