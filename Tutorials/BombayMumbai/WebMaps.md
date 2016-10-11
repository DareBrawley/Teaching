##Interactive Mapping with MapBox

###The Premise
This page will point you towards resources and tutorials that are helpful for creating relatively simple online interactive maps using MapBox and QGIS. 

In this example we will create a map that is comprised of the 1906 map of the City of Bombay along with several points of interest that users can click to see contemporary photographs of different areas of the city.  

###Before you begin
* Visit [MapBox](https://www.mapbox.com/studio/signup/) and create a free MapBox account.
* We will be partly designing our maps in HTML, so while you can use any text editor (such as TextEdit) it is a good idea to download [Sublime Text](https://www.sublimetext.com/2).

###To Begin
In broad strokes the process of using MapBox for this kind of assignment involves three steps: 
 1.  Prepare datasets in QGIS (or ArcGIS)
 2.  Upload datasets to MapBox
 3.  Design map (including the symbology of the layers) and combine the different layers using HTML. 

For additional guidannce on using MapBox beyond the tools covered in this tutorial please refer to these [examples published by MapBox.](https://www.mapbox.com/mapbox.js/example/v1.0.0/) 

###Step One: Prepare files
Create or compile any layers you hope to include in your map in QGIS. This includes georectifying any historical maps you hope to use, and creating any vector data sets (i.e. points, lines, or polygons) that you want to use to annotate your map, and/or cleaning any datasets you have downloaded from other sources.

In this example we will work with two datasets:
* 1906 map of Bombay which is available [here](http://dsal.uchicago.edu/maps/gazetteer/index.html) from the Digital South Asia Library at the University of Chicago. We digitized this already -- if you need a refresher as you are creating your own map please refer to [this](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/04_MakingData01.md) tutorial. 
* point file that we have created in MapBox based on the historical map. Creating new shapefiles and digitizing features on a historical map are covered in [this](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/05_MakingData02.md)tutorial so we will not go into the steps here. 

###Step Two: Upload datasets to MapBox
MapBox only accepts certain formats of data -- just like QGIS requires specific file formats like the shapefile -- thus we first need to export our new point layer as a GeoJSON file. 
* right click the layer name in the layers menu and select `save as`. Then in the format down menu select `GeoJSON`. Name your layer and save it. **Import Note**: when you are creating new annotation layers make sure that the projection system is `EPSG:4326, WGS:84`. 
![img](https://github.com/DareBrawley/Teaching/blob/master/Tutorials/BombayMumbai/Images/GEOjson.png)
* once you have saved your GeoJSON file open it with sublime text. It will look like this: 
![img](https://github.com/DareBrawley/Teaching/blob/master/Tutorials/BombayMumbai/Images/GeoJSON-file.png)
* in order for MapBox to read it correctly you will need to delete the second line (the one that begins with `"crs"`, in this case `"crs": { "type": "name", "properties": { "name": "urn:ogc:def:crs:OGC:1.3:CRS84" } },`)

Next we will upload these layers to MapBox. 
* In Safari navigate to [MapBox Studio](https://www.mapbox.com/studio/).
* First we'll upload our historical map which will serve as a base layer. Click the `Tilesets` tab.
	* Select `New Tileset` and then `upload file`. 
	* Browse for and then select the raster file you want to upload. It will need to be a GeoTiff (i.e. you need to have already georectified it in QGIS and saved that file as a .tif)
	* It can sometimes take a little while for these large files to be processed and uploaded via MapBox. It will keep processing even as we move on to the next steps. 

###An Example: Historical Map

```html
<!DOCTYPE html>
<html>
<head>
<meta charset=utf-8 />
<title>A simple map</title>
<meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
<script src='https://api.mapbox.com/mapbox.js/v2.4.0/mapbox.js'></script>
<link href='https://api.mapbox.com/mapbox.js/v2.4.0/mapbox.css' rel='stylesheet' />
<style>
  body { margin:0; padding:0; }
  #map { position:absolute; top:0; bottom:0; width:100%; }
</style>
</head>
<body>
<style>
.leaflet-popup-content img {
	max-width:200px;
}
</style>

<div id='map'></div>

<script>
L.mapbox.accessToken = 'pk.eyJ1IjoiZGFyZXRlYWNoaW5nIiwiYSI6ImNpdTQ4OHAyMjBoNWwyb2xwcTJpNW13bXQifQ.iJXS-wT_PuuszzbfY8ty3A';
  var map = L.mapbox.map('map', null, {
      maxZoom: 18
  }).setView([18.95, 72.81], 14);

  var layers = {
     Historical: L.mapbox.tileLayer('mapbox.light,dareteaching.91bd8da2'), //this adds the historical map on top of a basemap of contemporary streets (in the mapbox.light style) 
     Streets: L.mapbox.tileLayer('mapbox.light'),
     Satellite: L.mapbox.tileLayer('mapbox.satellite')
  };

  layers.Historical.addTo(map);
  L.control.layers(layers).addTo(map);

</script>
</body>
</html>
```




###Additional Resources and Examples
* Embedding a Sound Cloud clip in a point on your map. [Link](https://www.mapbox.com/mapbox.js/example/v1.0.0/soundcloud-embed/)
* MapBox.js examples. [Link](https://www.mapbox.com/mapbox.js/example/v1.0.0/)
* MapBox.js documentation. [Link](https://www.mapbox.com/mapbox.js/api/v2.4.0/)
* Custom Marker Tooltips. [Link](https://www.mapbox.com/mapbox.js/example/v1.0.0/custom-marker-tooltip/)

