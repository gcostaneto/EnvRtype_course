# MODULE 3 - Enviromic-aided Phenotype Prediction 

EnvRtype version 1.0.0 (August 2021)
Author: G.Costa-Neto

## Overview

All information from Module 1 and 2 can be used to create environmental similarity and integrate robust GP platforms for multiple environments, hereafter referred to as envirotype-informed GPs. Module 3 aims to provide tools to compute environmental similarity using correlations or Euclidean distances across different trials conducted on ECs. Thus, we developed a function to integrate this enviromic source in GP as an additional source of variation to bridge the gap between genomic and phenotypic variation. For that, we provide at least four different structures into a flexible platform to integrate multiple genomic and enviromic kinships.

Three main functions were designed for this purpose. First, the function env_kernel can be used for the construction of environmental relationship kernels using environmental information. Second, the **get_kernel** aims to integrate these kernels into statistical models accounting for different structures capable of explaining the phenotypic variation across MET. Finally, the function **kernel_model** can be used to fit regression models accounting for environmental and or genomic data using a computationally efficient Bayesian approach -- but you can do it at any package for GP! Ok, ok, I know that you may prefer to use ASReml or BGLR -- so in the next release we will share some codes (in fact, suggestions of codes) to use enviromics on these packages.


