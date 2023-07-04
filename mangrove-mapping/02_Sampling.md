---
layout: page
title: Ground-truth Data
parent: Intermediate Google Earth Engine - Mangrove Mapping
nav_order: 2
---

## Script
The script of this section is available [here]().

# Collecting Ground-Truth Data

Ground-truth data is all the data we can collect from the field, visual observations, or existing datasets that will be used for training and validating classification models. In this case, we will focus on mangrove presence and absence data, using the previous datasets we prepared in the previous section. We have a previous mangrove distribution map, elevation data, NDVI data to distinguish vegetation, NDWI data to distinguish water, and the multispectral data. These datasets will help us to define sampling areas for mangrove presence and absence, and are expected to improve the classification output.

All the exported data can be found in the tab `Assets`, we can click in any of them to see their properties and the ID for importing each dataset in the code editor.

<p align="center">
<img src="../images/mangrove/T5_2_01.png" vspace="10" width="400">
<p/>

The data in our Assets is private for default, and can be shared with some other user or shared publicly. For example, in this section we will import and work with data from my assets.

<p align="center">
<img src="../images/mangrove/T5_2_02.png" vspace="10" width="500">
<p/>

## Steps
1. Import datasets
2. Sampling points
3. Export data

### 1. Import datasets

We will load country boundaries, global mangrove distribution, DEM, NDVI, NDWI, and the multispectral data (composite).

```javascript
// GEE Collections

// Define country boundaries
var suriname = ee.FeatureCollection("USDOS/LSIB/2017")
                .filter(ee.Filter.eq('COUNTRY_NA','Suriname'));

Map.addLayer(suriname);

/////////  Global Mangrove Distribution Data  //////////
var globalMangrove = ee.ImageCollection("LANDSAT/MANGROVE_FORESTS").mosaic();

// Visualize mangrove dataset:
Map.addLayer(globalMangrove, {palette:['green']}, 'Global Mangrove');


/////////  Digital Elevation Model at 30m  //////////
var dem = ee.ImageCollection("COPERNICUS/DEM/GLO30").select('DEM');

// Clip and Visualize DEM:
var demSuriname = dem.mosaic().clip(suriname);
var demPalette = ['#002bff','#00f3ff','#00ff37','#fbff00','#ff0000'];
Map.addLayer(demSuriname, {palette: demPalette, min:0, max:850}, 'DEM');


/////////  Normalized Difference Vegetation Index - NDVI  //////////
var ndvi = ee.Image('users/lsandoval-sig/Suriname/NDVI_Map');

// Clip and Visualize NDVI:
var ndviPalette = ['#edf8e9','#c7e9c0','#a1d99b','#74c476','#41ab5d','#238b45','#005a32'];
Map.addLayer(ndvi, {palette: ndviPalette, min:0, max:0.8}, 'NDVI');


/////////  Normalized Difference Water Index - NDWI  //////////
// Let's import NDWI and set the thershold for water/land (-0.1):
var ndwi = ee.Image('users/lsandoval-sig/Suriname/NDWI_Map').lte(-0.1);

// Clip and Visualize NDWI:
var ndwiPalette = ['#0c00b0','#ffffff'];
Map.addLayer(ndwi, {palette: ndwiPalette, min:0, max:1}, 'NDWI');


////////////  Multispectral data: Sentinel-2 at 10m  /////////////
var sentinel2 = ee.Image("users/lsandoval-sig/Suriname/Suriname_Map");

// Visualize map
var visParams = {bands: ['B4','B3','B2'], min:0, max:2000};
Map.addLayer(sentinel2, visParams, 'Composite');
```

