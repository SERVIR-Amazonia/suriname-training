---
layout: page
title: LandTrendr Outputs
parent: "Advanced Google Earth Engine - Change Detection 2"
nav_order: 3
---

# Create LandTrendr Outputs

Here are the steps for creating tangible LandTrendr outputs that you can export and use in other analyses or products. The first couple we have already done in the previous section.

# 1. LandTrendr Options

In `LandTrendr Options`, select the best parameters for your project.

# 2. AOI

In `Asset Overlay`, put the path to your AOI in the first blank. Then check the box at the end to use it for the AOI in your analysis. Click the `Add Asset to Map` button.

Alternatively, draw a polygon or create a buffer around a point in `RGB Change Options`.

# 3. RGB Change Options

The `RGB Change Options` section will be used later to calculate changes over time. The value for Red should be your first year of your time period of interest, Blue your last year, and Green somewhere in between. Click the `Add RGB Imagery` button to see the results. This may take a while to load or may fail to visualize if you have a very large study area.

# 4. Pixel Time Series Options

The `Pixel Time Series Options` section is optional for producing final LandTrendr products. It is a quick way to visualize changes and the effects of your chosen parameters, but is not needed for the change analysis.

# 5. Change Filter Options

In `Change Filter Options`, you can choose to visualize and download various versions of the changes detected by LandTRandr. For now, we will create 2 versions of this change data that can be particularly useful in post-processing for finding forest degradation, loss, and growth:

**Greatest Loss:**

* Make sure `Loss` and `Greatest` are selected. You can experiment with other Change Types and Change Sort options later on. 
* Have the `Filter by Year` checked and adjust the time period if needed to include the full time you analyzed. 
* Adjust the years to your time period of interest. This is the time period that the LandTrendr analysis will be run on. 
* All other options should remain unchecked.

**Newest Gain:**

* This time change `Loss` to `Gain` and change `Greatest` to `Newest`. 
* Uncheck the `Filter by Year` option.
* Name the exports differently so you can tell they are the `Gain` version. 
* The RGB-year-year-year and DSNR files are the same as before so for your gain run you only need to download the files ending with: `MAG`, `DUR`, `PREVAL`, `YOD`.

<img align="center" src="../images/change-detection-2/ChangeFilterOptions.png" hspace="15" vspace="10" width="300">

For each of these sets of options, click `Add Filterted Disturbance Imagery`.  On the map, three new images should appear, which all characterize the selected type of change in some way: Year of Detection, Magnitude, and Duration.  Using the **Inspector** tab, click around on the map to see what values are present for these three images, and what these values mean in terms of the changes you filtered for.

**Duration:**

<img align="center" src="../images/change-detection-2/LT_duration.png" hspace="15" vspace="10" width="600">

**Magnitude:**

<img align="center" src="../images/change-detection-2/LT_magnitude.png" hspace="15" vspace="10" width="600">

**Year of Detection:**

<img align="center" src="../images/change-detection-2/LT_yearofdetection.png" hspace="15" vspace="10" width="600">

# 6. Download Options

In `Download Options`, set the `ESPG` to `4326` for WGS 84 (or whatever other coordinate system you want to work in) and set your output file name and path. For now, select `Download RGB Imagery` and `Download Change Imagery` in the `Download Selection` subsection, and click the `Download data` button. 

<img align="center" src="../images/change-detection-2/DownloadOptions.png" hspace="15" vspace="10" width="300">

Download all the resulting files from the **Tasks** tab to your GEE assets, which you can use later for post-processing. There will be 6 files to download, those ending with: `RGB-year-year-year`, `DSNR`, `MAG`, `DUR`, `PREVAL`, `YOD`.

<img align="center" src="../images/change-detection-2/LT_downloadtasks.png" hspace="15" vspace="10" width="400">

You can export the files as assets this by clicking `RUN` on each task, which will take you to a pop-up window that looks like this:

<img align="center" src="../images/change-detection-2/LT_runtask.png" hspace="15" vspace="10" width="400">

Make sure all of the information is correct, and click `RUN` again.