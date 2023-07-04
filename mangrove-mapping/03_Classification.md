---
layout: page
title: Classification
parent: Intermediate Google Earth Engine - Mangrove Mapping
nav_order: 3
---

# Classification & Accuracy Assessment

## Steps
1. Import and prepare collections
2. Data sampling (Training & Validation data)
3. Training & Classification
4. Accuracy assessment

### 1. Import and prepare collections

Let's import our collections:

```javascript
// GEE Collections

// Define country boundaries
var suriname = ee.FeatureCollection("USDOS/LSIB/2017")
                .filter(ee.Filter.eq('COUNTRY_NA','Suriname'));
// Visualize feature
Map.addLayer(suriname, {}, 'Suriname');

/////////  Global Mangrove Distribution Data  //////////
var globalMangrove = ee.ImageCollection("LANDSAT/MANGROVE_FORESTS").mosaic();

// Visualize mangrove dataset:
Map.addLayer(globalMangrove, {palette:['green']}, 'Global Mangrove');


/////////  Digital Elevation Model at 30m  //////////
var dem = ee.ImageCollection("COPERNICUS/DEM/GLO30").select('DEM');

// Clip and Visualize DEM:
var demSuriname = dem.mosaic();//.clip(suriname);
var demPalette = ['#002bff','#00f3ff','#00ff37','#fbff00','#ff0000'];
Map.addLayer(demSuriname, {palette: demPalette, min:0, max:850}, 'DEM');


/////////  Normalized Difference Vegetation Index - NDVI  //////////
var ndvi = ee.Image('users/lsandoval-sig/Suriname/NDVI_Map');

// Clip and Visualize NDVI:
var ndviPalette = ['#edf8e9','#c7e9c0','#a1d99b','#74c476','#41ab5d','#238b45','#005a32'];
Map.addLayer(ndvi, {palette: ndviPalette, min:0, max:0.8}, 'NDVI');


/////////  Normalized Difference Water Index - NDWI  //////////
var ndwi = ee.Image('users/lsandoval-sig/Suriname/NDWI_Map');

// Clip and Visualize NDWI:
var ndwiPalette = ['#ffffff','#0059ff','#1d00ff','#0c00b0'];
Map.addLayer(ndwi, {palette: ndwiPalette, min:0, max:1}, 'NDWI');


////////////  Multispectral data: Sentinel-2 at 10m  /////////////
var sentinel2 = ee.Image("users/lsandoval-sig/Suriname/Suriname_Map");

// Visualize map
var visParams = {bands: ['B4','B3','B2'], min:0, max:2000};
Map.addLayer(sentinel2, visParams, 'Composite');
```

Then, we proceed to put the DEM, NDVI, and NDWI together with the multispectral bands, into a single image that will be sampled previous to classificaiton.

```javascript
// Add bands to prepare image for sampling
var finalImage = sentinel2.addBands(ndvi).addBands(ndwi).addBands(demSuriname);
print('Bands', finalImage.bandNames());
```

<p align="center">
<img src="../images/mangrove/T5_3_01.png" vspace="10" width="500">
</p>

### 2. Data sampling (Training & Validation data)

```javascript
// Import our collection of points
var points = ee.FeatureCollection('users/lsandoval-sig/Suriname/Mangrove_samples');

// Define bands to sample:
var bands = ['B1','B2', 'B3', 'B4', 'B5', 'B6', 'B7','B8','B8A','B9','B11', 'DEM', 'NDVI','NDWI'];

// Property name containing the mangrove classes.
var property = 'mangroves';

// Print number of points per classe:
print('Points per class:',points.aggregate_histogram('mangroves'));

// Sample data at each point location:
var samples = finalImage.select(bands).sampleRegions({
  collection: points,
  properties: [property],
  scale: 10
});

//print('Samples:', samples);
```

If we print the collection of samples, we will see the specific values per band at each location.

<p align="center">
<img src="../images/mangrove/T5_3_02.png" vspace="10" width="500">
</p>

We will split our collection of points into training and validation points. Usually, training set is 80% of the total collection, while the validation set is 20%. The collection of points will be randomly split by adding a column with random numbers using `randomColumn('random')`, and then filtering points higher and lower than 0.8, which is our threshold for spliting the data.

```javascript
// Split samples into training and validation sets.
// First, we will add a column containing random numbers between 0 and 1
var random = samples.randomColumn('random');

// Split sample collection into training (80%) and validation (20%)
var fraction = 0.8;  
var training = random.filter(ee.Filter.lt('random', fraction));
var validation = random.filter(ee.Filter.gte('random', fraction));

// Verify number of training and validation points
print('Training points:', training.aggregate_histogram(property));
print('Validation points:', validation.aggregate_histogram(property));
```

### 3. Training & Classification



### 4. Accuracy Assessment
