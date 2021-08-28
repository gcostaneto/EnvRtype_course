# MODULE 3 - Enviromic-aided Phenotype Prediction 

EnvRtype version 1.0.0 (August 2021)
Author: G.Costa-Neto

## Overview

All information from Module 1 and 2 can be used to create environmental similarity and integrate robust GP platforms for multiple environments, hereafter referred to as envirotype-informed GPs. Module 3 aims to provide tools to compute environmental similarity using correlations or Euclidean distances across different trials conducted on the environmental covariables (ECs). Thus, we developed a function to integrate this enviromic source in GP as an additional source of variation to bridge the gap between genomic and phenotypic variation. For that, we provide at least four different structures into a flexible platform to integrate multiple genomic and enviromic kinships.

Three main functions were designed for this purpose. First, the function env_kernel can be used for the construction of environmental relationship kernels using environmental information. Second, the **get_kernel** aims to integrate these kernels into statistical models accounting for different structures capable of explaining the phenotypic variation across MET. Finally, the function **kernel_model** can be used to fit regression models accounting for environmental and or genomic data using a computationally efficient Bayesian approach -- but you can do it at any package for GP! Ok, ok, I know that you may prefer to use ASReml, BGLR or any other widely used package -- so in the next release we will share some codes (in fact, suggestions of codes) to use enviromics on these packages. But, between us, you can trust in [BGGE](https://academic.oup.com/g3journal/article/8/9/3039/6027023)! (from where we adapted the functions for kernel_model). In fact, the use of [BGLR](https://github.com/gdlc/BGLR-R) or ASReml might be interesting for, respectively, modeling different bayesian priors (and models) and for using factor analytic structures with reaction-norms (just as done by [Rogers et al. 2021](https://academic.oup.com/g3journal/article/11/2/jkaa050/6062399)). Perhaps the use of enviromic kernels might also be useful with genomics and machine learning approaches!

## env_kernel: building environmental relatedness kernels

This is the famous omega matrix (Jarquin et al 2014; Morais-Junior et al 2018 and others) also known as enviromic kinship (K_E or K_W, Costa-Neto et al., 2021a;b -- okay, it was us to suggested this name, but you have to agree that is fancy). In fact, this is a *environmental relatedness matrix* (or environmental similarity? or environmental distance?). To the best of our knowledge, there is three ways to parametrize it from the environmental covariables (**W** or **T** matrix): (1) linear way, inspired in GBLUP kernel approach (WW'/ and its normalizations), proposed by [Jarquin et al (2014)](https://link.springer.com/article/10.1007%2Fs00122-013-2243-1); (2) nonlinear gaussian kernel (GK, e.g., Gianola and van Kaam (2008); de los Campos et al., 2010; Cuevas et al., 2017), accouting for *environmental distances*, proposed by [Costa-Neto et al (2021)](https://www.nature.com/articles/s41437-020-00353-1), in which we also proposed the third way, (3) nonlinear arc-cosine kernel (inspired by [Cuevas et al 2019](https://www.g3journal.org/content/9/9/2913) and [Crossa et al (2019)](https://www.frontiersin.org/articles/10.3389/fgene.2019.01168/full), also named after *Deep Kernel*. Currently, EnvRtype::get_kernel() implements the linear way (using gaussian = F) and the nonlinear GK (using gaussian = T). If you want to know more about DK, please check this [link](https://github.com/gcostaneto/KernelMethods). For more detail about EnvRtype::get_kernel() please check the [G3 paper of the package](https://academic.oup.com/g3journal/article/11/4/jkab040/6129777).
The env_kernel function has two outputs called varCov (relatedness among covariables) and envCov (relatedness among environments). The first is useful to deepen the understanding of the relatedness and redundancy of the ECs. The second output is K_E. This matrix is the enviromic similarity kernel integrated into the GP models.

The get_kernel function has four main arguments: a list of genomic relationship kernels (K_G); a list of environmental relationship kernels (K_E); and a phenotypic MET data set (data), composed of a vector of environment identification (env), a vector of genotype identification (gid), and a vector of trait values (y); at last, the model argument sets the statistical model used (‘MM,' ‘MDs’, ‘EMM’, ‘EMDs’, ‘RNMM’ and ‘RNMDs’). The assumptions of genetics (G), environment (E) and GE effects for each one of those models is given below:

<img align="center" src="https://github.com/allogamous/EnvRtype/blob/master/fig/models.png" width="90%" height="90%">


Each genomic kernel in K_G must have the dimension of p × p genotypes. This argument assumes K_G = NULL by default. If no structure for genetic effects is provided, the get_kernel function automatically assumes an identity matrix for genotype effects, in which it considers no relatedness among individuals. Finally, the argument stage (stage = NULL by default) states which development stages can be used to create stage-specific enviromic kernels. More detail about the latter is given in the Example 3 of the Results section.
In the same manner, K_E could have the dimension of q × q environments, but the environmental kernels can be built at the phenotypic observation level in some cases. It means that for each genotype in each environment, there is a different EC, according to particular phenology stages or envirotyping at the plant level. Thus, using the additional argument dimension_KE = c(‘q’, ‘n’) (default is ‘q’, for environment), the K_E may accomplish a kernel with n × n, in which n = pq. 

**Example: implementing different models**

```{r, eval=FALSE}
data("maizeYield") # toy set of phenotype data (grain yield per environment)
data("maizeG"    ) # toy set of genomic relationship for additive effects 
data("maizeWTH")   # toy set of environmental data
y   = "value"      # name of the vector of phenotypes
gid = "gid"        # name of the vector of genotypes
env = "env"        # name of the vector of environments

ECs  = W_matrix(env.data = maizeWTH, var.id = c("FRUE",'SVP',"n","T2M_MAX"),statistic = 'mean')
## KG and KE might be a list of kernels
KE = list(W = env_kernel(env.data = ECs)[[2]])
KG = list(G=maizeG);
## Creating kernel models with get_kernel
MM    = get_kernel(K_G = KG, y = y,gid = gid,env = env, data = maizeYield,model = "MM") 
MDs   = get_kernel(K_G = KG, y = y,gid = gid,env = env,  data = maizeYield, model = "MDs") 
EMM   = get_kernel(K_G = KG, K_E = KE, y = y,gid = gid,env = env,  data = maizeYield, model = "EMM") 
EMDs  = get_kernel(K_G = KG, K_E = KE, y = y,gid = gid,env = env,  data = maizeYield, model = "EMDs") 
RMMM  = get_kernel(K_G = KG, K_E = KE, y = y,gid = gid,env = env,  data = maizeYield, model = "RNMM") 
RNMDs = get_kernel(K_G = KG, K_E = KE, y = y,gid = gid,env = env,  data = maizeYield, model = "RNMDs") 
```
**hands-on question** what really changes between each model?

## kernel_model: running a hierarchical bayesian approach for phenotype prediction

Details about the algoritm used in the EnvRtype::kernel_model() is given in [Granato et al 2018](https://academic.oup.com/g3journal/article/8/9/3039/6027023) and [Costa-Neto et al (2021)](https://www.nature.com/articles/s41437-020-00353-1). The function has four main arguments: a phenotypic MET data set (data), composed of a vector of environment identification (env), a vector of genotype identification (gid), and a vector of trait values (y), a list object for random effects (random, from get_kernel) and a matrix for fixed effects (fixed). For the Hierarchical Bayesian Modeling (see Appendix 1), the arguments for number of iterations (iterations, default is 1000) and the number of samples used for burn-in (burnin, default is 200) and thining (thining, default is 10) must be provided. The function has two main outputs: the predicted phenotypes (yHat), variance components for each random effect (VarComp) -- with its confidence intervals.

**Step 1: create your W_matrix (or env_typing)**

```{r, eval=FALSE}
data("maizeYield") # toy set of phenotype data (grain yield per environment)
data("maizeG"    ) # toy set of genomic relationship for additive effects 
data("maizeWTH")   # toy set of environmental data
y   = "value"      # name of the vector of phenotypes
gid = "gid"        # name of the vector of genotypes
env = "env"        # name of the vector of environments

### 1- Environmental Covariables (ECs)
stages    = c('VE','V1_V6','V6_VT','VT_R1','R1_R3','R3_R6',"H")
interval  = c(0,7,30,65,70,84,105)
EC1  = W_matrix(env.data = maizeWTH, var.id = 'FRUE')
EC2  = W_matrix(env.data = maizeWTH, var.id = 'SPV')
EC3  = W_matrix(env.data = maizeWTH, var.id = c('FRUE','SPV'))
EC4  = W_matrix(env.data = maizeWTH, var.id = 'FRUE',
                by.interval = T,time.window = interval,names.window = stages)
EC5  = W_matrix(env.data = maizeWTH, var.id = 'SPV',
                by.interval = T,time.window = interval,names.window = stages)
EC6  = W_matrix(env.data = maizeWTH, var.id = c('FRUE','SPV'),
                by.interval = T,time.window = interval,names.window = stages)

```

**Step 2: prepare the kernels (genomics and enviromics)**

```{r, eval=FALSE}

# create the environmental relatedness
K1 = list(FRUE = env_kernel(env.data = EC1)[[2]])
K2 = list(PETP = env_kernel(env.data = EC2)[[2]])
K3 = list(FRUE_PETP = env_kernel(env.data = EC3)[[2]])
K4 = list(FRUE = env_kernel(env.data = EC4)[[2]])
K5 = list(PETP = env_kernel(env.data = EC5)[[2]])
K6 = list(FRUE_PETP = env_kernel(env.data = EC6)[[2]])

# and then adjust the kernels for running kernel_models
M0  = get_kernel(K_G = KG,  y = y,gid = gid,env = env,  data = maizeYield, model = "MDs") 
M1  = get_kernel(K_G = KG, K_E = K1, y = y,gid = gid,env = env,  data = maizeYield, model = "RNMDs") 
M2  = get_kernel(K_G = KG, K_E = K2, y = y,gid = gid,env = env,  data = maizeYield, model = "RNMDs") 
M3  = get_kernel(K_G = KG, K_E = K3, y = y,gid = gid,env = env,  data = maizeYield, model = "RNMDs") 
M4  = get_kernel(K_G = KG, K_E = K4, y = y,gid = gid,env = env,  data = maizeYield, model = "RNMDs") 
M5  = get_kernel(K_G = KG, K_E = K5, y = y,gid = gid,env = env,  data = maizeYield, model = "RNMDs") 
M6  = get_kernel(K_G = KG, K_E = K6, y = y,gid = gid,env = env,  data = maizeYield, model = "RNMDs") 

```



**Step 3: run your kernel_model**

```{r, eval=FALSE}
fixed = model.matrix(~0+env,maizeWTH)

iter = 1000
burn = 500
seed = 78172
thin = 10
model = paste0('M',0:6)
Models = list(M0,M1,M2,M3,M4,M5,M6)
Vcomp <- c()
for(MODEL in 1:length(Models)){
  set.seed(seed)
  fit <- kernel_model(data = maizeYield,y = y,env = env,gid = gid,
                      random = Models[[MODEL]],fixed = Z_E,
                      iterations = iter,burnin = burn,thining = thin)
  
  Vcomp <- rbind(Vcomp,data.frame(fit$VarComp,Model=model[MODEL]))
}

Vcomp

```

**hands-on: now use gaussian kernel and check the results**

**hands-on: try the following models - MM, EMM, RNMM**

**hands-on: try a different seed. Why the results are different?**

**hands-on: try a larger number of iterations**

**hands-on: implement a cross-validation scheme manually. What is your hypothesis?**

