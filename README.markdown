Summary (Version 0.6.4)
=======================

This software allows you to download and compile high-resolution vector topographic maps from the NSW geospatial data servers, covering much of NSW and the ACT. The resulting maps include many of the features found in the printed NSW topographic map series and are well-suited for printing. You can specify the exact extent of the area which you wish to map, as well as your desired scale (typically 1:25000). The topographic map is output in [scalable vector graphics](http://en.wikipedia.org/wiki/Scalable_Vector_Graphics) (SVG) format for use and further editing with vector graphics programs such as Inkscape or Illustrator. Other map formats including raster, KMZ and GeoTIFF can also be produced.

This software was originally designed for the production of rogaining maps and as such includes several extra features (such as aerial imagery overlays, marker layers for control checkpoints, arbitrary map rotation and magnetic declination marker lines). However the software is also useful for anyone wanting to create custom NSW topo maps for outdoor recreation.

A few limitations currently exist when using the software. In particular, *map data is not always available, particularly in populated areas*, due to caching performed by the map server. (Your map will be blank, or include blank tiles, if you encounter this limitation.)

Pre-Requisites
==============

The software is run as a script, so you will need some familiarity with the command line. It was developed on a Mac but has also been tested on Windows and Ubuntu Linux.

The following open-source packages are required in order to run the script:

* The [Ruby programming language](http://ruby-lang.org). You'll need the more recent Ruby 1.9.3, not 1.8.x.
* [ImageMagick](http://imagemagick.org), a command-line image manipulation tool. The latest ImageMagick at time of development is version 6.7.3. Only the 8-bit (Q8) version is needed and will work faster and with less memory than the 16-bit version, particularly for larger maps.
* The [GDAL](http://gdal.org) command-line utilities. These are utilities for processing geospatial raster data. Version 1.9.x (January 2012) or later is needed.
* [Inkscape](http://inkscape.org/) (a vector graphics editing program), if you wish to make manual edits or additions to your map, or to create secondary raster formats such as PNG, TIFF or KMZ.
* A zip command utility, if you wish to produce KMZ maps.

An image editing tool such as [GIMP](http://www.gimp.org/) or Photoshop may also be useful for creating a custom background canvas for your map.

The [Batik SVG toolkit](http://xmlgraphics.apache.org/batik/tools/rasterizer.html) can also be used as an alternative to Inkscape for rasterising your map (i.e. converting to an image file format such as PNG or TIF). It is recommended as it gives better results, but needs a [Java Runtime Environment](http://java.com/en/download/). (Your PC may already have Java; if not, installation is straightforward.)

Finally, a geographic viewing or mapping program such as [Google Earth](http://earth.google.com) or [OziExplorer](http://www.oziexplorer.com/) is very useful for easily specifying the area you wish to create a map for, and for viewing your resulting map in conjunction with GPS data.

* _Windows_:
  * A complete Ruby 1.9.3 installation for Windows can be [downloaded here](http://rubyinstaller.org/) (be sure to select 'Add Ruby executables to your PATH' when installing).
  * Download a pre-built [ImageMagick binary](http://www.imagemagick.org/script/binary-releases.php#windows) for Windows. The Q8 version is preferred for speed, but either will work. Be sure to select 'Add application directory to your system path' when installing.
  * Install the GDAL utilities using the [OSGeo4W](http://trac.osgeo.org/osgeo4w/) installer. Unless you want all the software offered by the installer, use the 'advanced install' option to install only GDAL. When presented with packages to install, select 'All -> Uninstall' to deselect everything, then open 'Commandline Utilites', choose 'Install' for the gdal package (some other required packages will also be selected), and install. Subsequently you should use the 'OSGeo4w Shell' as your command line when running nswtopo.rb.
  * (Other ways of obtaining Windows GDAL utilities are listed [here](http://trac.osgeo.org/gdal/wiki/DownloadingGdalBinaries#Windows), however not all of them include GDAL 1.9.x or above, including FWTools which was formerly recommended.)
  * Download and install [Inkscape](http://inkscape.org/download/).
  * (If you want to create KMZ maps, install [7-Zip](http://www.7-zip.org) and add its location, `C:\Program Files\7-Zip`, to your PATH following [these instructions](http://java.com/en/download/help/path.xml), using a semicolon to separate your addition.)
* _Mac OS X_:
  * ImageMagick and GDAL can obtained for Mac OS by first setting up [MacPorts](http://www.macports.org/), a package manager for Mac OS; follow [these instructions](http://guide.macports.org/chunked/installing.html) on the MacPorts site. After MacPorts is installed, use it to install the packages with `sudo port install gdal` and `sudo port install imagemagick +q8`
  * Alternatively, you can download and install pre-built binaries; try [here](http://www.kyngchaos.com/software:frameworks#gdal_complete) for GDAL, and the instructions [here](http://www.imagemagick.org/script/binary-releases.php#macosx) for ImageMagick. (This may or may not be quicker/easier than installing XCode and MacPorts!)
  * Type `ruby -v` in a terminal window to see whether a version 1.9.3 or greater Ruby already exists. If not, you can install Ruby 1.9.3 a number of ways, as explained [here](http://www.ruby-lang.org/en/downloads/). (If you are using MacPorts, `sudo port install ruby19 +nosuffix` should also work.)
  * Download and install Inkscape [here](http://inkscape.org/download/), or install it using MacPorts: `sudo port install inkscape`
* _Linux_: You should be able to install the appropriate Ruby, ImageMagick, GDAL, Inkscape and zip packages using your distro's package manager (RPM, Aptitude, etc).

You can check that the tools are correctly installed by using the following commands:

    ruby -v
    identify -version
    gdalwarp --version

You should receive version or usage information for each tool if it is installed correctly and in your path.

A large amount of memory is helpful. I developed the software on a 2Gb machine but it was tight; you'll really want at least 4Gb or ideally 8Gb to run the software smoothly. You will also need a decent internet connection. The topographic download won't use a lot of bandwidth, but the aerial imagery could amount to 100Mb or more for a decent-sized map. You'll want an ADSL connection or better.

## Fonts

A few point features of the map (camping grounds, picnic areas, mines, towers) use special ESRI fonts, namely 'ESRI Transportation & Civic', 'ESRI Environmental & Icons' and 'ESRI Telecom'. Some Microsoft fonts (e.g. Cambria) may also be used. These fonts will be listed if they are used in your map but missing from your system.

If you wish the fonts to display correctly (nice, but not essential), you need to obtain and install these fonts on your system, either by scrounging them from the internet, or as follows:
* obtain the ESRI fonts by installing [ArcGIS Explorer](http://www.esri.com/software/arcgis/explorer/index.html) and then downloading the [fonts expansion pack](http://webhelp.esri.com/arcgisexplorer/900/en/expansion_packs.htm) (Windows only).
* obtain the Microsoft fonts by having Microsoft products on your PC (ick). (Mac users can download the Microsoft Office trial, without installing it, and [extract the fonts](http://www.askdavetaylor.com/fix_missing_calibri_cambria_font_errors_iworks_numbers_pages.html).)

Usage
=====

The software can be downloaded from [github](https://github.com/mholling/nswtopo). It is best to download from the latest [tagged version](https://github.com/mholling/nswtopo/tags) as this should be stable. You only need to download the script itself, `nswtopo.rb`. Download by clicking the 'ZIP' button, or simply copying and pasting the script out of your browser.

You will first need to create a directory for the map you are building. Running the script will result in a various image files being downloaded, so a directory is needed to contain them.

Most likely, you will also want to create a map configuration file called `nswtopo.cfg` in order to customise your map. (This format of this file is [YAML](http://en.wikipedia.org/wiki/YAML), though you don't really need to know this.) This is a simple text file and can be edited with Notepad or whatever text editor you use. Save this file in your map directory as `nswtopo.cfg` (be sure not to use `nswtopo.cfg.txt` by mistake).

## Specifying the Map Bounds

The simplest way to create a map is to trace out your desired area using Google Earth (or other equivalent mapping program). Use the 'Polygon' tool to mark out the map area, then save this polygon in KML format as `bounds.kml` in the directory you created for your map. When running the script (see below), your bounds file will be automatically detected and a map produced using the default scale and settings. You can also specify the bounds file explicitly in your configuration file:

    bounds: bounds.gpx

If you are using a waypoints file to mark rogaine control locations, you can use the same file to automatically fit your map around the control locations. In this case you should also specify a margin in millimetres (defaults to 15mm) between the outermost controls and edge of the map:

    bounds: controls.kml
    margin: 15

(Using a track to specify the bounds will also work, and a 15mm margin will again be used by default.)

Alternatively, specify the map bounds in UTM by providing the UTM zone (54, 55 or 56 for NSW) and minimum and maximum eastings and northings, as follows:

    zone: 55
    eastings:
    - 730500
    - 741500
    northings:
    - 6014500
    - 6022500

or, as latitude/longitude bounds:

    latitudes: 
    - -35.951221
    - -35.892871
    longitudes: 
    - 149.383789
    - 149.489746

Finally, you can specify a single coordinate for the map's centre, and a physical size for the map at the scale you specify (1:25000 by default). The map size should be specified in millimetres. For example:

    zone: 55
    easting: 691750
    northing: 6070500
    size: 220 x 360

or,

    latitude: -33.474050
    longitude: 150.137979
    size: 60 x 60

(Make sure you get your map bounds correct the first time to avoid starting over with the downloads.)

## Running the Script

Once you have created your configuration file, run the script in the directory to create your map. The script itself is the `nswtopo.rb` file. The easiest way is to copy this file into your folder and run it from there thusly: `ruby nswtopo.rb`. Alternatively, keep the script elsewhere and run it as `ruby /path/to/nswtopo.rb`. By giving the script exec privileges (`chmod +x nswtopo.rb` or equivalent), you can run it directly with `./nswtopo.rb` (you may need to modify the hash-bang on line 1 to reflect the location of your Ruby binary).

When the script starts it will list the scale of your map (e.g. 1:25000), its rotation and its physical size. The size (in megapixels) and resolution of any associated rasters (e.g. aerial imagery) will also be displayed.

The script will then proceed to download the topographic data. Depending on your connection and the size of your map, an hour or more may be required. (I suggest starting with a small map, say 80mm x 80mm, just to familiarize yourself with the software; this should only take a few minutes.) It is best not to interrupt the program while the topographic data is downloading, as you will have to start over.

You can ctrl-c at any point to stop the script. Files which have already downloaded will be skipped when you next execute the script. Conversely, deleting an already-created file will cause that file to be recreated when you run the script again.

After all files have been downloaded, the script will then compile them into a final map image in `.svg` format. The map image is easily viewed in a modern web browser such as Chrome or Firefox, or edited in a vector imaging tool like Inkscape or Illustrator.

You will likely want to tinker with the configuration file to change the appearance of your final map. To rebuild your map after changing the configuration, simply delete `map.svg` (or whatever name you've configured) and run the script again. The map will be recreated from the intermediate files which have been downloaded.

Map Configuration
=================

By editing `nswtopo.cfg` you can customise many aspects of your map, including which additional layers to include. If no other configuration is provided, reasonable defaults are used. The customisation options are shown below with their default values. (*It is not necessary to provide these default values in your configuration file.*)

Set the scale of the map as follows:

    scale: 25000              # desired map scale (1:25000 in this case)

Set the map rotation angle as an angle between +/- 45 degrees anticlockwise from true north (e.g. for a rotation angle of 20, true north on the map will be 20 degrees to the right of vertical). There is no degradation in quality in a rotated map (although horizontal labels will no longer be horizontal). The special value `magnetic` will cause the map to be aligned with magnetic north.

    rotation: 0               # angle of rotation of map (or 'magnetic' to align with magnetic north)

Another special value for rotation is `auto`, available when the bounds is specified as a `.kml` or `.gpx` file. In this case, a rotation angle will be automatically calculated to minimise the map area. This is useful when mapping an elongated region which lies oblique to the cardinal directions.

    rotation: auto            # rotate the map so as to minimise map area

Set the filename for the output map and related files.

    name: map                 # filename to use for the final map image

Additional Layers
=================

Any or all of the following additional layers can be included in your map by listing them in the `include` option in your `nswtopo.cfg` file. (At the very least, you will want to include `grid` for a normal map or `declination` for a rogaining map. The `relief` layer is also recommended, and in many cases the `vegetation` layer.)

    include:
    - aerial-lpi-eastcoast
    - aerial-lpi-ads40
    - aerial-google
    - aerial-nokia
    - aerial-best
    - reference-topo-s1
    - reference-topo-s2
    - reference-topo-current
    - canvas
    - vegetation
    - plantation
    - holdings
    - relief
    - grid
    - declination
    - controls

Viewing the map in Inkscape allows you to toggle individual layers on and off. This if helpful, for example, if you wish to view the aerial imagery superimposed against the topographic feature layers for comparison.

## Aerial Imagery

These are orthographic aerial images for the specified map area, derived from Google Maps, Nokia Maps, and the NSW LPI department. Depending on your map location there may be up to four different aerial images available.

These layers are very useful for confirming the accuracy of the topographic features. For example, you may be able to manually add firetrails, new dams, etc, which are missing from the NSW map layers, on the basis of what you can see in the aerial imagery. Since the images are correctly georeferenced, this is achieved simply by tracing out the extra information on the appropriate layer while viewing the aerial imagery underneath. (Another excellent use for these aerial imagery layers is to produce your own vegetation layer for a rogaine map. This is described below in the "canvas" section.)

* `aerial-best`: A mosaic of the best NSW LPI imagery
* `aerial-lpi-ads40`: Recent, high resolution imagery available from the NSW LPI; available for many but not all areas of interest 
* `aerial-lpi-eastcoast`: medium resolution imagery for most of the 25k topographic coverage; quite old film imagery (from the 90s?)
* `aerial-google`: generally good quality, recent aerial imagery from Google Maps; limited to 250 tiles per six hour period
* `aerial-nokia`: reasonable quality aerial imagery from Nokia Maps; limited to 250 tiles per six hours; georeferencing is not always the best

Each of these images downloads at a default resolution, typically 2.0 metres per pixel. (This indicates that one pixel in the image represents two metres on the ground.) You can override the default by specifying a different resolution for the image:

    include:
    - aerial-best: 0.5

Depending on the native resolution of the dataset, you may or may not obtain a more detailed image by specifying a better resolution. A lot of the `aerial-best` imagery has a native resolution of 0.5 m/px, and can yield very a very detailed image. However, for a map of reasonable size, the image produced at this resolution can be extremely large (easily 100+ megapixels)!

If you wish to just download all the aerial images at their default resolutions, simply specify the shortcut `aerial` in your your include list.

## Reference Topo Maps

These layers (`reference-topo-s1`, `reference-topo-s2` and `reference-topo-current`, or simply `reference` as shortcut) contain lower-resolution topographic map raster images at various points in time (oldest, older and current, respectively). They are useful to have as a reference for comparison against the output of this software.

## Vegetation

**N.B. The vegetation dataset changed in version 0.6.4. Ensure you have the correct v2 dataset.**

The vegetation layer in standard NSW printed topo sheets appears to be derived from a dataset called *NSW Interim Native Vegetation Extent (2008-v2)*, which is a 25-metre resolution raster representation of NSW, categorised into 'woody' and 'non-woody' vegetation. For our purposes this generally corresponds to forested and open areas on our map.

This vegetation data is not available from a map server, but the entire 162 MB dataset may be downloaded from [here](http://mapdata.environment.nsw.gov.au/geonetwork/srv/en/metadata.show?id=246) (you will need to provide your name and email address). You need only download this once as the same data is used for any maps you create.

Once you have downloaded the data, unzip the file to a suitable location, locate the file named `hdr.adf` and add its path (relative or absolute) to your configuration file. (You can also modify the default colours for woody and non-woody vegetation, should you wish.)

    vegetation:
      path: /Users/matthew/nswtopo/NSWInterimNativeVegetationExtentV2_2008/Data/nswintext08/hdr.adf
      colour:
        woody: "#C2FFC2"      # a light pastel green
        non-woody: white

Finally, add `vegetation` to your list of layers to include, and build or rebuild your map to view the resulting vegetation underlay.

## Canvas

If you include a PNG image named `canvas.png` in the map directory, it will be automatically detected and used as an underlay for the map. It is intended that this map canvas should be derived from one of the aerial images using an image editing tool such as Photoshop or GIMP, in order to represent vegetation cover. This is useful in case you are not satisfied with the vegetation layer provided above.

Generating your own vegetation layer can be accomplished using the 'color range' selection tool in Photoshop, for example, or other similar selection tools. (Selecting on a single channel, such as green or magenta, may be helpful.) You can also create additional vegetation markings (e.g. for the distinctive, nasty heath that sometimes appears in ACT rogaines) using the aerial imagery.

If you wish to create your canvas at a lower resolution, it is fine to resample (resize) the aerial image to smaller pixel count before selecting and colouring the vegetation. However, it is important that you *resample the image by changing its resolution* (usually shown in pixels/inch), rather than by changing the width and height.

## Plantations

If you include the `plantation` layer, a representation of pine forest plantations will be added to your map in darker green. The accuracy of this layer is not guaranteed however.

## Holdings

The `holdings` layer overlays property boundaries and the names of landowners. This information may be useful to rogainers when planning a course. (No information is available for the ACT.)

## Relief

By including the `relief` layer in your map, you can include an intuitive [shaded-relief](http://en.wikipedia.org/wiki/Cartographic_relief_depiction#Shaded_relief) depiction. This can be a helpful addition for quickly assessing the topography represented in a map. The shaded relief layer is automatically generated from the ASTER digital elevation model at 45 metre resolution.

You can specify the azimuthal angle, altitude and terrain exaggeration used to generate the shaded relief layer. (The conventional azimuth angle of 315 is set as default, should probably be left as is.) You can also specify the opacity of the layer to change the aggressiveness of the effect.
  
    relief:
      azimuth: 315            # azimuth angle for shaded relief layers (degrees clockwise from North)
      altitude: 45            # angle of illumination from horizon (45 is standard)
      exaggeration: 1         # vertical exaggeration factor
      opacity: 0.3            # opacity of the shaded relief; determines how subtle the effect is

The shaded relief is derived from low-resolution (45 metres per pixel) elevation data, and is embedded directly into the map at that resolution. Most SVG rendering engines correctly scale up such low-resolution data, producing a natural-looking, smooth gradient. However some software (Inkscape in particular) renders this data in a blocky, pixelated fashion. If you are using Inkscape and are unhappy with this appearance, you can instead download the relief layer at a higher resolution - say, 10.0 metres per pixel. (The compromise here is a significant increase in the SVG file size)

    relief:
      resolution: 10.0

## UTM Grid

The `grid` layer marks the UTM grid onto the map, for use with a GPS or to give grid references from the map. You should include the UTM grid if the map is for normal use; for a rogaining map, the grid is not needed and clutters up the map, so leave it out.

You can customise the appearance of the UTM grid and labels if you desire:

    grid:
      interval: 1000          # interval between grid lines (1000 metres by default)
      width: 0.1              # width in millimetres of the marked lines on the map
      colour: "#000000"       # colour of the gridlines as a hex triplet (black by default)
      label-spacing: 5        # number of gridlines between successive labels
      fontsize: 7.8           # font size (in points) of the grid labels
      family: Arial Narrow

## Declination

This layer marks magnetic north lines on the map, and is useful for rogaining maps. The magnetic declination angle for the map centre is automatically retrieved from the Geoscience Australia website. (Override by specifying an `angle: ` value.) Specify spacing and rendering for the magnetic declination lines. 

    declination:
      spacing: 1000           # perpendicular spacing of magnetic declination lines in metres
      width: 0.1              # width of the marked lines on the map, in millimetres
      colour: "#000000"       # colour of the magnetic declination lines as a hex triplet (black by default)

## Controls

Drop a control waypoints file (`controls.kml` or `controls.gpx`) into the directory and a layer containing control circles and numbers will be automatically generated. If a waypoint is name 'HH' it will be drawn as a triangle, otherwise a circle will be drawn. If a control has 'W' after its number (e.g. '74W'), or separate waypoints marked 'W1', 'W2' etc are found, those waypoints will be represented as water drops. You can modify any of the defaults shown below:

    controls:
      file: controls.kml      # filename (.kml or .gpx format) of control waypoint file
      fontsize: 14            # font size (in points) for control labels
      diameter: 7.0           # diameter of control circles in millimetres
      thickness: 0.2          # thickness of control circles in millimetres
      colour: "#880088"       # colour of the control markers and labels (as a hex triplet or web colour)
      water-colour: blue      # colour of waterdrop markers

Overlays
========

You can add overlays to your map for representing areas (polygons) and tracks (paths). For rogaine maps, you can use this feature to mark out-of-bounds areas on your map, as well as to add extra tracks which are not shown on the topographic map.

The simplest way to create overlays is to use Google Earth or equivalent software. Mark out the out-of-bounds area or areas using the polygon tool, then save these areas to a KML file (e.g. `boundaries.kml`). Similarly, trace out additional unmarked tracks using the path tool, and save them as KML (e.g. `tracks.kml`).

In your configuration file, add your overlay files and specify their colours, opacities and/or widths as follows:

    overlays:
      boundaries.kml:
        colour: black         # colour out-of-bounds areas in black...
        opacity: 0.3          # ...with 0.3 opacity to give a nice grayed-out rendering
      tracks.kml:
        colour: red           # mark tracks in red...
        width: 0.2            # ...with a width of 0.2mm

Build or rebuild your map by running the script to add the overlays. (Advanced users may alter the overlay rendering further using Inkscape, e.g. by adding dashes or dots to tracks or patterns to areas.)

Output Formats
==============

Once your master map file has been created in SVG format, you can create other output formats by specifying their file extensions from among the following in your configuration file:

    formats:
    - png
    - gif
    - jpg
    - tif
    - kmz
    - pdf
    - map
    - prj

These file extensions produce the following file formats:

* `png`, `gif` and `jpg` are common raster image formats. PNG is recommended. JPG is not recommended, as it not suited to line art and produces ugly artefacts.
* `tif` yields a TIFF image, a raster format commonly required by print shops. Additional [GeoTIFF](http://en.wikipedia.org/wiki/GeoTIFF) metadata is also included in the image, allowing it to be used in any GIS software which supports GeoTIFF.
* `kmz` is a map format used with [Google Earth](http://earth.google.com) and for publishing interactive maps on the web. (This would be useful for publishing a rogaine map with NavLight data.)
* `pdf` is the well-known document format. Map data will be preserved in vector form within the PDF.
* `map` specifies the [OziExplorer](http://www.oziexplorer.com/) map file format (using the PNG image as the companion raster).
* `prj` produces a simple text file containing the map's projection as a [PROJ.4](http://trac.osgeo.org/proj/) string.

If you make manual edits to the master SVG map, you can regenerate any other output formats you have specified simply by deleting those files, then running the script again to recreate them.

The raster image formats (PNG, GIF, JPG, TIFF and KMZ) will render at 300 pixels-per-inch (ppi) resolution by default. You can easily override this default however. For example, say a high-resolution, 600-ppi TIFF is desired for printing, and a more modest 200-ppi KMZ for publishing on the web:

    formats:
    - tif: 600
    - kmz: 200

You can also specify an output resolution for the PDF format, in which case the PDF will render as an embedded raster image (instead of vector data):

    formats:
    - pdf: 600

(This option will likely produce a larger PDF file, but guarantees the map's final appearance; it should be considered if the map is being sent as PDF to a print shop.)

Finally, projection formats other than PROJ.4 are available. Any of the [format options listed here](http://www.gdal.org/gdalsrsinfo.html) may be specified; for example, the projection may be desired in [well-known text](http://en.wikipedia.org/wiki/Well-known_text) format:

    formats:
    - prj: wkt

If you select `prj` as an output, a corresponding [ESRI world file](http://en.wikipedia.org/wiki/World_file) will be produced for each raster image. The world file may be used in conjunction with the projection file to georeference the image. (World file extensions for PNG, GIF, JPG and TIFF are `.pgw`, `.gfw`, `.jgw` and `.tfw`, respectively.)

## Producing Raster Images

To produce your map in PDF or any raster format (PNG, GIF, JPG, TIFF, KMZ), you must install either [Inkscape](http://inkscape.org/) or the [Batik SVG toolkit](http://xmlgraphics.apache.org/batik/download.cgi) (get the binary distribution). Then set your configuration file as follows:

* To use Inkscape for rasterising:

        rasterise: inkscape

  If your command line does not recognise the `inkscape` command, you may need to specify the full path. e.g. for the Mac:

        rasterise: /Applications/Inkscape.app/Contents/Resources/bin/inkscape

  (Or the corresponding `C:\Program Files\Inkscape\inkscape.exe` path in Windows.)

* To use the Batik SVG rasteriser, specify the path where you've downloaded and unzipped the Batik binary distribution:

        rasterise: /Users/matthew/nswtopo/batik-1.7

If possible, the use of Batik is recommended as it produces nicer results. In particular, it scales up the low-resolution vegetation and shaded-relief layers to produce nice, smooth variations. (On the other hand, Inkscape renders large, blocky pixels when rendering the vegetation and shaded-relief layers. This looks somewhat strange and could suggest more accuracy in the vegetation layer than in fact exists. Inkscape also renders some area elements incorrectly.)

If you are building a large map and encounter an `OutOfMemory` error, this means that Batik needs more more memory while rendering the map. You can fix this by adding your own [memory flags](http://docs.oracle.com/javase/6/docs/technotes/tools/solaris/java.html) to the java call. For example, to use 2Gb (2048Mb) of memory (the largest my java installation would allow), specify the following in your configuration file:

    java: java -Xmx2048M

Suggested Workflow for Rogaining Maps
=====================================

Here is a suggested workflow for producing a rogaine map using this software (alter according to your needs and tools):

1.  Set out the expected bounds of your course using the polygon tool in Google Earth, and save as `bounds.kml`. (Set the style to partially transparent to make this easier.)

2.  Select all the topographic, aerial and reference layers, as well as landholdings, using the following configuration:

        name: rogaine
        bounds: bounds.kml
        include:
        - aerial
        - reference
        - vegetation
        - declination
        - holdings
        - grid

3.  Run the script to create your preliminary map, `rogaine.svg`.

4.  Use the maps and aerial images to assist you in setting your rogaine. Ideally, you will carry a GPS with you to record waypoints for all the controls you set.

5.  When you have finalised your control locations, use Google Earth to create a `controls.kml` file containing the control waypoints. This can either be directly from your GPS by uploading the waypoints, or by adding the waypoints manually in Google Earth.

6.  In Google Earth, mark out any boundaries and out-of-bounds areas using the polygon tool, and save them all to a `boundaries.kml` file.

6.  You'll most likely want to recreate your map with refined boundaries. Delete the files created in step 2 (topographic.svg, rogaine.svg, aerial-\*.jpg, reference-\*.png etc), and modify your configuration to set the bounds using your controls file. Include only the layers you need for the printed map.

        name: rogaine
        bounds: controls.kml    # size the map to contain all the controls ...
        margin: 15              # ... with a 15mm margin around the outer-most controls
        rotation: magnetic      # align the map to magnetic north
        include:
        - vegetation            # show vegetation layer (or use a canvas)
        - declination           # show magnetic declination lines
        - controls              # show controls
        - relief                # show shaded relief (optional)
        overlays:
          boundaries.kml:       # mark out-of-bounds areas
            colour: black       # (in black)
            opacity: 0.3        # (only partially opaque)

    If you have trouble fitting your map on one or two A3 sheets, you can either reduce the margin or use the automatic rotation feature (`rotation: auto`) to minimise the map area.

7.  Run the script to re-download your map using the final map bounds. Your map should now show vegetation, magnetic declination lines and the controls and boundaries you have set.

9.  Add any other information you need (title, acknowledgements, logo etc.) to the map using a vector graphics editor (Inkscape, Illustrator). At this point you can make any desired tweaks to topographic features of the map (e.g. contours or watercourses) to more accurately represent what you've found on the ground.

10. Prepare your output file for the printers. It is possible to save directly to a PDF, however I recommend instead exporting your map to a high-resolution raster image (TIFF is traditional). Exporting to a raster lets you ensure the map has rendered correctly. A resolution of 300 dpi would suffice, however there is no harm in going higher (say 600 dpi).

Georeferencing
==============

The map projection used is transverse mercator, with a central meridian corresponding to the map's centre. This conformal projection is ideal for topographic maps. A grid for the relevant UTM zone(s) (usually zone 55 or 56) can be added to the map (for use with a GPS) by including the UTM grid layers. All output layers (including the aerial imagery and shaded relief layers) are precisely aligned and in the same projection.

If you need your map to be in a UTM projection (aligned to grid north rather than true north), you can specify this as follows:

    utm: true

Several formats of georeferenced output image are available. You can specify `tif` in the formats list to create a GeoTIFF image for the map. GeoTIFFs can be used by many commercial and open-source GIS software (e.g. [QGIS](http://www.qgis.org/), [Grass](http://grass.fbk.eu/)). [OziExplorer](http://www.oziexplorer.com/) map files can also be created by specifying `map` as an output format. Finally, `kmz` will produces a KMZ file suitable for use with Google Earth.

Customising Topographic Rendering
=================================

You can control how the raw topographic data (`topographic.svg`) is rendered into you final map. This allows you to change the colour, size and opacity of individual layers. The default rendering was chosen to give a reasonable map with emphasis on contours, and changes to rendering may not be needed.

To change rendering of a feature, open `topographic.svg` and identify the name of the topographic layer (e.g. `Gates_Grids`, `Contour_10m`) containing the feature. (The following labels for groups of related layers are also available: contours, water, pathways, roads, cadastre, labels).

Next, add a `render:` section to your `nswtopo.cfg` file. For each layer which you wish to modify, specify one or more of `opacity`, `expand` and `colour` values to change the opacity, scaling and colour, respectively, of the features in that layer. Colours should be specified as hex triplets (e.g. `"#FF0000"` for red). Use a [colour picker](http://www.google.com/search?q=color+picker) to choose your desired colour and get its hex triplet. `opacity` should be a value between 0.0 and 1.0. The `expand` value should specify how much the feature's size is reduced (expand < 1.0) or enlarged (expand > 1.0) compared to the raw map.

The following shows the default renderings and illustrates the format to be used. Any renderings you specify in your `nswtopo.cfg` will override these defaults. Note that `colour` may specify a single colour for the whole layer (e.g. the cadastre layer below), or a mapping of specific colours to new colours (e.g. the labels layer below). (The latter is useful if the original layer contains more than one colour; you will need to identify the colours to be changed using Inkscape.)

    render:
      plantation:
        opacity: 1
        colour: "#80D19B"
      pathways:
        expand: 0.5
        colour: 
          "#A39D93": "#363636"
      contours:
        expand: 0.7
        colour: 
          "#D6CAB6": "#805100"
          "#D6B781": "#805100"
      roads:
        expand: 0.6
        colour:
          "#A39D93": "#363636"
          "#9C9C9C": "#363636"
      cadastre:
        expand: 0.5
        opacity: 0.5
        colour: "#777777"
      labels: 
        colour: 
          "#A87000": "#000000"
          "#FAFAFA": "#444444"
      water:
        opacity: 1
        colour:
          "#73A1E6": "#4985DF"
      Creek_Named:
        expand: 0.3
      Creek_Unnamed:
        expand: 0.3
      Stream_Named:
        expand: 0.5
      Stream_Unnamed:
        expand: 0.5
      Stream_Main:
        expand: 0.7
      River_Main:
        expand: 0.7
      River_Major:
        expand: 0.7
      HydroArea:
        expand: 0.5
      PointOfInterest:
        expand: 0.6
      Tourism_Minor:
        expand: 0.6
      Gates_Grids:
        expand: 0.5
      Beacon_Tower:
        expand: 0.5
      holdings:
        colour:
          "#B0A100": "#FF0000"
          "#948800": "#FF0000"

As an example, to make the contours black, the roads thinner and the cadastre lines thicker and darker, add the following to your map configuration file:

    render:
      contours:
        colour: "#000000"
      roads:
        expand: 0.4
      cadastre:
        opacity: 1.0
        expand: 0.8
        colour: "#202020"

Release History
===============

* 12/12/2011: version 0.1 (initial release)
  * 13/12/2011: version 0.1.1: added bridges, floodways, fixed narrow gaps in roads
  * 14/12/2011: version 0.1.2: reworked UTM grid to display correctly across zone boundaries
* 21/12/2011: version 0.2: added map rotation; added specification of map bounds via gpx/kml file; added ability to auto-rotate map to minimise area.
* 11/01/2012: version 0.3: misc. additions (e.g. lookouts, campgrounds, rock/pinnacle labels, etc); collected point markers into single layer; separated permanent and intermittent water layers; prevented label/feature overlap; decreased download times; removed unavailable ACT layers; added low-res reference topo.
* 2/2/2012: version 0.4: added ferry routes, mangroves, restricted areas, canals, breakwaters, levees, road outlines; tweaked road & track colours; added grid-style UTM labels; removed absolute path from OziExplorer .map file; fixed bug wherein resolution tags in some output images were incorrectly set.
  * 8/2/2012: version 0.4.1: fixed bug whereby excluding labels also excluded control-labels
  * 9/2/2012: version 0.4.2: added kmz as output format
  * 13/2/2012: version 0.4.3: reworked road/track colours and outlines
  * 7/3/2012: version 0.4.4: fixed bug in OziExplorer .map files created by non-windows OS; added layer opacity; added overlay layers from GPX/KML/etc files
* 3/6/2012: version 0.5: Substantial rewrite to use the new NSW ArcGIS server
  * 4/6/2012: version 0.5.1: added metadata to identify layers correctly to Inkscape
* 30/6/2012: version 0.6: fixed bug with shaded-relief and vegetation layers on some versions of ImageMagick; added option for default nswtopo.cfg file stored in nswtopo.rb location; improved SVG topographic labels layer; added other output formats: .png, .tif, .kmz, .pdf, .prj, .wld, .map; added rendering using Inkscape or Batik; switched from 0.9996 to 1.0 for transverse mercator scale factor; changed config.yml to nswtopo.cfg; added configurations for individual output raster dpis and input raster resolutions
  * 5/6/12: version 0.6.1: fixed vegetation rendering bug on linux; added time remaining estimations; bugfixes; added fix for java OutOfMemory error when using Batik
  * 5/8/12: version 0.6.2: fixes to restore Windows compatibility and update Windows installation instructions
  * 4/10/12: version 0.6.3: changed old LPIMAP layer names to new LPIMAP layer names; added the option of specifying a map bound using a track; fixed problem with ESRI SDS 1.95 1 font; fixed bug with KMZ generation; fixed broken cadastre layer; fixed broken holdings layer
  * 25/9/13: version 0.6.4: fixed aerial-best, paths and holdings layers; expanded and renamed reference topo layers; updated vegetation layer to use v2 dataset.
