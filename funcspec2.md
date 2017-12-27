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

To be able to convert Retrobattle to use Cats, various new features must be
added.


Current features
----------------

These are the current features provided by Cats version 1:

* Basic sprites with animations
* Scrollable tile background


Goals
-----

These are the goals for Cats version 2:

* Frame offsets for sprites (origin)
* Animation pausing
* Pause all animations
* Fullscreen toggle
* Toggle mouse pointer
* Window title
* Move sprite relative to current position
* Text rendering


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
             "origin": [16, 16],
             "looping": true,
             "frames": [
                 [0, 100], [1, 150], [2, 100], [1, 150]
             ]
         }
	}
}
</pre>

### Methods

* PauseAnimation(id, on)

  Set pause state of the provided sprite instance.

	id - integer, id of a sprite instance

  on - boolean, indicates whether animation should be paused or not

* PauseAnimations(on)

  Set pause state of all sprite instances.

  on - boolean, indicates whether animations should be paused or not

* SetFullscreen(on)

  Set fullscreen mode.

  on - boolean, indicates whether fullscreen mode should be on or off

* ShowPointer(on)

  Show or hide mouse pointer.

  on - boolean, indicates whether the mouse pointer should be shown or not

* SetWindowTitle(title)

  Set the window title.

  title - string, the window title

* MoveSprite(id, delta x, delta y)

  Move sprite relative to its current position.

	id - integer, id of a sprite instance

  delta x - the amount to move in the x axis

  delta y - the amount to move in the y axis

