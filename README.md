

# The Geospatial_studio 
**NOTE: This tutorial intends to provide basic steps for importing and processing Geospatial data in Blender using BlenderGIS Addon. For comprehensive review of the features and manual of the addon please visit [domlysz/BlenderGIS repository](https://github.com/domlysz/BlenderGIS). 
## Pre-requisites
1. Blender  [Latest Build](https://www.blender.org/download/)
2. BlenderGIS [Add-on](https://github.com/domlysz/BlenderGIS) 
3. [GRASS GIS](https://grass.osgeo.org/download/) or ARCGIS
4. [Download](./sample/) sample data folder
5. Laptop with Windows 7 or higher, Mac OS X or Linux

### Exporting Geospatial data in GRASS GIS (optional)
*Note: similar operations can be performed using ArcGIS. 
For comprehensive tutorial on importing different data-types (e.g., Lidar, vectors and rasters) using GRASS GIS and ARCGIS see [Geospatial Modelling Module](http://ncsu-geoforall-lab.github.io/geospatial-modeling-course/grass/terrain_modeling.html).
   
__Exporting Raster__

* Open GRASS GIS
* Export rasters with Geotiff format containing the world file. For best compatibility, use float32 as datatype.

Example for Lidar DEM:
`r.out.gdal input=elev_cut@Dix_10_12 output=D:\Geospatial_studio\elevation.tif format=GTiff type=Float32 createopt=TFW=YES`

__Exporting Vector__

* Export vector data as ESRI shapefiles (.shp)
*Note: When exporting the shape files preserve attributes such as unique object identifier or spatial information such as elevation or height.  

Example for buildings vector:
`v.out.ogr input=Buidlings@Dix_10_12 output=D:\Geospatial_studio\Buildings format=ESRI_Shapefile`

----------
__Exporting Textures__

###1. Preparing Blender scene 

####1. 1. Setup Blender GIS addon
* Make sure that addon is properly installed 
* Open Blender and from __file__ menu select __user preferences__ (or  use __Alt + Ctrl + U__ ), and then select __Add-ons__ tab.  
* In the search tab on top left type 'gis' and make sure that in the __Categories__ section __All__ is selected.
* You should be able to see multiple components of the Addon. Select to activate all components.
*  From the bottom of the preferences window click __Save User Settings__ 
 
####1. 2. Adding a new predefined coordinate reference system (CRS)

*Note: A much more comprehensive instruction on Georefencing management is available at BlenderGIS addon [gitub page](https://github.com/domlysz/BlenderGIS/wiki/Gereferencing-management).

* Before setting up the coordinate reference system of the Blender scene and configuring the scene projection, you should know the Coordinate Reference System (CRS) and the Spatial Reference Identifier (SRID) of your project. In GRASS GIS, CRS information can be retrieved from  `v.info` or `r.info` . You get find the SRID from [http://epsg.io/](http://epsg.io/) or [spatial reference website ](http://spatialreference.org/) using your CRS. The example data sets in this exercise have a NAD83(HARN)/North Carolina CRS (SSRID ESPG: 3358).   

*  In BlenderGIS add-on section (in preferences windows), select to expand the __3D View: BlenderGIS__ . 
*  In the preferences section find __Spatial Reference system__ and click on the __+ Add__ button. 
*  In the add window put  'ESPG: 3358'  for __definition__ and 'NAD83(HARN)/North Carolina' for __Description__. Then select __Save to addon preferences__.


####1. 3. Opening the blender file and setting the Coordinate system
* From the __file__ menu, select __open__  , browse to find the downloaded 'Geospatial_studio' folder and open the 'studio.blend' file
* From the __3D view__ toolbar (on the left side of the screen) , find __GIS__ panel. 
If you cannot find the GIS tab, then check if the add-on is properly installed and activated in blender preferences (step 1.1. )
*  In the second section of the panel , __Geoscene__, click on the Gear shaped icon . You should be able to find and select the 'NAD83(HARN)/North Carolina' preset. After selecting it click on the __√__ icon to set it as scene coordinate system.

###2. Importing Geospatial datasets
####2. 1. rasters
Rasters can be imported and used in different ways. You can import them _As DEM_ to use it as a 3D surface or as_Raw DEM_  to be triangulated or skinned inside Blender. You can select _On Mesh_ to drape them as a texture on your 3D meshes. In this example, we import the lidar elevation dataset as a 3D mesh using _As DEM_ method. 
* In the __file __ menu select__import__ and find __Georeferenced raster__.
* On the bottom left side of the window find  __Mode__ and select __As DEM__.
* For __Subdivision__ select  __Mesh__ and make sure that __CRS__ is set to NAD83(HARN)/North carolina.
* Browse to the 'Geospatial Studio' folder and select 'elevation.tif'
####2.1. Vectors 








