CollectiveAccess/Historic Images, New Technologies (HINT) jQuery Tileviewer
===========================================================================

A jQuery plugin implementing an HTML5 viewer for high resolution imagery. Only the on-screen portion of the image is loaded, at the minimum resolution suitable for the zoom level. This allows even extremely large images to load quickly and be examined in detail at full resolution with fluid panning and zooming.

Before an image can be used with Tileviewer it must be broken down into square chunks ("tiles") at various resolutions. The viewer will load these tiles as needed to form the image. While are several tiled image formats in use, Tileviewer currently only supports [Tilepic] (http://docs.collectiveaccess.org/wiki/TilePic), a format developed at the University of California, Berkeley library.

Tileviewer was originally developed for [CollectiveAccess](http://www.collectiveaccess.org), open source collections management software for museums and archives. The version is this repository is extended to fulfill requirements of the Historical Society of Pennsylvania's [Historic Images, New Technologies](https://hsp.org/history-online/historic-images-new-technologies) (HINT) project, with support from the [National Historical Publications and Records Commission](http://www.archives.gov/nhprc/) (NHPRC). All of the changes in this version are also available in the latest [CollectiveAccess](https://github.com/collectiveaccess/providence) version.

Features
--------
To come

Installation
------------
To come

Dependencies
------------

The following javascript libraries are used by Tileviewer and must be loaded before a viewer in initialized:

*  JQuery 1.7+
*  JQuery.HotKeys
*  Brandon Aaron's (http://brandonaaron.net) mousewheel jquery plugin 3.0.3
*  Circular-Slider (https://github.com/princejwesley/circular-slider)
*  jQuery-UI 1.9+

Options
------- 
To come

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