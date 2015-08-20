CollectiveAccess jQuery Tileviewer
==================================

A jQuery plugin implementing an HTML5 viewer for high resolution imagery. Only the on-screen portion of the image is loaded, at the minimum resolution suitable for the zoom level. This allows even extremely large images to load quickly and be examined in detail at full resolution with fluid panning and zooming.

Before an image can be used with Tileviewer it must be broken down into square chunks ("tiles") at various resolutions. The viewer will load these tiles as needed to form the image. While are several tiled image formats in use, Tileviewer currently only supports [Tilepic] (http://docs.collectiveaccess.org/wiki/TilePic), a format developed at the University of California, Berkeley library.

Installation
------------
To come


Options
------- 
To come

Demonstration
-------------
An example of the viewer is in the *example/* folder in the GitHub repository. You can also see the example at http://demo.collectiveaccess.org/ImageViewerExample/

Credits
-------
Forked from Soichi Hayashi's excellent jquery-tileviewer plugin (currently available at https://github.com/soichih/jquery-tileviewer) back in 2011, before it was on Github :-)


TODO
----
*  Reduce or eliminate the number of dependencies; in particular eliminate the dependency on jQuery-UI
*  Add support for tiled image formats other than Tilepic

Known Issues
------------
*  Detection of mouse over on annotations is skewed and incorrect when image is rotated
