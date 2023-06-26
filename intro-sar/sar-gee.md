---
layout: page
title: Sentinel-1 data in Earth Engine
parent: Introduction to Synthetic Aperture Radar (SAR)
nav_order: 2
---

# Sentinel-1 data in Earth Engine

## Optical vs SAR data

The following code is to show clear SAR images compared to cloud-covered optical imagery. You don't need to worry about the code, just explore the map and see how available Sentinel-1 compare with cloud-covered Sentinel-2 images from the same time period. This is one of the main advantages or radar data, the cloud penetration ("all-weather" monitoring).

Script "`1 Optical vs. SAR`" from the repository and folder `T4` or direct link: [https://code.earthengine.google.com/d28e842183019d39b797e914b3e1bec1](https://code.earthengine.google.com/d28e842183019d39b797e914b3e1bec1).

<img align="center" src="../images/intro-sar/1.png"  vspace="10" width="700">

## Sentinel-1 Acquisition modes

As seen in the Theory section, Sentinel-1 satellite has different acquisition modes (Stripmap (SM), Interferometric Wide swath (IW), Extra-Wide swath (EW), and Wave mode (WV)). The following code shows data availability in two modes: IW and EW. Again, don't worry about the code, just explore the map and data availability. Where are the EW most available? Over Suriname, which mode would you use?

Script "`2 Sentinel-1 Acquisition modes`" from the repository and folder `T4` or direct link: [https://code.earthengine.google.com/26ddeacb2cecbb4e427dcc126f3759e4](https://code.earthengine.google.com/26ddeacb2cecbb4e427dcc126f3759e4).

<img align="center" src="../images/intro-sar/2.png"  vspace="10" width="700">

## Sentinel-1 Polarizations

The following code allows you to explore the Sentinel-1 images in different polarizations. What can you observe from the VV data compared to the VH data? Use the `Inspector` to inspect pixel values over interesting landscape objects.

Script "`3 Sentinel-1 Polarizations`" from the repository and folder `T4` or direct link: [https://code.earthengine.google.com/e9df5f1152b5fc905fcb2d8971efc84f](https://code.earthengine.google.com/e9df5f1152b5fc905fcb2d8971efc84f).

<img align="center" src="../images/intro-sar/3.png"  vspace="10" width="700">

## Sentinel-1 Visualization

Now, let's go step-by-step to filter the `COPERNICUS/S1_GRD` Sentinel-1 image collection in Earth Engine according to a specific time period and area of interest and add the first image of the collection to the map.

We start by defining the area and time period of interest. We will use a point over Paramaribo (-55.2038, 5.8520) and the month of August 2022 as the time period.

```javascript
//--------------------------------------------------------------
// Define area of interest (vector data) and time period
//--------------------------------------------------------------

var aoi = ee.Geometry.Point([-55.2038, 5.8520]);
Map.centerObject(aoi, 9);
// TIP: Centering the map before adding a layer is more efficient 
// than adding it afterward.

var start = '2022-08-01';
var end = '2022-09-01';
```

Now, we define the SAR data. Here, we will filter the images that have both VV and VH polarizations as bands, that were acquired in the IW mode, and that were acquired in the Descending orbit. We print this collection to the `Console`. Note that `filterMetadata(PROPERTY, 'equals', VALUE)` and `filter(ee.Filter.eq(PROPERTY, VALUE))` are equivalents and perform the same exact function, i.e., they are two ways of accomplishing the same result.

```javascript
//--------------------------------------------------------------
// Define SAR data
//--------------------------------------------------------------

// Filter Sentinel-1 image collection, considering polarization, instrument mode, orbit direction, and the area of interest.
var s1 = ee.ImageCollection("COPERNICUS/S1_GRD")
  .filterMetadata('transmitterReceiverPolarisation', 'equals', ['VV', 'VH'])
  .filter(ee.Filter.eq('instrumentMode', 'IW'))
  .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING')) // check out: https://sentinel.esa.int/web/sentinel/missions/sentinel-1/observation-scenario
  .filterBounds(aoi)
  .filterDate(start, end);

// Print the collection.
print(s1);
```

If you check the printed collection in the `Console`, you will see it only has two images. These images will have three bands, the `VV` band, the `VH` band, and the `angle` band.

<img align="center" src="../images/intro-sar/4.png"  vspace="10" width="700">

Now, let's select the first image in the collection and add it to the map as three different 