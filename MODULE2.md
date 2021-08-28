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

**Example** Computing W_matrix for a multi-environment maize data
```{r, eval=FALSE}
data("maizeWTH") # toy set of environmental data
var = c("SVP", "T2MDEW", "T2M_MAX", "T2M_MIN") # variables
W = W_matrix(env.data = maizeWTH[maizeWTH$daysFromStart < 100,],
             var.id=var, statistic="mean", by.interval=TRUE)
dim(W)
```

**Example** W_matrix for single or multiple covariables

```{r, eval=FALSE}
data("maizeYield") # toy set of phenotype data (grain yield per environment)
data("maizeG"    ) # toy set of genomic relationship for additive effects 
data("maizeWTH")   # toy set of environmental data

stages    = c('VE','V1_V6','V6_VT','VT_R1','R1_R3','R3_R6',"H")
interval  = c(0,7,30,65,70,84,105) # in days after emergence
EC1  = W_matrix(env.data = maizeWTH, var.id = 'FRUE')
EC2  = W_matrix(env.data = maizeWTH, var.id = 'SVP')
EC3  = W_matrix(env.data = maizeWTH, var.id = c('FRUE','SVP'))
EC4  = W_matrix(env.data = maizeWTH, var.id = 'FRUE',
                by.interval = T,time.window = interval,names.window = stages)
EC5  = W_matrix(env.data = maizeWTH, var.id = 'T2MDEW',
                by.interval = T,time.window = interval,names.window = stages)
EC6  = W_matrix(env.data = maizeWTH, var.id = c('FRUE','T2MDEW'),
                by.interval = T,time.window = interval,names.window = stages)
                
EC7  = W_matrix(env.data = maizeWTH, var.id = c('"T2M","T2M_MAX","T2M_MIN",
                        "WS2M","RH2M","T2MDEW","ALLSKY_SFC_LW_DWN",
                        "ALLSKY_SFC_SW_DWN"),
                by.interval = T,time.window = interval,names.window = stages)
                
```
                
**hands-on: now using thermal time (GDD) to defone the time window of each development stage**

## env_typing: environmental typologies and its frequencies across time and space

An environment can be viewed as the status of multiple resource inputs (e.g., water, radiation, nutrients) across a certain time interval (e.g., from sowing to harvesting) within a specific space or location. The quality of those environments is an end-result of the daily balance of resource availability, which can be described as a function of how many resources are available and how frequently those resources occur (e.g., transitory or constant effects). Also, the relationship between resource absorption and allocation depends on plant characteristics (e.g., phenology, current health status). Then, this particular environmental-plant influence is named after the envirotype to differentiate it from the concept of raw environmental data (data collected directly from sensors). It can be referred to as environmental type (ET). Finally, the typing of environments can be done by discovering ETs; the similarity among environments is a consequence of the number of ETs shared between environments.
Before the computation of ETs, a first step was to develop a design based on ecophysiological concepts (e.g., plants' needs for some resource) or summarize the raw data from the core environments being analyzed. Then, for each ET we computed the frequency of occurrence, which represents the frequency of specific quantities of resources available for plant development. Typing by frequency of occurrence provides a deeper understanding of the distribution of events, such as rainfall distribution across different growing cycles and the occurrence of heat stress conditions in a target location (Heinemann et al. 2015). Thus, groups of environments can be better identified by analyzing the events occurring in a target location, year, or planting date. This step can be done not only by using grade point averages (e.g., accumulated sums or means for specific periods), but also by their historical similarity. In this way, we are able to not only group environments in the same year, but also through a historical series of years. Finally, this analysis deepens in resolution when the same environment is divided by time intervals, which can be fixed (e.g., 10-day intervals), or categorized by specific phenological stages of a specific crop.
To implement envirotype profiling, we created the env_typing() function. This function computes the frequency of occurrence of each envirotype across diverse environments. This function has 12 arguments, nine of which (env.data, id.names, env.id, days.id var.id, statistic, by.interval, time.window, and names.window) work in the same way as already described in the previous functions. The argument cardinals are responsible for defining the biological thresholds between envirotypes and adaptation zones. These cardinals must respect the ecophysiological limits of each crop, germplasm, or region. For that, we suggest literature on ecophysiology and crop growth modeling, such as Soltani and Sinclar (2012). The argument cardinals can be filled out as vectors (for single-environmental factors) or as a list of vectors for each environmental factor considered in the analysis. 

**Example** Basic use of env_typing for typing temperature in Los Baños, Philipines, from 2000 to 2020

```{r, eval=FALSE}
env.data = get_weather(env.id = 'LOSBANOS',country = 'PHL',
                       lat = 14.170,lon = 121.241,variables.names = 'T2M',
                       start.day = '2000-03-01',end.day = '2020-03-01')

card = list(T2M=c(0,8,15,28,40,45,Inf)) # a list of vectors containing empirical and cardinal thresholds
env_typing(env.data = env.data,env.id = 'env', var.id = 'T2M', cardinals = card)
```

**hands-on: run the same analysis involving Nairobi (Kenya) and other city of your preference**

**hands-on: run the same analysis using FRUE variable**

**hands-on: generate environments within LOS BANOS locations in different years (at least 5 environments). Then repeat the analysis**

**hands-on: how can you plot this outputs? think about it**

**Example** Basic use of env_typing for more than one variable

```{r, eval=FALSE}
var = c("PRECTOT", "T2MDEW") # variables
env.data = get_weather(env.id = 'LOSBANOS',country = 'PHL',
                       lat = 14.170,lon = 121.241,variables.names = var,
                       start.day = '2000-03-01',end.day = '2020-03-01')
card = list(PRECTOT = c(0,5,10,25,40,100), T2MDEW = NULL) # cardinals and data-driven limits
env_typing(env.data = env.data,env.id = 'env', var.id = var, cardinals = card)
```

**Hands-on: master challenge**

Collect VPD data, and then process the raw-data for maize in the following places and time windows: Goiânia (Brazil, 16.67 S, 49.25 W, from March 15th, 2020 to April 04th, 2020); Texcoco (Mexico, 19.25 N, 99.50 W, from May 15th, 2019 to June 15th, 2019); Brisbane (Australia, 27.47 S, 153.02 E, from September 15th, 2018 to October 04th, 2018); Montpellier (France, 43.61 N, 3.81 E, from June 18th, 2017 to July 18th, 2017); Los Baños (the Philippines, 14.170 N, 121.431 E, from May 18th, 2017 to June 18th, 2017); Porto-Novo (Benin, 6.294 N, 2.361 E, from July 18th, 2016 to August 18th, 2016), Cali (Colombia, 3.261 N, 76.312 W, from November 18th, 2017 to December 18th, 2017); Palmas (Brazil, 10.168 S, 48.331 W, from December 18th, 2017 to January 18th, 2018). Finally, run the typologies and plot it. Then, repeat everything using now the quantitative descriptors (W matrix).


## env_association: bilinear models (GE, G+GE,E+GE,E+G+GE) for MET-specific reaction-norm

Currently under development (for EnvRtype v 1.0.1, November 2021). Aims to associate the W matrix with phenotypic data, using factorial regression (using lm or glm), partial least squares (using plsr or plsdepot) or GAM (using gam).

## env_drivers: environmental drivers of the phenotypic correlations across METs

Currently under development (for EnvRtype v 1.0.1, November 2021).

## env_group: grouping environments according to its diversity

Currently under development (for EnvRtype v 1.0.1, November 2021). Aims to use T matrix (typologies) or W matrix for grouping environments with shared similarity

