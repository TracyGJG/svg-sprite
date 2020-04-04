# svg-sprite

Experiements with Scalable Vector Graphic (SVG) Sprites

## Background

The purpose behind this series of experiments was to develop a way of compiling an SVG Sprite (an SVG file containing a number of individual SVG images) so that a single (sub-)SVG image can be used in an HTML image element or as a CSS background.

The mechamism needs to minimise manipulation of the original sub-image yet provide a convenient address for its use in an HTML page. The approach also needs to be usable with resources hosted on a web server.

## Experiment Zero: Embedded

This was not really an experiment but an exercise to confirm the ground rules. This test case involves including the SVG sprint in the HTML file with each sub-image contained within SVG Symbol elements. This enables the sub-image to remain untouched (just cut and pasted) but to access it we need an SVG element containing a USE element, and a slightly complicated address using an href from the xlink namespace.

## Experiment One: Symbol (Unstyled)

In the first experiment we move the sprite to its own file in the same structure as in the previous example. However, this makes the addressing, from within the HTML file, even more complicated and still restricts us to SVG elements, when what we want is to use HTML image elements and CSS background-image styles.

Another slmall negative for this approach is it is server-side only solution.

## Experiment Two: Symbol (Styled)

This experiment is just an extention of the previous but demonstrates an advantage of the approach in that elements within the sub-image can be styled using CSS, from within the HTML page.

## Experiment Three: Fragment Identifiers

SVG now provides a VIEW element that enables the developer to assign an identifier (#name} to a fragment of the SVG document using a viewBox parameter. This is very well suited for sprites and works well with HTML image elements and CSS background-images. Addressing a sub-image is still on the difficult side but is little more than applying a hash Id after the sprite URL, much like any deep-link.

This solution is usable client-side but there are a couple a big drawbacks:

1. All of the sub-images use the same coordinate system making it necessary to revise the locations of all but one sub-image to ensure each has its own space that can be referenced using the viewBox attribute.
2. This is a minor issue but, to help maintain the sprite long-term it would be beneficial to keep all the elements involved in a sub-image together.

## Experiment Four: Nested

Ideally, what we are after is a sprite that can be built by copy-and-pasteing the content of smaller SVG's and adding a reference so we can access them from HTML image elements.

In this experiement we get this by transfering the content of the image SVG elements into specifically located SVG elements, nested within the SVG element of the sprite. We can then use VIEW elements as before to provide a reference.

The only down-side is, this only works client-side and not from a web server.

## Experiment Five: Combination

In this final experiment we have a slightly more complicated solution but we get so much in return;

1. client-side and server-side support
2. simple containment of the image SVG content
3. a simple (URL#) reference from within image elements.

This is acheived using an SVG Symbol to contain the images, each with its own coorinate system. We have to attribute each Symbol with a 'private' Id, which is used by an SVG USE element within the sprite. This is used to create an instance of the image at a specific location within th sprite.

Finally we use an SVG VIEW element to attribute the fragment, that contains the image, with an identifer that can be used externally.

A bonus with this approach is, when accessed over a web server, the Symbols can be accessed using the 'private' Id from within the HTML document, using SVG and USE elements. This mechanism allows the image to be styled using CSS in the HTML document.
