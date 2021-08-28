# MODULE 1 - Remote Environmental Sensing (and its implications)

EnvRtype version 1.0.0 (August 2021)
Author: G.Costa-Neto

## Overview

Module 1 starts by collecting raw environmental data from public platforms, such as a satellite-based weather system named ‘NASA’s Prediction of Worldwide Energy Resources’ (NASA POWER, https://power.larc.nasa.gov/), which can access information daily anywhere on earth. This database is well consolidated and validated for use in several research fields, including crop modeling in agricultural research (White et al., 2011; Monteiro et al., 2018; Aboelkhair et al., 2019). Details about resolution and validation of this data source are given in https://power.larc.nasa.gov/docs/methodology/validation/. 
Data collection may span existing experimental trials (single sampling trials) or historical trends for a given location × planting date arrangement. This module gathers the functions for remote data collection of daily weather and elevation data, as well as the computation of ecophysiological variables, such as the effect of air temperature on radiation use efficiency. The module includes a toolbox with ‘Remote Data Collection’ and ‘Data Processing’ steps, both designed to help researchers find a viable alternative for expensive in-field environmental sensing equipment. 


## get_weather : collects weather and elevation data worldwide (1981-present)

EnvRtype implements the remote collection of daily weather and elevation data by the get_weather function. This function has the following arguments: the environment name (env.id); geographic coordinates (latitude, lat; longitude, lon) in WGS84; time interval (start.day and end.day, given in ‘year-month-day’); and country identification (country), which sets the raster file of elevation for the region of a specific country. Countries are specified by their 3-letter ISO codes:

<img align="center" src="https://github.com/allogamous/EnvRtype/blob/master/fig/ISO3.png" width="60%" height="60%">

All weather information is given on a daily basis. Altitude (ALT) information is provided by SRTM 90 m resolution and can be collected from any place between -60 and 60 latitudes. This information is presented as a data.frame class output in R. It is possible to download data for several environments by country. Below is the table of the variables collected from get_weather.

<img align="center" src="https://github.com/allogamous/EnvRtype/blob/master/fig/variables.png" width="90%" height="90%">

Details about the computation of the variables can be found at: [Costa-Neto et al. (2021)](https://github.com/gcostaneto/GEMS_R/blob/main/References/Conceptual%20papers/Costa-Neto%20et%20al%202020%20EnvRtype%20a%20software%20to%20interplay%20enviromics%20and%20quantitative%20genomics%20in%20agriculture.pdf) Soltani and Sinclair (2012) and [FAO-boletin](http://www.fao.org/3/x0490e/x0490e04.htm)

## get_soil: collection of soil data from SoilGrids

Currently under development (for EnvRtype v 1.0.1). For now, please use SoilGrids (https://soilgrids.org/) followed by extract_GIS (example below)

## extract_GIS : point estimates from a raster file (GIS-based raster files)

The function extract_GIS can be useful for collecting covariables from raster files within databases such as WorldClim (Fick et al., 2017; https://www.worldclim.org/), SoilGrids (https://soilgrids.org/), ClimateSERV ( https://climateserv.servirglobal.net/), EarthMaps (https://earthmap.org/) and Nasa Power (https://power.larc.nasa.gov). 


## param_temperature: computing growing degree-days and generic temperature-stress factors

EnvRtype provides the param_temperature function, which computes additional thermal-related parameters, such as GDD and FRUE, and T2M_RANGE. As previously described, the first is useful to predict phenological development, while the second is an ecophysiology parameter used to quantify the impact of temperature on crop growth and biomass accumulation in crop models (Soltani and Sinclar, 2012). Thus, both can be useful to relate how temperature variations shape some species' adaptation in the target environment. GDD is also important for modeling plant-pathogen interactions because some pests and diseases have temperature-regulated growth. Finally, the daily temperature range (T2M_RANGE) impacts processes such as floral abortion in crops where the main traits are related to grain production. For more details about the impact of temperature on diverse crops, please check Luo (2011).
Thus, the param_temperature() function has eight arguments (env.data, Tmax, Tmin, Tbase1, Tbase2, Topt1, Topt2 and merge). To run this function with data sources other than get_weather(), it is necessary to indicate which columns denote maximum air temperature (Tmax, default is Tmax = ‘T2M_MAX’) and minimum air temperature (Tmin, default is Tmin = ‘T2M_MIN’) (BOX 6 of Costa-Neto et al. 2021). The cardinal temperatures must follow the processes provided in the previously described ecophysiology literature (Soltani and Sinclar, 2012).

## param_radiation: basic variables related to solar incidence and day length



## param_atmospheric: basic variables related to the atmospheric demands

We implemented the param_atmospheric() function to run basic computations of atmospheric demands. This function has 11 arguments: env.data; PREC (rainfall precipitation in mm, default is PREC=’PRECTOT’); Tdew (dew point temperature in °C, default is Tdew=’T2M_DEW’); Tmax (maximum air temperature in °C, default is Tmax=’T2M_MAX’); Tmin (minimum air temperature in °C, default is Tmin=’T2M_MIN’); RH (relative air humidity %, default is RH=’RH2M’); Rad (net radiation, in MJ m-2 day-1, default is Rad =’Srad’); alpha (empirical constant accounting for vapor deficit and canopy resistance values, default is alpha=1.26); Alt (altitude, in meters above sea level, default is Alt = ALT); G (soil heat flux in W m-2, default is G=0); and merge (default is merge=TRUE). 

