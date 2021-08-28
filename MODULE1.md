# MODULE 1 - Remote Environmental Sensing (and its implications)

## Overview

Module 1 starts by collecting raw environmental data from public platforms, such as a satellite-based weather system named ‘NASA’s Prediction of Worldwide Energy Resources’ (NASA POWER, https://power.larc.nasa.gov/), which can access information daily anywhere on earth. This database is well consolidated and validated for use in several research fields, including crop modeling in agricultural research (White et al., 2011; Monteiro et al., 2018; Aboelkhair et al., 2019). Details about resolution and validation of this data source are given in https://power.larc.nasa.gov/docs/methodology/validation/. 
Data collection may span existing experimental trials (single sampling trials) or historical trends for a given location × planting date arrangement. This module gathers the functions for remote data collection of daily weather and elevation data, as well as the computation of ecophysiological variables, such as the effect of air temperature on radiation use efficiency. The module includes a toolbox with ‘Remote Data Collection’ and ‘Data Processing’ steps, both designed to help researchers find a viable alternative for expensive in-field environmental sensing equipment. 


## get_weather : collects weather and elevation data worldwide (1981-present)

EnvRtype implements the remote collection of daily weather and elevation data by the get_weather function. This function has the following arguments: the environment name (env.id); geographic coordinates (latitude, lat; longitude, lon) in WGS84; time interval (start.day and end.day, given in ‘year-month-day’); and country identification (country), which sets the raster file of elevation for the region of a specific country. Countries are specified by their 3-letter ISO codes:

<img align="center" src="https://github.com/allogamous/EnvRtype/blob/master/fig/ISO3.png" width="60%" height="60%">

All weather information is given on a daily basis. Altitude (ALT) information is provided by SRTM 90 m resolution and can be collected from any place between -60 and 60 latitudes. This information is presented as a data.frame class output in R. It is possible to download data for several environments by country.

<img align="center" src="https://github.com/allogamous/EnvRtype/blob/master/fig/variables.png" width="90%" height="90%">


## extract_GIS : point estimates from a raster file (GIS-based raster files)

The function extract_GIS can be useful for collecting covariables from raster files within databases such as WorldClim (Fick et al., 2017; https://www.worldclim.org/), SoilGrids (https://soilgrids.org/), ClimateSERV ( https://climateserv.servirglobal.net/), EarthMaps (https://earthmap.org/) and Nasa Power (https://power.larc.nasa.gov). 


## param_temperature: computing growing degree-days and generic temperature-stress factors

EnvRtype provides the param_temperature function, which computes additional thermal-related parameters, such as GDD and FRUE, and T2M_RANGE. As previously described, the first is useful to predict phenological development, while the second is an ecophysiology parameter used to quantify the impact of temperature on crop growth and biomass accumulation in crop models (Soltani and Sinclar, 2012). Thus, both can be useful to relate how temperature variations shape some species' adaptation in the target environment. GDD is also important for modeling plant-pathogen interactions because some pests and diseases have temperature-regulated growth. Finally, the daily temperature range (T2M_RANGE) impacts processes such as floral abortion in crops where the main traits are related to grain production. For more details about the impact of temperature on diverse crops, please check Luo (2011).
Thus, the param_temperature() function has eight arguments (env.data, Tmax, Tmin, Tbase1, Tbase2, Topt1, Topt2 and merge). To run this function with data sources other than get_weather(), it is necessary to indicate which columns denote maximum air temperature (Tmax, default is Tmax = ‘T2M_MAX’) and minimum air temperature (Tmin, default is Tmin = ‘T2M_MIN’) (BOX 6). The cardinal temperatures must follow the processes provided in the previously described ecophysiology literature (Soltani and Sinclar, 2012). ![image](https://user-images.githubusercontent.com/25282742/131204720-e615ac38-147a-4f97-9131-c9bdb618e6a6.png)


## param_
