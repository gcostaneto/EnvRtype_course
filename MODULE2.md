# MODULE 2 - Macro-Environmental Characterization

EnvRtype version 1.0.0 (August 2021)
Author: G.Costa-Neto

## Overview

The processed environmental information can then be used for many purposes. In Module 2, we designed tools for the characterization of the macro-environmental variations, which can also be done across different time intervals of crop growth and development (when associated with a crop) or fixed time intervals (to characterize locations). The environmental characterization toolbox involves two types of environmental profiling:

  1)	Discovering environmental types (envirotypes, hereafter abbreviated as ETs) and how frequently they occur at each growing environment (location, planting date, year). Based on the ET-discovering step, it is possible to create environmental profiles and group environments with the same ET pattern. This step is also useful for running exploratory analysis, e.g., to discover the main ET of planting dates at a target location.

  2)	Gathering environmental covariables (hereafter abbreviated as ECs) from point-estimates (e.g., mean air temperature, cumulative rainfall). These ECs can be used for many purposes, from a basic interpretation of G×E to estimating gene-environment interactions. At the end of this process, a matrix of ECs (W) is created and integrated with tools from Module 3.

## W_matrix: environmental covariables by environment (or genotype-environment entries)



## env_typing: environmental typologies and its frequencies across time and space


## env_association: bilinear models (GE, G+GE,E+GE,E+G+GE) for MET-specific reaction-norm

Currently under development (for EnvRtype v 1.0.1, November 2021). Aims to associate the W matrix with phenotypic data, using factorial regression, partial least squares or GAM.

## env_drivers: environmental drivers of the phenotypic correlations across METs

Currently under development (for EnvRtype v 1.0.1, November 2021).

## env_group: grouping environments according to its diversity

Currently under development (for EnvRtype v 1.0.1, November 2021). Aims to use T matrix (typologies) or W matrix for grouping environments with shared similarity

