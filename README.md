# The Geospatial_studio 
**NOTE: This tutorial provides basic steps for importing and processing Geospatial data in Blender using BlenderGIS Addon. For comprehensive review of the features and manual of the addon please visit [domlysz/BlenderGIS repository](https://github.com/domlysz/BlenderGIS). 
## Pre-requisites
1. Blender  [Latest Build](https://www.blender.org/download/)
2. BlenderGIS [Add-on](https://github.com/domlysz/BlenderGIS) 
3. [GRASS GIS](https://grass.osgeo.org/download/) or ARCGIS
4. [Download](./sample/) sample data folder
5. Laptop with Windows 7 or higher, Mac OS X or Linux

## Preparing Geospatial data in GRASS GIS (optional)
*Note: for comprehensive tutorial on importing different data-types (e.g., Lidar, vectors and rasters) using GRASS GIS and ARCGIS see [Geospatial Modelling Module](http://ncsu-geoforall-lab.github.io/geospatial-modeling-course/grass/terrain_modeling.html)   
#### Exporting Raster
* Open GRASS GIS
* Convert rasters to Geotiff files with world file
For Blender GIS addon to import rasters, you need either tagged Geotiffs or Geotiffs with world files (.tfw). Not all raster types are directly readable in Blender. Specially if you wantwant use it as elevation modr

# The Geospatial_studio 
**NOTE: This tutorial provides basic steps for importing and processing Geospatial data in Blender using BlenderGIS Addon. For comprehensive review of the features and manual of the addon please visit [domlysz/BlenderGIS repository](https://github.com/domlysz/BlenderGIS). 
## Pre-requisites
1. Blender  [Latest Build](https://www.blender.org/download/)
2. BlenderGIS [Add-on](https://github.com/domlysz/BlenderGIS) 
3. [GRASS GIS](https://grass.osgeo.org/download/) or ARCGIS
4. [Download](./sample/) sample data folder
5. Laptop with Windows 7 or higher, Mac OS X or Linux

## Preparing Geospatial data in GRASS GIS (optional)
*Note: for comprehensive tutorial on importing different data-types (e.g., Lidar, vectors and rasters) using GRASS GIS and ARCGIS see [Geospatial Modelling Module](http://ncsu-geoforall-lab.github.io/geospatial-modeling-course/grass/terrain_modeling.html)   
#### Exporting Raster
* Open GRASS GIS
* Convert rasters to Geotiff files with world file. For best compatibility, export geotiff with float32 
<p style="text-align:left;">`r.out.gdal input=elevation@PERMANENT output=D:\Geospatial_studio\elevation format=GTiff type=Float32`




----------