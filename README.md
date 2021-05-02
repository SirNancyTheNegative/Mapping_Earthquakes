# Mapping Earthquake Data through Leaflet and Mapbox

## Overview

### Purpose

In this project, we fetch earthquake and tectonic plate data from the web and compile it into a single interactive map. This data is fetched through multiple sources, assigned a visualization on a map, and added to a map formed using Mapbox API. Its purpose, then, is to demonstrate records of earthquake data with respect to the boundaries between tectonic plates, as well as emphasize the earthquake records with potentially dangerous magnitudes.

## Process

### The Map

Before we can add anything to the map, we have to have the map in the first place. For the map we're using, we need to get the map data we want from Mapbox's API. We want three different maps, in this case: Basic streets, satellite imaging, and a dark-mode map with street data. 

In order to do this, we make a new tileLayer object containing the details of the map. For the case of the streets map, we write the following:

```
let streets = L.tileLayer('https://api.mapbox.com/styles/v1/mapbox/streets-v11/tiles/{z}/{x}/{y}?access_token={accessToken}', {
	attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery (c) <a href="https://www.mapbox.com/">Mapbox</a>',
	maxZoom: 18,
	accessToken: API_KEY
});
```

The satellite view and dark mode view layers are much the same, though with the appropriate URLs to access the correct maps. In order to use all these maps, however, we need a way to switch between them.

### Overlays and Control

If we want to turn on and off information at a whim, we need an overlay that lets us do so. As such, we will make two objects that will hold our maps and layers: `baseMaps` and `overlays`. `baseMaps` will be an object holding the three maps with descriptive keys for each, and `overlays` will hold the layerGroups that we'll form in the subject below.

### Earthquake Data

For the earthquake data, we pulled JSON information using d3's `.json()` function from the web, and made use of it using the `.then()` function in order to decide on style info. For our case, we decided to have dynamically-made circle markers with fill color and size dependent on the magnitude of the earthquake, ranging from green to red for the former and being equal to the magnitude times four for the latter. We put these individual markers into a LayerGroup object labeled `allEarthquakes`, before adding it to the map.

The data containing only earthquakes above a magnitude of 4.5 is defined in much the same way, instead saved in a LayerGroup object labeled `majorEQ` and sourced from a different JSON file.

The data containing the tectonic plates is rather different, however. Besides being pulled from a third JSON file, we only need a style, since this JSON simply draws lines. For the color, We chose a deep orange and a line thickness of 3.
