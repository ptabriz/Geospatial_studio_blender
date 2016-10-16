

# 3D Visualization of Geospatial Data with Blender
**NOTE: This tutorial intends to provide basic steps for importing and processing Geospatial data in Blender using BlenderGIS Addon. For comprehensive review of the features and manual of the addon please visit domlysz/BlenderGIS [repository](https://github.com/domlysz/BlenderGIS). 
### Pre-requisites
1. Blender  [Latest Build](https://www.blender.org/download/)
2. BlenderGIS [Add-on](https://github.com/domlysz/BlenderGIS) 
3. [GRASS GIS](https://grass.osgeo.org/download/) or ARCGIS
4. Download [Repository](https://github.com/ptabriz/Geospatial_studio_blender).
5. Laptop with Windows 7 or higher, Mac OS X or Linux

### 0. Exporting Geospatial data in GRASS GIS (optional)
*Note: similar operations can be performed using ArcGIS. 
For comprehensive tutorial on importing different data-types (e.g., Lidar, vectors and rasters) using GRASS GIS and ARCGIS see [Geospatial Modelling Module](http://ncsu-geoforall-lab.github.io/geospatial-modeling-course/grass/terrain_modeling.html).
   
####0. 1. Exporting Raster

* Open GRASS GIS
* Export rasters with Geotiff format containing the world file. For best compatibility, use float32 as datatype.

__`Command Console >>>`__<br>
>`r.out.gdal input=elev_cut@Dix_10_12 output=D:\Geospatial_studio\elevation.tif >format=GTiff type=Float32 createopt=TFW=YES`

####0. 2. Exporting Vector

* Export vector data as ESRI shapefiles (.shp)
*Note: When exporting the shape files preserve attributes such as unique object identifier or spatial information such as elevation or height.  

__`Command Console >>>`__ <br>
>`v.out.ogr input=Buidlings@Dix_10_12 output=D:\Geospatial_studio\Buildings >format=ESRI_Shapefile`

----------

###1. Preparing Blender scene 

####1. 1. Setup Blender GIS addon
* Make sure that addon is properly installed 
* Open Blender and from __file__ menu select __user preferences__ (or  use __Alt + Ctrl + U__ ), and then select __Add-ons__ tab.  
* In the search tab on top left type 'gis' and make sure that in the __Categories__ section __All__ is selected.
* You should be able to see multiple components of the Addon. Select to activate all components.
*  From the bottom of the preferences window click __Save User Settings__ 
 
####1. 2. Adding a new predefined coordinate reference system (CRS)

*Note: A much more comprehensive instruction on Georefencing management is available at BlenderGIS addon [gitub page](https://github.com/domlysz/BlenderGIS/wiki/Gereferencing-management).

* Before setting up the coordinate reference system of the Blender scene and configuring the scene projection, you should know the Coordinate Reference System (CRS) and the Spatial Reference Identifier (SRID) of your project. In GRASS GIS, CRS information can be retrieved using  `v.info` or `r.info` functions . You get find the SRID from [http://epsg.io/](http://epsg.io/) or [spatial reference website ](http://spatialreference.org/) using your CRS. The example data sets in this exercise have a NAD83(HARN)/North Carolina CRS (SSRID ESPG: 3358).   

*  In BlenderGIS add-on section (in preferences windows), select to expand the __3D View: BlenderGIS__ . 
*  In the preferences section find __Spatial Reference system__ and click on the __+ Add__ button. 
*  In the add window put  'ESPG: 3358'  for __definition__ and 'NAD83(HARN)/North Carolina' for __Description__. Then select __Save to addon preferences__.


####1. 3. Opening the blender file and setting the Coordinate system
* From the __file__ menu, select __open__  , browse to find the downloaded 'Geospatial_studio' folder and open the 'studio.blend' file
* From the __3D view__ toolbar (on the left side of the screen) , find __GIS__ panel. 
If you cannot find the GIS tab, then check if the add-on is properly installed and activated in blender preferences (step 1.1. )
*  In the second section of the panel , __Geoscene__, click on the Gear shaped icon . You should be able to find and select the 'NAD83(HARN)/North Carolina' preset. After selecting it click on the __√__ icon to set it as scene coordinate system.

----------
###2. Importing Geospatial datasets
####2. 1. rasters
Rasters can be imported and used in different ways. You can import them _As DEM_ to use it as a 3D surface or as_Raw DEM_  to be triangulated or skinned inside Blender. You can select _On Mesh_ to drape them as a texture on your 3D meshes. In this example, we import the lidar elevation dataset as a 3D mesh using _As DEM_ method. 

* From  __file __ menu select__import__ and find __Georeferenced raster__.
* On the bottom left side of the window find  __Mode__ and select __As DEM__.
* For __Subdivision__ select  __Mesh__ and make sure that __CRS__ is set to NAD83(HARN)/North carolina.
* Browse to the 'Geospatial Studio' folder and select 'DEM.tif'
* If all the steps are followed correctly, you should be able to see the terrain in 3D view window. 
* In _3D view_ __Right-click__ on the DEM object to select it , push __Alt+c__ and select __Mesh from Curve__ . Make sure the mouse cursor is in the _3D view_ area. This is step converts the imported raster to _Mesh_ object type to allow further modification.  

*Trouble shooting: When importing your own raster data, you might encounter situations where the DEM is imported as a flat surface. Make sure that 1) you selected the _As DEM_ method 2) the raster resolution is not very coarse, 3) the data-type is float32 and 4) the coordinate system of the raster is matching the Blender Scenes' coordinate system. For more detailed instructions and troubleshooting read [georeference raster import](https://github.com/domlysz/BlenderGIS/wiki/Import-georef-raster) wiki .

__`Python Console >>>`__
>`bpy.ops.importgis.georaster(filepath="D:\Geospatial_studio\DEM.tif",importMode="DEM",subdivision="mesh")`<br>
>`bpy.ops.object.convert(target='MESH')`

####2.1. Vectors 
*Note: When importing a shapefile it's necessary to specify the projection of the file
Blender Addon reads into the attribute table of the shapefiles. This is helpful when you want to dismantle the shape files into discrete and uniquely named 3D objects. Also, the _z coordinate_ or _elevation_ attributes of the vectored can be used to extrude or define the hight of the 3D objects. In this tutorial,  we use two different shape-files to entertain these attributes. First, we will import the building shape file , and will assign the _height_ and the _name_ attribute to the corresponding 3D objects. Then, we will import the Lidar pointclouds for high vegetation (trees) using the _Z coordinate_ attribute. 

__Buildings__

* From  __file __ menu select__import__ and find __Shapefile(.shp)__.
* Browse to 'Geospatial_studio\shp' , select 'Building.shp' and click on __import SHP__ on top right of the window. You should be able to see the __import SHP parameters__ dialog on the left side of the screen.
* Select __Elevation from the field__ and from the filed drop-down menu select _elevation_  
* Select __Extrusion from the field__ and from the filed drop-down menu select _height_  
*  Select __separate objects__ and __object name from the field__  , and from the filed drop-down menu select _building_  
* The __CRS__ should be already set. If not, select NAD83(HARN)/North Carolina and click  __OK__.
* You should be able to see and select buildings as separate objects on the terrain, right click on each building object and check its  name in the __outliner__ panel (top right) 
__`Python Console >>>`__
>`bpy.ops.importgis.shapefile(filepath="D:/Geospatial_studio/shp/building.shp",fieldElevName="elevation",fieldExtrudeName="height",fieldObjName='Building',separateObjects=True,shpCRS=3358)`

__Lidar point clouds__

* From  __file __ menu select__import__ and find __Shapefile(.shp)__.
* Browse to 'Geospatial_studio\shp' , select 'Lidar_highveg.shp' and click on __import SHP__.
* In the __import SHP parameters__ dialog, make sure that the correct CRS is indicated and click  __OK__.

----------
###3. Materials and Texture
In this section we will drape _orthophoto_ as a texture on the terrain. You can apply textures to the 3D surfaces in blender using complex mapping methods (e.g. height mapping, bump mapping, normal mapping, displacement mapping, reflection mapping, specular mapping, mipmaps, occlusion mapping). However, [texture mapping](https://en.wikipedia.org/wiki/Texture_mapping) is beyond the scope of this tutorial. If you are interested to learn more about texture mapping and materials in blender, [Blender wikibooks](https://en.wikibooks.org/wiki/Blender_3D:_Noob_to_Pro/Materials_and_Textures) is a good place to start. 

* From the top header select __Cycles Render__ as your rendering engine. <br> Note: Cycles is Blender’s ray-trace based production render engine. Discover more about cycles [here](https://www.blender.org/manual/render/cycles/introduction.html).
* You can design workflows related to materials and textures in _Node editor_ environment. Change the bottom editor panel to __node editor__. This can be done by simply changing the _Editor Type selector_ button which is located at the left side of a header. Every area in Blender may contain any type of editor and it is also possible to open the same type multiple times. . Discover more about node editor [here](https://www.blender.org/manual/en/editors/node_editor/introduction.html).
* From [3D view header](https://wiki.blender.org/index.php/User:Pepribal/Ref/3DView/Header)( bottom of the window) , find __Viewport shading__ button and select __Material__.
*  In 3D view, right click on the DEM to select the object. 
* From the [properties panel](https://en.wikibooks.org/wiki/Blender_3D:_Noob_to_Pro/Properties_Window)(you can find it below the _outliner_), goto __Material__  and from __Material browser select__ 'Ortho_texture'. Now you should be able to see the material workflow in node editor.
*  On the left node , _image Texture_ , find __open image__ , and browse to the geospatial folder to 'select the Ortho.png'. You should be able to see the texture draped on the terrain. 

### 4. Populating Trees
In this step, we populate evergreen and deciduous trees on the terrain surface. There are many ways to do that in Blender.  The most efficient method is __Particle Systems Modifier_ which scatters a predefined duplicates of an objects (_particle_) on the surface of another object ( _Emitter_). The pattern through which the particles are scattered can be defined using _Vertex groups_,using which we simply define the vertices of the surface that should get populated. In this tutorial, the particle systems are already setup and ready to assign to the elevation surface , we will only go through creating the vertex groups.  You can setup your own particle system using the instructions provided [here](https://www.blender.org/manual/physics/particles/particle_system_panel.html?highlight=particle%20systems). 

####4.1. Assigning vertex group

* Right click on the terrain to select 'elevation' object. 
* In __properties panel__ find __Data__ . In data panel expand __Vertex Groups__ and click on __+__ button to add new vertex groups. Add two new vertex groups and name them 'Evergreen' and 'Deciduous', respectively. Click on Evergreen to set as active vertex group. 
* Now we select our desired vertices using __weight paint __ method 
* In the bottom header of the 3D view, find and change the  __Object interaction mode__ to __weight paint__  (hotkey __ctrl+Tab__)
* From the left toolbar change the weight to 1.00 
* Using the tree point clouds as spatial reference, paint over the surface to delineate the dense forested patches. Use ( __ctrl+Tab__) to toggle back and forth between _object mode_ and _weight paint mode to see the ortho photo and get idea about the exact location of the patches. switch to object mode after painting is finished. 
* Now select 'Deciduous' as active vertex group and repeat the process for the rest of the tress in the scene .  before 

__`Python Console >>>`__

> ######Clear existing vertex groups and add two new groups
`bpy.data.objects['elevation'].vertex_groups.clear()`
`bpy.data.objects['elevation'].vertex_groups.new(name='Evergreen')`
`bpy.data.objects['elevation'].vertex_groups.new(name='Deciduous')`
`bpy.data.objects['elevation'].vertex_groups.active_index=0`
>######Toggle weight paint mode and assign weight
`bpy.ops.paint.weight_paint_toggle()`
`bpy.context.scene.tool_settings.unified_paint_settings.weight = 1`

####4.1. Setup particle systems

* In the properties panel select __Modifiers__ tab. Then, click on the __Add Modifier__ drop down menu and select __particle system__.
* Now expand the modifier (using the button on the right side) to see the modifier parameters. Browse __Setting__ to find and select 'Evergreen'
* Scroll down to find the __Vertex Groups__ tab and expand it to see the parameters. In __Density__ field find and select 'evergreen'. Now should be able to see the trees populated in the painted areas.
* Repeat the same procedure for the 'Deciduous' vertex group , but this time select 'Deciduous' both in _particle setting_ and _Vertex group_ parameters.

__`Python Console >>>`__

>`DEM=bpy.data.objects['DEM']`<br />
`bpy.ops.object.particle_system_add() `<br />
`psys1 = DEM.particle_systems[-1]`<br />
`pset1 = psys1.settings`<br />
`pset1.name = 'Evergreen'`<br />
`pset1.use_dead=True`<br />
`pset1.frame_start = 1`<br />
`pset1.frame_end = 1`<br />
`pset1.lifetime = 50`<br />
`pset1.lifetime_random = 0.0`<br />
`pset1.emit_from = 'FACE'`<br />
`pset1.count=600`<br />
`pset1.use_render_emitter = True`<br />
`pset1.emit_from="FACE"`<br />
`pset1.use_emit_random=True`<br />
`pset1.userjit=70`<br />
`pset1.use_modifier_stack=True`<br />
`pset1.use_rotations=True`<br />
`pset1.render_type='OBJECT'`<br />
`pset1.dupli_object = bpy.data.objects["Evergreen"]`<br />
`pset1.use_rotation_dupli=True`<br />
`pset1.particle_size=.3`<br />
`pset1.size_random=.3`<br />
`pset1.rotation_mode='OB_Y'`<br />
`pset1.type = 'HAIR'`<br />
`psys1.vertex_group_density='Deciduous'`

### 5. Rendering

There two different ways to render scene in _Cycles render engine_. You can activate _Real time-rendering_ that is useful for quick previews or GPU rendering for final output. Note that the GPU rendering is by default setup to render only active camera. 

* In 3D_view bottom header find  __Shader option__ and select __Rendered__ . Interact with the 3D model to see how it is rendered on the-fly.
* Set one of the windows (scripting or node editor) to camera view. This can be done by selecting 3D view from bottom header of the window.  Then select the desired camera by right-clicking on the camera object in 3D view or selecting the object name (e.g, camera) in _outliner_. Then, set the 3D view to camera using __NUM0__.
* For final rendering push __F12__ after selecting the desired camera. For changing the render output and parameters, use the __Render__ tab in the __Properties__ panel. 

>__`Python Console >>>`__
>
>######Change the 3D view shader to rendered
`bpy.context.space_data.viewport_shade = 'RENDERED'`

>######Change the active camera to object 'Camera' and set 3D view window to camera . <br> Note: can be only executed from script.
>`cameraObj=bpy.data.objects['Camera']`<br />
 `bpy.context.scene.objects.active = cameraObj`<br />
` bpy.ops.view3d.object_as_camera() `
>######Render directly to the file path 
`bpy.context.scene.render.filepath = "D:\\Geospatial_studio\\Render\\"` <br />
>`bpy.ops.render.render() `



