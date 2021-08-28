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

**Example** Nairobi, Kenya (latitude 1.367 N, longitude 36.834 E) from 01 march 2015 to 01 April 2015. 

Use decimal values for the coordinates (negative for west and south). Use year-month-day structure for date values (YYYY-MM-DD).

```{r, eval=FALSE}
require(EnvRtype)
env.data = get_weather(env.id = 'NAIROBI',country = 'KEN',
                       lat = -1.367,lon = 36.834,
                       start.day = '2015-03-01',end.day = '2015-04-01')

head(env.data)
```

**Hands-on 1: download the date for Nairobi in different years and time windows. Try to remove country = 'KEN' and see what happens**

**Hands-on 2: use different lat and lon also for Kenya. See what happens. **

## get_soil: collection of soil data from SoilGrids

Currently under development (for EnvRtype v 1.0.1). For now, please use SoilGrids (https://soilgrids.org/) followed by extract_GIS (example below).
I went into SoilGrids and download the clay information for the layer 5-15 cm, also for Nairobi. It is a raster file. 


## extract_GIS : point estimates from a raster file (GIS-based raster files)

The function extract_GIS can be useful for collecting covariables from raster files within databases such as WorldClim (Fick et al., 2017; https://www.worldclim.org/), SoilGrids (https://soilgrids.org/), ClimateSERV ( https://climateserv.servirglobal.net/), EarthMaps (https://earthmap.org/) and Nasa Power (https://power.larc.nasa.gov). 

```{r, eval=FALSE}
data("clay_5_15")
env.data = extract_GIS(covraster = clay_5_15,name.out = 'clay_5_15',env.data = env.data)
head(env.data)
```
If you want to name the clay variable into a different manner...
```{r, eval=FALSE}
env.data = extract_GIS(covraster = clay_5_15,name.out = 'clay_any_name_you_want',env.data = env.data)
head(env.data)
```
**Hands-on 3: go to SoilGrids and download other soil attributes. Save at your directory folder, import to R using yourRasterName = raster("NameOfTheAttribute.tiff")**

**Hands-on 4: go to ClimateSERV and download rainfall data for Kenya. Then do as the same you have done for clay data**

PLOS: using raster::getData to download bioclimatic variables and future climate data (CMIP5). Then, try to use extract_GIS for gather this data for Kenya.

```{r, eval=FALSE}
getData('worldclim', var='tmin', res=0.5, lon=36.834, lat=-1.367) # temperature min (average across years)
getData('worldclim', var='bio', res=10) # bioclimatic variables
getData('CMIP5', var='tmin', res=10, rcp=85, model='AC', year=70) # different scenarios (see https://www.worldclim.org/)
```

## param_temperature: computing growing degree-days and generic temperature-stress factors

EnvRtype provides the param_temperature function, which computes additional thermal-related parameters, such as GDD and FRUE, and T2M_RANGE. As previously described, the first is useful to predict phenological development, while the second is an ecophysiology parameter used to quantify the impact of temperature on crop growth and biomass accumulation in crop models (Soltani and Sinclar, 2012). Thus, both can be useful to relate how temperature variations shape some species' adaptation in the target environment. GDD is also important for modeling plant-pathogen interactions because some pests and diseases have temperature-regulated growth. Finally, the daily temperature range (T2M_RANGE) impacts processes such as floral abortion in crops where the main traits are related to grain production. For more details about the impact of temperature on diverse crops, please check Luo (2011).
Thus, the param_temperature() function has eight arguments (env.data, Tmax, Tmin, Tbase1, Tbase2, Topt1, Topt2 and merge). To run this function with data sources other than get_weather(), it is necessary to indicate which columns denote maximum air temperature (Tmax, default is Tmax = ‘T2M_MAX’) and minimum air temperature (Tmin, default is Tmin = ‘T2M_MIN’) (BOX 6 of Costa-Neto et al. 2021). The cardinal temperatures must follow the processes provided in the previously described ecophysiology literature (Soltani and Sinclar, 2012), such as:

<img align="center" src="https://github.com/allogamous/EnvRtype/blob/master/fig/cardinals.png" width="90%" height="90%">

**Example** Consider the estimations for dry beans at the same location in Nairobi, Kenya 

```{r, eval=FALSE}
TempData = param_temperature(env.data = env.data,Tbase1 = 8,Tbase2 = 45,Topt1 = 30,Topt2 = 35)
head(TempData)
env.data = param_temperature(env.data = env.data,Tbase1 = 8,Tbase2 = 45,Topt1 = 30,Topt2 = 35,merge = TRUE)
head(env.data) # merging TempData automatically
```

**Hands-on 5: consider maize and sugarcane. What changes?**

**Hands-on 6: plot the GDD and FRUE across the time interval. What conclusions can you make?**

## param_radiation: basic variables related to solar incidence and day length

The function called param_radiation() is available to compute additional radiation-based variables that can be useful for plant breeders and researchers in several fields of agricultural research (e.g., agrometeorology). These parameters include the actual duration of sunshine hours (n, in hours) and total daylength (N, in hours), both estimated according to the altitude and latitude of the site, time of year (Julian day, from 1 to 365), and cloudiness (for n). Solar radiation is important factor in most computations of crop evapotranspiration (Allen et al., 1998) and biomass production (Muchow et al., 1990; Muchow and Sinclair, 1991). More details about those equations are given in ecophysiology and evapotranspiration literature (abovementioned Allen et al., 1998; Soltani and Sinclair, 2012).
The arguments of param_radiation are: env.data and merge, in which merge denotes if the computed radiation parameters must be merged with the env.data set (merge = TRUE, by default).

**Example**

```{r, eval=FALSE}
RadData = param_radiation(env.data = env.data) 
head(RadData)
env.data = param_radiation(env.data = env.data,merge = TRUE)
```

## param_atmospheric: basic variables related to the atmospheric demands

We implemented the param_atmospheric() function to run basic computations of atmospheric demands. This function has 11 arguments: env.data; PREC (rainfall precipitation in mm, default is PREC=’PRECTOT’); Tdew (dew point temperature in °C, default is Tdew=’T2M_DEW’); Tmax (maximum air temperature in °C, default is Tmax=’T2M_MAX’); Tmin (minimum air temperature in °C, default is Tmin=’T2M_MIN’); RH (relative air humidity %, default is RH=’RH2M’); Rad (net radiation, in MJ m-2 day-1, default is Rad =’Srad’); alpha (empirical constant accounting for vapor deficit and canopy resistance values, default is alpha=1.26); Alt (altitude, in meters above sea level, default is Alt = ALT); G (soil heat flux in W m-2, default is G=0); and merge (default is merge=TRUE). 


**Example**

```{r, eval=FALSE}
AtmData  = param_atmospheric(env.data = env.data, Alt = 1628) 
head(AtmData)
env.data = param_atmospheric(env.data = env.data, Alt = 1628,merge = TRUE)
env.data = param_atmospheric(env.data = env.data, merge = TRUE)
head(env.data)
```

**Hands-on 6: plot the RTA and VPD across the time interval. What conclusions can you make?**

## summaryWTH: summarizing raw-data

A basic data summary of the outputs from the get_weather function is done by the summaryWTH() function. This function has 10 arguments (env.data, id.names, env.id, days.id, var.id, statistic, probs, by.interval, time.window, and names.window). The common arguments with extract_GIS have the same described utility. Other identification columns (year, location, management, responsible researcher, etc.) may be indicated in the id.names argument, e.g., id.names = c(‘year’,’location’,’treatment’)

```{r, eval=FALSE}
summaryWTH(env.data = env.data, env.id = 'env', days.id = 'daysFromStart',statistic = 'mean')
summaryWTH(env.data = env.data) # by default
```

**Hands-on 7: what happens if we use quantiles? or define time intervals?**

**Hands-on: the final challenge**

Collect T2M data, and then process for soybean in the following places and time windows: Goiânia (Brazil, 16.67 S, 49.25 W, from March 15th, 2020 to April 04th, 2020); Texcoco (Mexico, 19.25 N, 99.50 W, from May 15th, 2019 to June 15th, 2019); Brisbane (Australia, 27.47 S, 153.02 E, from September 15th, 2018 to October 04th, 2018); Montpellier (France, 43.61 N, 3.81 E, from June 18th, 2017 to July 18th, 2017); Los Baños (the Philippines, 14.170 N, 121.431 E, from May 18th, 2017 to June 18th, 2017); Porto-Novo (Benin, 6.294 N, 2.361 E, from July 18th, 2016 to August 18th, 2016), Cali (Colombia, 3.261 N, 76.312 W, from November 18th, 2017 to December 18th, 2017); Palmas (Brazil, 10.168 S, 48.331 W, from December 18th, 2017 to January 18th, 2018);


**Example of running multiple scenarios (common way and with parallelization)**


```{r, eval=FALSE}


env = c('GOI','TEX','BRI','MON','LOS','PON','CAL','PAL')
lat = c(-16.67,19.25,-27.47,43.61,14.170,6.294,3.261,-10.168)
lon = c(-49.25,-99.50,153.02,3.87,121.241,2.361,-76.312,-48.331)
cou = c('BRA','MEX','AUS','FRA','PHL','BEN','COL','BRA')
start = c('2020-03-15','2019-05-15','2018-09-15',
          '2017-06-18','2017-05-18','2016-07-18',
          '2017-11-18','2017-12-18')
end = c('2020-04-15','2019-06-15','2018-10-15',
        '2017-07-18','2017-06-18','2016-08-18',
        '2017-12-18','2018-01-18')


require(foreach)

# current way (ok, I agree that is slow, specially for A LARGE NUMBER of environments)
env.data = get_weather(env.id = env, lat = lat, lon = lon, start.day = start, end.day = end)
head(env.data)

require(doParallel)
require(foreach)

cl <- makeCluster(3) # number of clusters
registerDoParallel(cl)

myData = foreach(ENV = 1:length(env), .combine = "rbind") %dopar%
  {
    cat(paste0('doing....',env[ENV],'\n'))
    myEnvData = EnvRtype::get_weather(
                            env.id     = env  [ENV], 
                             lat       = lat  [ENV], 
                             lon       = lon  [ENV], 
                             start.day = start[ENV], 
                             end.day   = end  [ENV],
                             country   = cou  [ENV]
                             
                            )
    
    return(myEnvData)
    
  }

stopCluster(cl)

head(myData)

```

**Example for GEMS study group (maize in Sergipe, Brazil)**


```{r, eval=FALSE}
sites    = c('Nossa Senhora da Gloria','Caninde do Sao Francisco',
             'Gararu','Monte Alegre','Nossa Senhora de Lourdes',
             'Poco Redondo','Porto da Folha')
latitudes  = c(-10.217,-9.642,-9.967,-10.027,-10.320,-9.798,-9.909)
longitudes = c(-37.424,-37.788,-37.090,-37.558,-36.578,-37.6903,-37.278)

start = c('2020-03-15','2019-05-15','2018-09-15',
          '2017-06-18','2017-05-18','2016-07-18',
          '2017-11-18','2017-12-18')
end = c('2020-04-15','2019-06-15','2018-10-15',
        '2017-07-18','2017-06-18','2016-08-18',
        '2017-12-18','2018-01-18')


require(foreach)
gems.data = foreach(ENV = 1:length(sites), .combine = "rbind") %dopar%
  {

    myEnvData = get_weather(
      env.id    = sites      [ENV], 
      lat       = latitudes  [ENV], 
      lon       = longitudes [ENV], 
     # start.day = start[ENV], 
    #  end.day   = end  [ENV],
      country   = 'BRA'
      
    )
    
    return(myEnvData)
    
  }

head(gems.data)
```

**hands-on: now adapt it for your planting dates, crop, motivations....**
