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

<p align="center">
<img src="../images/flood/T6_2_01.png" vspace="10" width="800">
</p>

We will do flood mapping of the [flooding events reported on March 2022 in Suriname](https://en.wikipedia.org/wiki/2022_Suriname_floods). In the previous section, we were able to confirm there was very high precipitation in March 2022, specially in Central-South Suriname, close to the mountains and to the Suriname River.

## Steps
1. Import collections
2. Filter images before/after the event
3. Detect flooded areas

## 1. Import collections

We will import the Sentinel-1 collection, the Suriname boundary, and the [JRC collection of global surface](https://developers.google.com/earth-engine/datasets/catalog/JRC_GSW1_4_GlobalSurfaceWater). The JRC collection provides water occurrence at 30 m per pixel and will help to identify water bodies of Suriname, before detecting floods.

```javascript
// Reported flooding events on March 10, 2022.
// Let's set a wide time range between January and May
var ini = '2022-01-01';
var end = '2022-05-30';

// Define country boundaries
var suriname = ee.FeatureCollection("USDOS/LSIB/2017")
                .filter(ee.Filter.eq('COUNTRY_NA','Suriname'));
Map.addLayer(suriname, {}, 'Suriname');

// Import JRC collection of global surface water:
var jrc = ee.Image("JRC/GSW1_4/GlobalSurfaceWater")
            .select('occurrence')
            .clip(suriname)
            .gt(40).selfMask();

Map.addLayer(jrc,{palette:['#001eff']},'Water Occurrence');
```

The Sentinel-1 collection will be filtered using several properties to get specific images. We will use images with VV polarization, interferometric wide swath mode (IW mode - see more [here](https://sentinel.esa.int/web/sentinel/user-guides/sentinel-1-sar/acquisition-modes)), and 10 m resolution. The time period to filter will be from January 2022 to May 2022.

```javascript
// Prepare the Sentinel-1 collection
var sar = ee.ImageCollection("COPERNICUS/S1_GRD")
          .filterBounds(aoi)
          .filterDate(ini, end)
          .filter(ee.Filter.eq('instrumentMode', 'IW'))
          .filterMetadata('resolution_meters', 'equals', 10)
          .select('VV');

// Collection details
print('SAR Collection:', sar);
print('Dates:', sar.aggregate_array('system:time_start')
                      .map(function(x){return ee.Date(x)}));
```

Additionally, we printed the filtered collection and the date of each image in a readable format.

<p align="center">
<img src="../images/flood/T6_2_02.png" vspace="10" width="500">
</p>


## 2. Filter images before/after the event



## 3. Detect flooded areas

