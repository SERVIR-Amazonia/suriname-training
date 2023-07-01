---
layout: page
title: Preparing Datasets
parent: Intermediate Google Earth Engine - Mangrove Mapping
nav_order: 1
---

# Dataset Identification and Preparation

Before running any habitat mapping it is important to identify the type of data and processing we require to achieve the best possible outcome. In this case we will data available in the catalog of GEE for mapping mangroves in Suriname. The mangrove trees grow in coastal areas, this means we can focus on those areas and there is no need to search for mangroves in inland or highland areas.

We first have to identify the data that best satisty our needs, for example: what pixel, temporal or spectral resolution is better? We will use multispectral data from [Sentinel-2](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR_HARMONIZED), which delivers data at 10 m per pixel every ~5 days from ~2016 to present.

Additionally, the mangrove mapping may use other ancillary data to increase mapping performance. In this we will use digital elevation data (DEM), normalized difference vegetation index (NDVI), and normalized difference water index (NDWI). Previously known mangrove distribution is useful to delimitate our mapping area nad to collect sampling points. The [global mangrove distribution](https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_MANGROVE_FORESTS?hl=en)https://developers.google.com/earth-engine/datasets/catalog/LANDSAT_MANGROVE_FORESTS?hl=en is available in GEE and it was produced by using Landsat imagery at 30 m per pixel.
