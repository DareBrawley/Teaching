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

###Step One: prepare your files
Create or compile any layers you hope to include in your map in QGIS. This includes georectifying any historical maps you hope to use, and creating any vector data sets (i.e. points, lines, or polygons) that you want to use to annotate your map, and/or cleaning any datasets you have downloaded from other sources.

You can also create new point, line or polygon layers directly from Mapbox. In the following examples we will use a point file to create our annotations. 

In this example we will work with two datasets:
* 1906 map of Bombay which is available [here](http://dsal.uchicago.edu/maps/gazetteer/index.html) from the Digital South Asia Library at the University of Chicago. We digitized this already -- if you need a refresher as you are creating your own map please refer to [this](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/04_MakingData01.md) tutorial. 
* point file that we have created in QGIS based on significant points in the historical map. Creating new shapefiles and digitizing features on a historical map are covered in [this](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/05_MakingData02.md) tutorial so we will not go into the steps here. 

###Step Two: upload datasets to MapBox
* In Safari navigate to [MapBox Studio](https://www.mapbox.com/studio/).
* First we'll upload our historical map which will serve as a base layer. Click the `Tilesets` tab.
	* Select `New Tileset` and then `upload file`. 
	* Browse for and then select the raster file you want to upload. It will need to be a GeoTiff (i.e. you need to have already georectified it in QGIS and saved that file as a .tif)
	* It can sometimes take a little while for these large files to be processed and uploaded via MapBox. It will keep processing even as we move on to the next steps. 

* For one of the two examples provided here the annotations are created using point files which you can click to see photographs or historical images associated with a particular location. I created the initial dataset in QGIS but then imported it directly into the html file used for our map as a GeoJSON file -- this is a good approach for your projects.


###Additional Resources and Examples
This page gives to helpful examples you can modify to create your map but there is a lot more you can do with MapBox! These are some helpful resources for understanding MapBox and its functionalities as well as seeing extensive examples with sample code: 

* Embedding a Sound Cloud clip in a point on your map. [Link](https://www.mapbox.com/mapbox.js/example/v1.0.0/soundcloud-embed/)
* MapBox.js examples. [Link](https://www.mapbox.com/mapbox.js/example/v1.0.0/)
* MapBox.js documentation. [Link](https://www.mapbox.com/mapbox.js/api/v2.4.0/)
* Custom Marker Tooltips. [Link](https://www.mapbox.com/mapbox.js/example/v1.0.0/custom-marker-tooltip/)

###Step Three: design your map! 
##An Example: Historical Map with Annontations

```html
<!DOCTYPE html>
<html>
<!---the information between the two head tags doesn't need to be changed and just sets up the file and its connections to MapBox's API-->
<head>
<meta charset=utf-8 />
<title>An annotated map</title> <!--this is the title of your map's webpage, it will appear on the top bar of the webpage-->
<meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
<script src='https://api.mapbox.com/mapbox.js/v2.4.0/mapbox.js'></script>
<link href='https://api.mapbox.com/mapbox.js/v2.4.0/mapbox.css' rel='stylesheet' />
<!--this css between the two style tags sets up a map that will fill a full browser window regardless of the size of the window-->
<style>
  body { margin:0; padding:0; }
  #map { position:absolute; top:0; bottom:0; width:100%; }
</style>
</head>
<!-- the body of the page is where we now create our map-->
<body>
<!--here we set up the style we will later use for the popups of our annotations-->  
<style>
.leaflet-popup-content img {
  max-width:200px;
}
.leaflet-popup-content p {
  font-size: 10
  fo
}
</style>

<div id='map'></div>

<script>
//replace the mapbox access token here for the one associated with your mapbox account
L.mapbox.accessToken = 'pk.eyJ1IjoiZGFyZXRlYWNoaW5nIiwiYSI6ImNpdTQ4OHAyMjBoNWwyb2xwcTJpNW13bXQifQ.iJXS-wT_PuuszzbfY8ty3A';

//here we create a variable where we will store our map object, inside the parenthesis we define the name of the map element 'map' and we define the id of the content of the map using the MapID from Mapbox. 
//In this case the content of the map is the historical map we have uploaded (Map ID: 'dareteaching.91bd8da2') on top of a tileset provided by map box('mapbox.light')
var map = L.mapbox.map('map','mapbox.light,dareteaching.91bd8da2');

//these are our annotations. Mapbox requires specific file types in order to read data (like how QGIS requires a shapefile). GeoJSONs are a spatial data type like the shape file. they store 'properties' (i.e. attributes) and 'geometry' (i.e. spatial coordinates)
var annotation_data = [ //this line defines the name of the variable where we will store our point data for annotations
  {
  type: 'Feature', 
  geometry: { 
    type: "Point", //we have created a point file here
    coordinates: [ 72.834766783363435, 18.941302655942383 ] //these are the coordinates of our first point
  },
  properties: { //these are the properties associated with our first point
    'marker-color': '#808080', //these styles define the marker type we are using in Mapbox
    'marker-size': 'small',
    'marker-symbol': 'circle',
    name: "Chhatrapati Shivaji Terminus",  //you can add additional properties 
    about: "This is a description of this train station  . . .", 
    url: "https://upload.wikimedia.org/wikipedia/commons/c/c5/Chhatrapati_Shivaji_Terminus_(Victoria_Terminus).jpg" //this url points to a webpage with an image I want to show in the popup for this point
    } 
  }, { //this is the second point in our dataset
    type: 'Feature', 
    geometry: { 
      type: "Point", 
      coordinates: [ 72.842038084801572, 18.951785322265913 ]
    },
    properties: { 
      'marker-color': '#808080',
      'marker-size': 'small',
      'marker-symbol': 'circle',
      name: "Victoria Dock", 
      about: "This is a description of this port", 
      url: "https://upload.wikimedia.org/wikipedia/commons/0/07/The_Victoria_Dock.jpg"
    }
}];

//this creates a variable where we will store the featureLayer 
var myLayer = L.mapbox.featureLayer().addTo(map);

// creates a custom popup using the custom feature properties
myLayer.on('layeradd', function(e) {
    var marker = e.layer,
        feature = marker.feature;

    // Create custom popup content, separate html tags enclosed in single quotes with the names of properties from your geoJSON file
    var popupContent =  '<a target="_blank" class="popup" >' + 
    '<img src="' + feature.properties.url + '"/>' + 
    '<p>' feature.properties.name '</p>'+ '</a>';

    // http://leafletjs.com/reference.html#popup
    marker.bindPopup(popupContent,{
        closeButton: true,
        minWidth: 320
    });
});

// Add features to the map
myLayer.setGeoJSON(annotation_data);

//this sets the default view of the map -- the latitude and longitude of the center point of the map as well as the zoom level
map.setView([18.95, 72.81], 14);

</script>
</body>
</html>
```


##Another Example: Historical Map with Multiple Layers

```html
<!DOCTYPE html>
<html>
<!---the information between the two head tags doesn't need to be changed and just sets up the file and its connections to MapBox's API-->
<head>
<meta charset=utf-8 />
<title>A map with layers</title> <!--this is the title of your map's webpage, it will appear on the top bar of the webpage-->
<meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
<script src='https://api.mapbox.com/mapbox.js/v2.4.0/mapbox.js'></script>
<link href='https://api.mapbox.com/mapbox.js/v2.4.0/mapbox.css' rel='stylesheet' />
<!--this css between the two style tags sets up a map that will fill a full browser window regardless of the size of the window-->
<style>
  body { margin:0; padding:0; }
  #map { position:absolute; top:0; bottom:0; width:100%; }
</style>
</head>
<body>

<div id='map'></div>

<script>
//replace the mapbox access token here for the one associated with your mapbox account
L.mapbox.accessToken = 'pk.eyJ1IjoiZGFyZXRlYWNoaW5nIiwiYSI6ImNpdTQ4OHAyMjBoNWwyb2xwcTJpNW13bXQifQ.iJXS-wT_PuuszzbfY8ty3A';
//here we create a variable where we will store our map object, inside the parenthesis we define the name of the map element 'map' and we define the id of the content of the map
  var map = L.mapbox.map('map', null)
  //.set view allows us to set the default position and zoom level of the map when the page first loads, 
  //the first element within brackets is a set of latitude and longitude coordinates
  //the secoond element is the zoom level
      .setView([18.95, 72.81], 14);
//here we are creating a variable where we will store all of the layers we want to include on the map 
  var layers = {
    //each layer gets a name (i.e. Historical) and then is we use the L.mapbox.tileLayer function to add a tile layer to the map
     Historical: L.mapbox.tileLayer('dareteaching.91bd8da2,mapbox.light'), //this adds the historical map on top of a basemap of contemporary streets (in the mapbox.light style) 
     Streets: L.mapbox.tileLayer('mapbox.light'),
     Satellite: L.mapbox.tileLayer('mapbox.satellite')
  };
  //here call the Historical layer and add this as the base and default layer on our map 
  layers.Historical.addTo(map);
  //here we add all of the layers to the map
  L.control.layers(layers).addTo(map);

</script>
</body>
</html>
```




