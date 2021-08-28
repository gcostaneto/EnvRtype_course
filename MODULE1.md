# MODULE 1 - Remote Environmental Sensing (and its implications)

## Overview

Module 1 starts by collecting raw environmental data from public platforms, such as a satellite-based weather system named ‘NASA’s Prediction of Worldwide Energy Resources’ (NASA POWER, https://power.larc.nasa.gov/), which can access information daily anywhere on earth. This database is well consolidated and validated for use in several research fields, including crop modeling in agricultural research (White et al., 2011; Monteiro et al., 2018; Aboelkhair et al., 2019). Details about resolution and validation of this data source are given in https://power.larc.nasa.gov/docs/methodology/validation/. 
Data collection may span existing experimental trials (single sampling trials) or historical trends for a given location × planting date arrangement. This module gathers the functions for remote data collection of daily weather and elevation data, as well as the computation of ecophysiological variables, such as the effect of air temperature on radiation use efficiency. The module includes a toolbox with ‘Remote Data Collection’ and ‘Data Processing’ steps, both designed to help researchers find a viable alternative for expensive in-field environmental sensing equipment. 



EnvRtype implements the remote collection of daily weather and elevation data by the get_weather function. This function has the following arguments: the environment name (env.id); geographic coordinates (latitude, lat; longitude, lon) in WGS84; time interval (start.day and end.day, given in ‘year-month-day’); and country identification (country), which sets the raster file of elevation for the region of a specific country. Countries are specified by their 3-letter ISO codes:

<img align="center" src="https://github.com/allogamous/EnvRtype/blob/master/fig/ISO3.png" width="60%" height="60%">

