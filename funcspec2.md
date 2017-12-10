Cats, functional specification #2
=================================

Project
-------

Cats - C#/C++ Abstraction for Tiles and Sprites. General purpose 2D
graphics engine.

This is version 2 of the specification. The API, data structures and
file formats should be regarded as a _work in progress_ and will
probably change down the road. The implementation of this
specification will be versioned as 0.2.


Author
------

Andreas Remar, andreas.remar@gmail.com


Scenarios
---------

### Scenario #1, Convert Retrobattle to use Cats++

To be able to convert Retrobattle to use Cats, frame offsets for sprites
must be added to Cats.


Current features
----------------

These are the current features provided by Cats version 1:

* Basic sprites with animations
* Scrollable tile background


Goals
-----

These are the goals for Cats version 2:

* Frame offsets for sprites
* Animation pausing

TODO: Add more features here as work on converting Retrobattle to Cats
proceeds.


Nongoals
--------

This version of Cats will not support the following features, though many
will be included in later versions:

* More "dynamic" sprite definition with variable frame sizes
* Sprite metadata
* Tile metadata
* Read in a tilemap from a TMX file
* Sprite rotation
* Sprite flip (horizontal/vertical)
* Tile layers (and layer info for sprites)
* Tile flip (horizontal/vertical)
* Tile map wrap around
* Dynamic tile definitions (specify in the file where each tile is
  located in the graphics)
* Camera


Overview
--------

The same base functionality that exists in Cats version 1 is retained and
expanded with a couple of minor additions.

The new sprite frame offset requires that the sprite file definition format
is extended.

The detailed functional specification naturally consists of a list of
methods and data structures that make up the API for the graphics
engine, as this is what the user of the graphics engine sees and uses.


Details
-------

### File formats

The file formats are all based on JSON.

#### Sprite file format extension

An animation can now have an offset that will be subtracted from the images
position when the image is rendered. This offset will be the same for all
frames in the animation.

Example sprite definition:

<pre>
{
    "animations": {
        "walk": {
            "image": {
                 "path":"path/to/walk.png",
                 "width":32,
                 "height":32
             },
             "offset": [4, 4],
             "looping": true,
             "frames": [
                 [0, 100], [1, 150], [2, 100], [1, 150]
             ]
         }
	}
}
</pre>

