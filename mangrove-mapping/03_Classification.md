---
layout: page
title: Classification
parent: Intermediate Google Earth Engine - Mangrove Mapping
nav_order: 3
---

# Classification & Accuracy Assessment

```javascript
// Define country boundaries
var suriname = ee.FeatureCollection("USDOS/LSIB/2017")
                .filter(ee.Filter.eq('COUNTRY_NA','Suriname'));
// Visualize feature
Map.addLayer(suriname, {}, 'Suriname');
```
