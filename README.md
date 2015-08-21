CollectiveAccess/Historic Images, New Technologies (HINT) jQuery Tileviewer
===========================================================================

A jQuery plugin implementing an HTML5 viewer for high resolution imagery. Only the on-screen portion of the image is loaded, at the minimum resolution suitable for the zoom level. This allows even extremely large images to load quickly and be examined in detail at full resolution with fluid panning and zooming.

Before an image can be used with Tileviewer it must be broken down into square chunks ("tiles") at various resolutions. The viewer will load these tiles as needed to form the image. While are several tiled image formats in use, Tileviewer currently only supports [Tilepic] (http://docs.collectiveaccess.org/wiki/TilePic), a format developed at the University of California, Berkeley library.

Tileviewer was originally developed for [CollectiveAccess](http://www.collectiveaccess.org), open source collections management software for museums and archives. The version in this repository is extended to fulfill requirements of the Historical Society of Pennsylvania's [Historic Images, New Technologies](https://hsp.org/history-online/historic-images-new-technologies) (HINT) project, with support from the [National Historical Publications and Records Commission](http://www.archives.gov/nhprc/) (NHPRC). All of the changes in this version are also available in the latest [CollectiveAccess version](https://github.com/collectiveaccess/providence).

Features
--------
Tileviewer is far from the only zooming tiled image viewer out there. There's [OpenSeaDragon](https://openseadragon.github.io), [Zoomify](http://www.zoomify.com), [PanoJS](http://www.dimin.net/software/panojs/) and others. Tileviewer has the basic features you'll see in all of them:

*  Smooth pan and zoom over images of virtually any size.
*  A navigator indicating the on-screen portion of the image and providing quick access to other regions.
*  Convenient zoom out to show the entire image.
*  HTML5-based. Does not require Flash or any other browser plugin. Is compatible with all commonly used web browsers (Chrome, Firefox, Safari, Internet Explorer).

In addition, Tileviewer also provides:

*  Annotation system with support for interactively defined point, rectangle and polygon regions. Each annotation may have an independently placed text box and an optional entry form for an arbitrary set of extended metadata. Extended metadata makes possible richly catalogued annotations, as is done in the HINT project. An optional annotation list facilitates in-viewer search and quick navigation between annotated regions.
*  Tools for adding measurement annotations and specifying the scale of an image in physical units. This allows one to assert the size of a specific image feature, then use that scale to derive the length of other features, as is done in the [iDigPaleo](http://www.idigpaleo.org) project.
*  Live rotation of images. 

Demonstration
-------------
An example of the viewer is in the *example/* folder in the GitHub repository. You can also see the example at http://demo.collectiveaccess.org/ImageViewerExample/

Installation
------------
To use Tileviewer in a page you first need to pull in jQuery and several jQuery plugins:

*  [JQuery 1.7+](http://jquery.com)
*  [JQuery.HotKeys](https://github.com/jeresig/jquery.hotkeys)
*  [Brandon Aaron's](http://brandonaaron.net) mousewheel jquery plugin 3.0.3
*  [Circular-Slider](https://github.com/princejwesley/circular-slider)
*  [jQuery-UI 1.9+](https://jqueryui.com)
*  [ca.genericpanel.js](https://github.com/collectiveaccess/providence/blob/master/assets/ca/ca.genericpanel.js) *only needed if extended annotation metadata editing is required*

To use the viewer with default toolbar icons you'll also need to load [Font-Awesome](https://fortawesome.github.io/Font-Awesome/). If you are using your own icons then you won't need Font-Awesome, but you will need to set the *toolbarIcons* and *uiIcons* viewer options (see below).

Next pull in *jquery.tileviewer.js* and *jquery.tileviewer.css*.

The &lt;head&gt; of your page might look something like this:

	<head>
		<meta charset="utf-8" />
		<title>jQuery TileViewer Example</title>
		<link rel="stylesheet" href="css/circular-slider.css" type="text/css" media="screen" />
		<link rel='stylesheet' href='js/jquery-ui/smoothness/jquery-ui-1.9.2.custom.css' type='text/css' media='all'/>
		<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css">
		<link rel="stylesheet" href="css/jquery.tileviewer.css" type="text/css" media="screen" />
		<script src='js/jquery.js' type='text/javascript'></script>
		<script src='js/jquery.hotkeys.js' type='text/javascript'></script>
		<script src='js/jquery.mousewheel.js' type='text/javascript'></script>
		<script src='js/circular-slider.js' type='text/javascript'></script>
		<script src='js/jquery-ui/jquery-ui-1.9.2.custom.min.js' type='text/javascript'></script>
		<script src='js/jquery.tileviewer.js' type='text/javascript'></script>
	</head>

Once everything is loaded you can use Tileviewer as you would any other jQuery plugin. Create a &lt;div&gt; on the page and then turn it into a viewer like so:

	jQuery("#mydiv").tileviewer({ ... options ... });

There are many options, but most have reasonable defaults that won't have to change. At a minimum you will need to set two options: *src* and *info*

*src* is the URL of the tile server providing data for the loaded image. Most tile servers take two parameters, the name of the image to view and the tile number to fetch. (Tilepic breaks images into a sequential stream of tiles numbered from 1 to however many tiles are required to cover the image at all required resolutions). The *src* should be the URL required to get tiles for the image you want with the parameter for tile number at the end *omitting the tile number*. Ex. *http://demo.collectiveaccess.org/viewers/apps/tilepic.php?p=http://demo.collectiveaccess.org/media/demo/sample.tpc&t=* 

At the moment Tileviewer only supports Tilepic format images. We plan to extend it to support other tile servers soon.

*info* contains four key values about the image needed for display in the viewer: overall width and height, tile size, and the number of levels. Width and height are the original image dimensions. Tile size is set when encoding an image to Tilepic format and is almost always 256. The number of layers depends upon the image and tile size and is returned by the Tilepic encoding process. You can easily calculate the layer count after the fact if needed. Each layer is half as large as the previous layer, with the smallest layer fitting within a single tile. Counting iterations while halving image size repeatedly until both width and height are less than tile size will give you the layer count.

Tileviewer requires a browser that supports the &lt;canvas&gt; tag. Most do these days but it still is a good idea to check. You can do that with code like this:

	var elem = document.createElement('canvas');
		if (elem.getContext && elem.getContext('2d')) {
			... viewer code goes here ...	
		}
	}

It's also generally a good idea to initialize the viewer after the DOM is fully loaded using jQuery.ready(). The code to load a viewer might look something like this:

	<script type='text/javascript'>
		var elem = document.createElement('canvas');
		if (elem.getContext && elem.getContext('2d')) {
			jQuery(document).ready(function() {
				jQuery('#viewerContainer').tileviewer({
					src: 'http://demo.collectiveaccess.org/viewers/apps/tilepic.php?p=http://demo.collectiveaccess.org/media/demo/sample.tpc&t=',
					info: {
						width: 7426,
						height: 9155,
						tilesize: 256,
						levels: 8
					}
				}); 
			});
		}
	</script>
	
	<div id="viewerContainer" style="width: 800px; height: 800px;"></div>

To use the annotation and measurement features, enable file downloads and change other behavior see the options list below.

Keyboard shortcuts
------------------
The viewer supports a number of keyboard shortcuts for common actions:

<ul>
<li><code>space</code> to activate image panning</li>
<li><code>r</code> to select the rectangle annotation tool</li>
<li><code>p</code> to select the point annotation tool</li>
<li><code>y</code> to select the polygon annotation tool</li>
<li><code>d</code> to delete the selected annotation</li>
<li><code>n</code> to toggle visibility of the image overview</li>
<li><code>h</code> to return the image to the centered, zoomed out "home" position</li>
<li><code>c</code> to toggle visibility of viewer controls</li>
<li><code>TAB</code> to hide controls and labels</li>
<li><code>+</code>or <code>]</code> to zoom in in small increments</li>
<li><code>-</code> or <code>[</code> to zoom out in small increments</li>
<li><code>←</code>, <code>↑</code>, <code>→</code> or <code>↓</code> to pan the image in small increments</li>
</ul>


Options
------- 

### Setup and initialization

#### src
> The URL to the Tilepic tile server. Most tile servers take two parameters, the name of the image to view and the tile number to fetch. (Tilepic breaks images into a sequential stream of tiles numbered from 1 to however many tiles are required to cover the image at all required resolutions). The *src* should be the URL required to get tile for the image you want with the parameter for tile number at the end *omitting the tile number*. Ex. *http://demo.collectiveaccess.org/viewers/apps/tilepic.php?p=http://demo.collectiveaccess.org/media/demo/sample.tpc&t=*  [REQUIRED]

#### info
> A javascript object containing values about the image needed for display: overall width and height, tile size, and the number of levels. Width and height are the original image dimensions. Tile size is set when encoding an image to Tilepic format and is almost always 256. The number of layers depends upon the image and tile size and is returned by the Tilepic encoding process. You can easily calculate the layer count after the fact if needed. Each layer is half as large as the previous layer, with the smallest layer fitting within a single tile. Halving image size repeatedly until both width and height are less than tile size will give you the layer count. Ex.

	{
		width: 7426,
		height: 9155,
		tilesize: 256,
		levels: 8
	}

[REQUIRED]

#### id
> DOM ID to set internal viewer canvas to [Default: tileviewer]

#### empty
> color of empty (loading) tile, as hex color, if no subtile is available [Default: #cccccc]

#### zoomSensitivity
>  Governs how sensitive zooming is. Higher numbers will provide faster zooming. [Default: 16]

#### thumbnail
>  If set image overview (aka. display thumbnail) is displayed on viewer start up. [Default: false]

#### debug
>  Output debugging messages [Default: false]

#### maximumPixelsize
>  Set this to > 1 if you want to let user to zoom image larger than 100% [Default: 4]

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

#### tooltipClass
>  CSS class to apply to all toolbar tooltips [Default: tileviewerTooltipFormat]
            
#### uiIcons
> Specifies icon to use for each user interface (non-toolbar) control. Keys are UI control strings: *zoomIn*, *zoomOut* (image zoom buttons to either side of the top-of-viewer zoom slider control displayed when the *sliderZooming* option is set), *lock* (lock control on annotation text boxes), *delete* (delete control on annotation text boxes) and *close* (close box control on annotation text boxes). Values are the HTML used to render the icon. By default [Font-Awesome](https://fortawesome.github.io/Font-Awesome/) icons are used. If you are not loading Font-Awesome on the page including your viewer you will have to override these to use other icons.  [Default: 
	
	{
		'zoomIn': '<i class="fa fa-plus-square-o fa-2x"><i>',
		'zoomOut': '<i class="fa fa-minus-square-o fa-2x"><i>',
		'lock': '<i class="fa fa-lock"></i>',
		'delete': '<i class="fa fa-trash-o"></i>',
		'close': '<i class="fa fa-times"></i>'
	}
]       

#### toolbarZooming
>  If set zoom in and out controls are shown on toolbar. [Default: false]

#### sliderZooming
>  If set zoom in and out controls with magnification slider are shown at top center of viewer. [Default: false]

#### allowRotation
>  If set image rotation is allowed. [Default: true]

#### mediaDownloadUrl
>  URL to download displayed media. [Default: null]

#### helpLoadUrl
>  URL to HTML help document.  [Default: null]

### Annotations 

#### useAnnotations
>  Allow editing and display of annotations. [Default: true]

#### displayAnnotations
>  Display annotations on viewer load. [Default: true]

#### annotationLoadUrl
>  URL returning list of annotations for the displayed image. See *Annotation date formats* below for details of the annotation list response. [Default: null]

#### annotationSaveUrl
>  URL for annotation recording web service. See *Annotation date formats* below for detail of the expected annotation submission and response formats. [Default: null]

#### lockAnnotations
>  Lock annotations on viewer load. Annotations will display but user cannot add, remove or drag existing annotations.  [Default: false]

#### lockAnnotationText
>  Lock annotation text on viewer load. Annotation text will display but not be editable. [Default: false]

#### showAnnotationTools
>  Show annotation tools on viewer load. [Default: true]

#### annotationTextDisplayMode
>  Determines how viewer displays annotation text: 
- simultaneous: show all annotation text all the time
- mouseover: show annotation text only when mouse is over the annotation or it is selected
- selected: show annotation text only when it is selected 
[Default: mouseover]

#### annotationColor
>  Color to use for annotation outlines when not selected. [Default: #000000]

#### annotationColorSelected
>  Color to use for annotation outlines when selected. [Default: #CC0000]

#### highlightPointsWithCircles
>  If set viewer will draw circles around point label locations. [Default: true]

#### allowDraggableTextBoxesForRects
>  If set viewer will draw draggable text boxes for rectangular annotations. [Default: true]

#### annotationEditorPanel
>  Instance of CollectiveAccess "generic panel" to open extended annotation editor in. [Default: null]

#### annotationEditorUrl
>  URL for extended annotation editor form. [Default: null]

#### annotationEditorLink
>  Content of extended annotation editor link. [Default: More]

#### annotationDisplayMode
>  Determines how annotations are displayed: 
- perimeter: display by outlining perimeter of annotation
- center: display by marking center of annotated regions with circles
[Default: center]

#### annotationDisplayModeCenterColor
>  The color and opacity of the dot used to mark the center, as an rgba() string. Used when *annotationDisplayMode* option is set to "center",  [Default: rgba(175, 0, 0, 0.4)]

#### allowAnnotationList
>  If set display of on-screen list of annotations is allowed. [Default: true]

#### annotationList
>  If set on-screen list of annotations is displayed on viewer load. [Default: false]

#### allowAnnotationSearch
>  Allow annotation search option in on-screen list of annotations. [Default: true]

#### annotationPrefixText
>  Text to display before annotation text. [Default: null]

#### defaultAnnotationText
>  Initial text value for newly created annotations. [Default: null]

#### emptyAnnotationLabelText
>  Text to display in text box when there is no user-set annotation text. [Default: <span class='tileviewerAnnotationDefaultText'>Enter your annotation</span>]

#### emptyAnnotationEditorText
>  Text to display in annotation text editor when there is no user-set annotation text. [Default: Type text. Drag to position.]

#### showEmptyAnnotationLabelTextInTextBoxes
>  If set text specified in the *emptyAnnotationEditorText* is displayed in annotation text editor when no user-set text is available.  [Default: true]

#### useKey
>  Allow display of on-screen annotation color key. [Default: false]

#### showKey
>  If set on-screen annotation color key is displayed on viewer load. [Default: false]

### Measurements

#### enableMeasurements
>  If set, show measurement tool and prompt user to set image scale [Default: true]

#### scale
>  Scale factor for tranlating viewer measurements from relative lengths to physical lengths. [Default: null]

#### measurementUnits
>  Units of *scale* (Ex. in, ft, cm, m) [Default: null]

#### imageScaleControlFirstSetText
>  Text to display to user when first setting scale for an image. [Default: 

	<div class='tileviewerImageScaleControlsHeader'>A scale must be set for this image before measurements can be evaluated.</div><div class='tileviewerImageScaleControlsHelpText'>Enter the length with units (mm, cm, m, km, in, ft, miles, etc.) of the currently selected measurement below.</div>
]

#### imageScaleControlChangeSettingText
>  Text to display to user when changing scale settings. [Default: 

	<div class='tileviewerImageScaleControlsHeader'>This image is scaled at %1.</div><div class='tileviewerImageScaleControlsHelpText'>To change scale enter the length with units (mm, cm, m, km, in, ft, miles, etc.) of the currently selected measurement below.</div>
]

Annotation data
---------------
Annotations are stored and loaded separately from the image. They may be stored by any means and in any format, but typically they are serialized in a database. Data for an annotation consists of an identifier, some text and metadata, and a series of *size-relative* coordinates and dimensions. 

A size-relative coordinate represents the distance to the X or Y origin as the percentage of image width or height. The origin is taken to be the upper left-hand corner of the image. Ex. For X coordinates, 0 would indicate the X origin, 100 the extreme right side of the image and 33.33333 a point one-third of the way across the image. Similarly a size-relative dimension represents a fraction of the width or height of the image.

The set of coordinates varies by type of annotation. Currently there are four types:

* *Point* The simplest type of annotation, applying to a specific point location. Requires X and Y coordinates for annotation location and text box placement, as well as text box width and height.
* *Rectangle* A rectangular region. Requires X and Y coordinates for annotation location and text box placement, as well as annotation and text box width and height.
* *Polygon* An arbitrary closed path passing through any number of points. Requires a list of X and Y coordinates defining the annotation region, X and Y coordinates for text box placement, and text box width and height.
* *Measurement* A line segment that represents a measured distance. Requires two X and Y points for segment location, an X and Y location for text box placement, as well as text box width and height.

The viewer interacts with a server to load annotation data on start up and to record  changes to annotations over time. The viewer loads the initial state of an image's annotations from the URL specified by the *annotationLoadUrl* option. The response should be a JSON list containing as many objects as there are annotations. Each object should contain the following keys:

* *annotation_id* A unique identifier for the annotation. This should be generated in the server-side database.
* *x* the horizontal location of the annotation in size-relative coordinates.
* *y* the vertical location of the annotation in size-relative coordinates.
* *w* the size-relative width of the annotation.
* *h* the size-relative height of the annotation. 
* *tx* the horizontal location of the text box in size-relative coordinates.
* *ty* the vertical location of the text box in size-relative coordinates.
* *tw* the size-relative width of the text box.
* *th* the size-relative height of the text box. 
* *points* A list of size-relative X/Y coordinates defining a polygon or measurement line.
* *label* Text to display in annotation text box.
* *type* Annotation type, one of *point*, *rect*, *polygon*, *measurement*.
* *locked* Indicates if annotation is locked to prevent changes; 0=not locked, 1=locked.
* *key* Annotation color key.

When saving changes to image annotations, the viewer will contact the server using the *annotationSaveUrl* with a JSON-format data object. The object has the following keys:

* *save* contains any annotations that need to be saved. Each annotation is represented as an object with all of the keys appropriate to its type (save for *key* which is calculated on the server).
* *delete* is a list of annotation_ids for annotations to be removed. 

The response is always a JSON object with *error* and *annotation_ids* keys. *error* will be set to an error message or null if no error occurred. *annotation_ids* is a list of annotation_ids that were affected by a *save*. Annotation_ids are not returned for *delete* actions.

Credits
-------
Forked from Soichi Hayashi's excellent jquery-tileviewer plugin (currently available at https://github.com/soichih/jquery-tileviewer) back in 2011, before it was on Github :-)

Developed with support from the National Science Foundation and National Historical Publications and Records Commission, in collaboration with Stony Brook University (State University of New York), the [Historical Society of Pennsylvania](http://www.hsp.org) and the Yale Peabody Museum.

TODO
----
*  Reduce or eliminate the number of dependencies; in particular eliminate the dependency on jQuery-UI and CollectiveAccess ca.genericpanel.js library
*  Add support for tiled image formats other than Tilepic
*  Reimplement touch event support (stripped from current code)
*  Make available via Bower, et al.

Known Issues
------------
*  Detection of mouse over on annotations is skewed and incorrect when image is rotated

License
-------
Tileviewer is made available under the [GNU Public License version 3](http://www.gnu.org/licenses/gpl-3.0.en.html).