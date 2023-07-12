---
layout: page
title: Flood Mapping
parent: Intermediate Google Earth Engine - Flood Mapping
nav_order: 2
---

## Script
The script of this section is available [here]().

# Flood Mapping

Flood mapping is the detection of water above its usual level. Normally, the areas in risk of flooding are close to the coast, water bodies such as rivers or lakes, or areas with geomorphological characteristics that allows accumulation of water. Flooding may be caused by natural events such as high precipitations or sea-level rise, but some anthropogenic events can caused flooding by deforestion or lanscape changes. 

Flood mapping can be done with multispectral data under some specific conditions such as no clouds and visible water bodies (e.g. not covered by vegetation).
However, flooding events are most likely related to high precipitation, that means using multispectral data might not be possible due to high presence of clouds covering the area of interest during the event. This is not a limitation for Synthetic Aperture Radar (SAR) data, which allows to detect water bodies or flooded areas, even with cloud presence and dense vegetation. The [Sentinel-1 SAR collection](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S1_GRD) is available on GEE providing data from 2014 to present every 16 days at 5.4 GHz (C-band) at three resolutions 10, 25, and 40 m per pixel, and different polarizations (HH, VV, VH, HV). 

We will do flood mapping of the [flooding events reported on March 2022 in Suriname](https://en.wikipedia.org/wiki/2022_Suriname_floods). In the previous section, we were able to confirm there was very high precipitation in March 2022, specially in Central-South Suriname, close to the mountains and to the Suriname River.

## Steps


<p align="center">
<img src="../images/flood/file.jpeg" vspace="10" width="600">
</p>
