CollectiveAccess/Historic Images, New Technologies (HINT) jQuery Tileviewer
===========================================================================

A jQuery plugin implementing an HTML5 viewer for high resolution imagery. Only the on-screen portion of the image is loaded, at the minimum resolution suitable for the zoom level. This allows even extremely large images to load quickly and be examined in detail at full resolution with fluid panning and zooming.

Before an image can be used with Tileviewer it must be broken down into square chunks ("tiles") at various resolutions. The viewer will load these tiles as needed to form the image. While are several tiled image formats in use, Tileviewer currently only supports [Tilepic] (http://docs.collectiveaccess.org/wiki/TilePic), a format developed at the University of California, Berkeley library.

Tileviewer was originally developed for [CollectiveAccess](http://www.collectiveaccess.org), open source collections management software for museums and archives. The version is this repository is extended to fulfill requirements of the Historical Society of Pennsylvania's [Historic Images, New Technologies](https://hsp.org/history-online/historic-images-new-technologies) (HINT) project, with support from the [National Historical Publications and Records Commission](http://www.archives.gov/nhprc/) (NHPRC). All of the changes in this version are also available in the latest [CollectiveAccess version](https://github.com/collectiveaccess/providence).

Features
--------
Tileviewer is far from the only zooming tiled image viewer out there. There's [OpenSeaDragon](https://openseadragon.github.io), [Zoomify](http://www.zoomify.com), [PanoJS](http://www.dimin.net/software/panojs/) and others. Tileviewer has the basic features you'll see in all of them:

*  Smooth pan and zoom over images of virtually any size.
*  A navigator indicating the on-screen portion of the image.
*  A quick way to zoom out to show the entire image.
*  HTML5-based. Does not require Flash or any other browser plugin. Is compatible with all commonly used web browsers (Chrome, Firefox, Safari, Internet Explorer).

In addition, Tileviewer also includes:

*  Annotation system with support for interactively defined point, rectangle and polygon regions. Each annotation may have an independently placed text box and an optional entry form for an arbitrary set of extended set. Extended data makes possible richly catalogued annotations, as is done in the HINT project. An optional annotation list facilitates navigation between annotated regions.
*  Tools for adding measurement annotations and specifying the scale of an image in physical units. This allows one to assert the size of a specific image feature, then use that scale to derive the length of other features, as is done in the [iDigPaleo](http://www.idigpaleo.org) project.
*  Live rotation of images. 

Installation
------------
To come

Dependencies
------------

The following javascript libraries are used by Tileviewer and must be loaded before a viewer is initialized:

*  JQuery 1.7+
*  JQuery.HotKeys
*  Brandon Aaron's (http://brandonaaron.net) mousewheel jquery plugin 3.0.3
*  Circular-Slider (https://github.com/princejwesley/circular-slider)
*  jQuery-UI 1.9+

Options
------- 

#### id
> DOM ID to set internal viewer canvas to [Default: tileviewer]

#### empty
> color of empty (loading) tile, as hex color, if no subtile is available [Default: #cccccc]

#### src
> ?

#### width
> Canvas width (not image width) [Default: 400px]

#### height
> Canvas height (not image height) [Default: 400px]

#### zoomSensitivity
>  ? [Default: 16]

#### thumbnail
>  Display thumbnail? [Default: false]

#### debug
>  ? [Default: false]

#### grabberSize
>  ? [Default: 12]

#### maximumPixelsize
>  Set this to > 1 if you want to let user to zoom image larger than 100% [Default: 4]

#### thumbDepth
>  ? [Default: 2]

#### toolbar
>  List of tools, in order, to display on toolbar. This can be used to control what tools are available or reorder the toolbar. The value is a list of strings, one for each tool. Tools include *zoomIn*, *zoomOut* (image zoom controls), *pan* (pan image by dragging), *toggleAnnotations* (show/hide annotations), *rect* (create rectangular annotation), *point* (create point annotation), *polygon* (create polygon annotation), *measure* (create measurement annotation), *lock* (prevent changes to annotations),  *overview* (toggle visibility of image navigator), *rotation* (toggle visibility of image rotation panel), *expand* (force image to fit to screen), *list* (toggle visibility of annotation list), *download* (download image), *help* (toggle visibility of help text), *key* (toggle visibility of annotation color key). You can also use *separator*  to add a horizontal rule to separate two regions of the toolbar. [Default: 
	['zoomIn', 'zoomOut', 'pan', 'toggleAnnotations', 'rect', 'point', 'polygon', 'measure', 'lock', 'separator',  'overview', 'rotation', 'expand', 'separator', 'list', 'download', 'help', 'key']
]

#### toolbarIcons
>  Specifies icon to use for each toolbar tool. Keys are tool strings as defined for the *toolbar* option. Values are the HTML used to render the icon. By default [Font-Awesome](https://fortawesome.github.io/Font-Awesome/) icons are used. If you are not loading Font-Awesome on the page including your viewer you will have to override these to use other icons.  [Default: 
	{
		'pan': '<i class="fa fa-arrows">',
		'toggleAnnotations': '<i class="fa fa-eye"></i>', 
		'rect': '<i class="fa fa-square-o"></i>', 
		'point': '<i class="fa fa-circle-thin"></i>', 
		'polygon': '<i class="fa fa-share-alt"></i>', 
		'measure': '<i class="fa fa-text-width"></i>', 
		'lock': '<i class="fa fa-lock"></i>', 
		'overview': '<i class="fa fa-picture-o"></i>', 
		'rotation': '<i class="fa fa-undo"></i>', 
		'expand': '<i class="fa fa-expand"></i>', 
		'list': '<i class="fa fa-bars"></i>', 
		'download': '<i class="fa fa-download"></i>', 
		'help': '<i class="fa fa-life-ring"></i>', 
		'key': '<i class="fa fa-key"></i>',
		'zoomIn': '<i class="fa fa-search-plus"></i>',
		'zoomOut': '<i class="fa fa-search-minus"></i>'
	}
]

Demonstration
-------------
An example of the viewer is in the *example/* folder in the GitHub repository. You can also see the example at http://demo.collectiveaccess.org/ImageViewerExample/

Credits
-------
Forked from Soichi Hayashi's excellent jquery-tileviewer plugin (currently available at https://github.com/soichih/jquery-tileviewer) back in 2011, before it was on Github :-)

Developed with support from the National Science Foundation and National Historical Publications and Records Commission, in collaboration with Stony Brook University (State University of New York), Historical Society of Pennsylvania and the Yale Peabody Museum.

TODO
----
*  Reduce or eliminate the number of dependencies; in particular eliminate the dependency on jQuery-UI
*  Add support for tiled image formats other than Tilepic
*  Reimplement touch event support (stripped from current code)
*  Make available via Bower, et al.

Known Issues
------------
*  Detection of mouse over on annotations is skewed and incorrect when image is rotated

License
-------
Tileviewer is made available under the [GNU Public License version 3](http://www.gnu.org/licenses/gpl-3.0.en.html).