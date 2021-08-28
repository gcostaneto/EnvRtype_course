# MODULE 2 - Macro-Environmental Characterization

EnvRtype version 1.0.0 (August 2021)
Author: G.Costa-Neto

## Overview

The quality of an environment is measured by the amount of resources available to fulfill the plants' demands. In an experimental network composed of multi-environment trials (MET), the environment's quality is relative to the global environmental gradient. Finlay and Wilkinson (1963) proposed using phenotypic data as a quality index over an implicit environmental gradient. However, this implicit environmental quality index was proposed as an alternative to explicit environmental factors, given the difficulties in obtaining high-quality envirotyping data. Thus, the processed environmental information can then be used for many purposes. The MODULE 2 provides tools for the characterization of the macro-environmental variations, which can also be done across different time intervals of crop growth and development (when associated with a crop) or fixed time intervals (to characterize locations).  Then, the environmental characterization toolbox involves two types of environmental profiling:

  1)	Discovering environmental types (envirotypes, hereafter abbreviated as ETs) and how frequently they occur at each growing environment (location, planting date, year). Based on the ET-discovering step, it is possible to create environmental profiles and group environments with the same ET pattern. This step is also useful for running exploratory analysis, e.g., to discover the main ET of planting dates at a target location.

  2)	Gathering environmental covariables (hereafter abbreviated as ECs) from point-estimates (e.g., mean air temperature, cumulative rainfall). These ECs can be used for many purposes, from a basic interpretation of G×E to estimating gene-environment interactions. At the end of this process, a matrix of ECs (W) is created and integrated with tools from Module 3.

## W_matrix: environmental covariables by environment (or genotype-environment entries)

Aims to provide a quantitative descriptor matrix of the environment -- that is, a matrix of quantitative environmental covariables (W), following the terminology used by Jarquin et al (2014), Costa-Neto et al. (2021a), de los Campos et al. (2020),Crespo-Herrera et al. (2021) and others. Based on these W matrices, several analyses can be performed, such as (1) dissecting the G×E interaction; (2) modeling genotype-specific sensibility to critical environmental factors; (3) dissecting the environmental factors of QTL×E interaction; (4) integrating environmental data to model the gene × environment reaction-norm; (5) providing a basic summary of the environmental gradient in an experimental network; (6) producing environmental relationship matrices for genomic prediction.
To implement these applications, the processed environmental data must be translated into quantitative descriptors by summarizing cumulative means, sums, or quantiles, such as in summaryWTH(). However, these data must be mean-centered and scaled to assume a normal distribution and avoid variations due to differences in scale dimensions. To create environmental similarity kernels, Costa-Neto et al. (2021a) suggested using quantile statistics to better describe each variable's distribution across the experimental network. Thus, this allows a statistical approximation of the environmental variables' ecophysiological importance during crop growth and development. In this context, we developed the W_matrix() function to create a double-entry table (environments/sites/years environmental factors). Contrary to env_typing(), the W_matrix() function was designed to sample each environmental factor's quantitative values across different environments. 
The same arguments for the functions summaryWTH() and env_typing() are applicable (env.data, id.names, env.id, days.id var.id, statistic, by.interval, time.window, and names.window). However, in W_matrix(), arguments center = TRUE (by default) and scale = TRUE (by default) denote mean-centered (w-w ̅) and scaled ((w-w ̅)⁄σ), in which w is the original variable, w ̅ and σ are the mean and standard deviation of this covariable across the environments. Quality control (QC = TRUE argument) is done by removing covariables with more than  σ_TOL±  σ, where σ_TOL is the tolerance limit for standard deviation, settled by default argument as sd.tol = 3.


## env_typing: environmental typologies and its frequencies across time and space


## env_association: bilinear models (GE, G+GE,E+GE,E+G+GE) for MET-specific reaction-norm

Currently under development (for EnvRtype v 1.0.1, November 2021). Aims to associate the W matrix with phenotypic data, using factorial regression (using lm or glm), partial least squares (using plsr or plsdepot) or GAM (using gam).

## env_drivers: environmental drivers of the phenotypic correlations across METs

Currently under development (for EnvRtype v 1.0.1, November 2021).

## env_group: grouping environments according to its diversity

Currently under development (for EnvRtype v 1.0.1, November 2021). Aims to use T matrix (typologies) or W matrix for grouping environments with shared similarity

