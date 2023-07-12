---
layout: page
title: Precipitation Data
parent: Intermediate Google Earth Engine - Flood Mapping
nav_order: 1
---

## Script
The script of this section is available [here]().

# Precipitation Data

In this section, we will review how to import, visualize, and get precipitation data. High precipitation levels can determine whether floodings are observed. We will use the [CHIRPS (Climate Hazards Group InfraRed Precipitation With Station Data) daily data](https://developers.google.com/earth-engine/datasets/catalog/UCSB-CHG_CHIRPS_DAILY), which provides daily global data at 5.5 km from 1981 to present.

```javascript
// Define country boundaries
var suriname = ee.FeatureCollection("USDOS/LSIB/2017")
                .filter(ee.Filter.eq('COUNTRY_NA','Suriname'));
Map.addLayer(suriname, {}, 'Suriname');

// Import the CHIRPS daily precipitation data:
var chirps = ee.ImageCollection("UCSB-CHG/CHIRPS/DAILY");
```

Now, we must filter the CHIRPS collection. We will use the a time range that covers March of 2022, where severe floodings where [reported](https://en.wikipedia.org/wiki/2022_Suriname_floods) in Suriname. Then we sum the data from March using `.sum()` and then clip it to the Suriname polygon.

```javascript
// Filter collection
// Focus on March 2022
var prec = chirps
            .filterDate('2022-03-01','2022-03-31')
            .filterBounds(suriname);

// Get cumulative precipitation of the month and clip it.
var precMonth = prec.sum().clip(suriname);

// Craete precipitation palette and visualize map
var palette = ['ffffd9','edf8b1','c7e9b4','7fcdbb','41b6c4','1d91c0','225ea8','253494','081d58'];
Map.addLayer(precMonth,{min:0, max:850, palette:palette},'Precipitation');
```

<p align="center">
<img src="../images/flood/T6_1_01.png" vspace="10" width="600">
</p>

We can get daily values of precipitation at an individual point or location for March 2022 and plot it. In this case, we require to draw a point in the area where higher precipitations were observed, and then create a chart using the `ui.Chart.image.seriesByRegion()` function.

```javascript
// Draw a pin in the area of higher observed precipitation (uncomment "var pin" if needed)
//var pin = /* color: #d63000 */ee.Geometry.Point([-56.08198200121426, 3.77768041314967]);

// Create chart of monthly precipitation:
var precChart = ui.Chart.image.seriesByRegion({
  imageCollection: prec,
  regions: pin,
  reducer: ee.Reducer.mean(),
  scale: 5500,
});
```

We need to print the variable containing the chart to visualize it. But, before doing that let's set some settings. We can set the type of chart by using `.setChartType()` and set more options with `.setOptions()`. We will use the `'ColumnChart'` type, but other types are available such as `'ScatterChart', 'LineChart', 'BarChart','PieChart', 'AreaChart'`. More info about chart styling can be found [here](https://developers.google.com/earth-engine/guides/charts_style).

```javascript
// Create chart of monthly precipitation:
var precChart = ui.Chart.image.seriesByRegion({
  imageCollection: prec,
  regions: pin,
  reducer: ee.Reducer.sum(),
  scale: 5500,
});

// Set chart type
precChart.setChartType('ColumnChart');

// Chart settings
precChart.setOptions({
  title: 'Daily Precipitation',
  vAxis: {
    title: 'Precipitation (mm)',
  }
});

// Print chart
print(precChart);
```

<p align="center">
<img src="../images/flood/T6_1_02.png" vspace="10" width="600">
</p>
