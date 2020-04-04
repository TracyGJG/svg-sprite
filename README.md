# svg-sprite
Experiements with SGV Sprites

## Background
The purpose behind this series of experiments was to develop a way of compiling an SVG Sprite (an SVG file containg a number of individual SVG images) so the a single (sub-)SVG image can be used in an HTML image element or as a CSS background.

The mechamism needs to minimise manipulation of the original sub-image yet provide a convenient address for its use in an HTML page.

## Experiment Zero: Embedded
This was not really an experiment but an exercise to confirm the ground rules. This test case involves including the SVG sprint in the HTML file with each sub-image contained within SVG Symbol elements. This enabled the sub-image to remain untouched (just cut and pasted) but to access it we need an SVG element containing a USE element, and a slightly complicated address using an href from the xlink namespace.

## Experiment One: Symbol Unstyled
In the first experiment we move the sprite to its own file in the same structure as in the previous example. However, this makes the addressing, from within the HTML file, even more complicated and still restricts us to use SVG elements when what we want is to use HTML image elements and CSS background-image styles.

On the plus-side, this appraoch does not require a web server for the HTML page to access the SVG sprite.

## Experiment Two: Symbol Styled
This experiment is just an extention of the previous but demonstrates another advantage of the approach in that elements within the sub-image can be styled using CSS from within the HTML page. Again, this is a server-side solution.

## Experiment Three: Fragment Identifiers
SVG now provides a View element that enables the developer to assign an identifier (#name} to a fragment of the SVG document using a viewBox parameter. This is very well suited for sprites and works well with HTML image elements and CSS background-images. Addressing a sub-image is still on the difficult side but is little more than applying a hash Id after the sprite URL, much like any deep-link.

This solution is usable client-side but there are a couple a big drawbacks:
1) All of the sub-image use the same coordinate system making it necessary to revise the locations of all but one image to ensure each has its own space that can be referenced using the viewBox attribute.
2) This a minor issue but to help maintain the sprite it would be beneficial to keep all the elements in volved in an image together.

## Experiment Four: Nested
Ideally, what we are after is a sprite that can be built by copy-and-pasteing the content of smaller SVG's and adding a reference so we can access them fomr HTML image elements.

In this experiement we get this by transfering the content of the image SVG elements into specifically located SVG elements, nested within the SVG element of the sprite. We can then use view elements as before by reference. The only down-side is, this only works client-side and not from a web server.

## Experiment Five: Combination
In this final experiment we have a slightly more complicated solution but we also get;
1) client-side and server-side support
2) simple containment of the image SVG content
3) a simple (URL#) reference from within image elements.

This is acheived using an SVG Symbol to contain the images, each with its own coorinate system. We have to attribute each Symbol with a private Id, which is used by an SVG USE element to create an instance of the image at a given location within th sprite. Finally we use an SVG VIEW element to attribute the fragment that contains the image with an identifer that can be used externally.

A bonus with this approach is, when accessed from a server, the Symbols can be accessed using the Id from within the HTML document, using SVG and USE elements. This mechanism allows the image to be styled using CSS in the HTML document. 
