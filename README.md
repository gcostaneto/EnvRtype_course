# Overview

Envirotyping is a technique used to unfold the non-genetic drivers associated with the phenotypic adaptation of living organisms. At [Costa-Neto et al. (2021)](https://github.com/gcostaneto/GEMS_R/blob/main/References/Conceptual%20papers/Costa-Neto%20et%20al%202020%20EnvRtype%20a%20software%20to%20interplay%20enviromics%20and%20quantitative%20genomics%20in%20agriculture.pdf), we introduce the [EnvRtype R package](https://github.com/allogamous/EnvRtype), a novel toolkit developed to interplay large-scale envirotyping data (enviromics) into quantitative genomics. To start a user-friendly envirotyping pipeline, this package offers: (1) remote sensing tools for collecting (get_weather and extract_GIS functions) and processing ecophysiological variables (param_temperature, param_radiation and param_atmospheric functions) from raw environmental data at single locations or worldwide; (2) environmental characterization by typing environments and profiling descriptors of environmental quality (env_typing function), in addition to gathering environmental covariables as quantitative descriptors for predictive purposes (W_matrix function); and (3) identification of environmental similarity that can be used as an enviromic-based kernel (env_typing function) in whole-genome prediction (GP), aimed at increasing ecophysiological knowledge in genomic best-unbiased predictions (GBLUP) and emulating reaction norm effects (get_kernel and kernel_model functions). In this course, I expect to highlight the importance of literature mining concepts in fine-tuning envirotyping parameters for each plant species and target growing environments. Then, exemplify its use for creating global-scale envirotyping networks and integrating reaction-norm modeling in GP are also outlined.

# Software

Installation can be done using the install_git function from the devtools package, such as:

```{r, eval=FALSE}
library(devtools)
install_github('allogamous/EnvRtype')
library(EnvRtype)
```

One way to use the package is through source() within your R environment, such as:

```{r, eval=FALSE}
source("https://raw.githubusercontent.com/allogamous/EnvRtype/master/sourceEnvRtype.R")
```

or specific functions

```{r, eval=FALSE}

source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/AtmosphericPAram.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/SradPARAM.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/SupportFUnction.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/EnvTyping.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/Wmatrix.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/covfromraster.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/envKernel.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/gdd.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/getGEenriched.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/get_weather_gis.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/processWTH.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/met_kernel_model.R')
source('https://raw.githubusercontent.com/allogamous/EnvRtype/master/R/summary_weather.R')
```
ok, it is ugly, but solves it.


**Some useful packages for our hands-on**

```{r, eval=FALSE}
source("https://raw.githubusercontent.com/gcostaneto/Funcoes_naive/master/instpackage.R")
inst.package(c("superheat","FactoMineR","tidyverse","ggplot2","reshape2","plyr"))
```

# Modules (and presentation)

> [Presentation](https://github.com/gcostaneto/EnvRtype_course/blob/main/EnvRtype_lecture.pdf) 

> [MODULE 1](https://github.com/gcostaneto/EnvRtype_course/blob/main/MODULE1.md) - Remote Environmental Sensing

> [MODULE 2](https://github.com/gcostaneto/EnvRtype_course/blob/main/MODULE2.md) - Macro-Environmental Characterization

> [MODULE 3](https://github.com/gcostaneto/EnvRtype_course/blob/main/MODULE3.md) - Enviromic-aided Phenotype Prediction (under genomic prediction context) 
