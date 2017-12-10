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

### Scenario #1, Convert Retrobattle to SDL2


Current features
----------------

These are the current features provided by Cats version 1:

* Basic sprites with animations
* Scrollable tile background


Goals
-----

These are the goals for Cats version 2:

* Frame offsets for sprites


Nongoals
--------

The first version of Cats will not support the following features,
though many will be included in later versions:

* More "dynamic" sprite definition with variable frame sizes
* Animation pausing
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

The detailed functional specification naturally consists of a list of
methods and data structures that make up the API for the graphics
engine, as this is what the user of the graphics engine sees and uses.

In general, this is how the graphics engine will be used:

### 1. Initialization

This consists of creating the game window as well as setting up the
renderer. Here is also where sprites and tilesets are loaded.

### 2. Setup of backgrounds and sprite instances creation

Set up the initial background and create sprite instances for the
objects that will be visible from the start.

### 3. Continous updating of sprite instances and tilemaps

For each frame the sprite instances will probably be moved around, the
background might be scrolled, and individual tiles in the tilemap may
be modified.


Details
-------

### File formats

The file formats are all based on JSON.

#### Sprite file format

A sprite will be specified in a JSON object, containing the key
"animations". "animations" will point to an object containing the
animations, with the keys used as animation names. The name of the
JSON file will determine the sprites name, e.g. the filename
"hero.json" means that the sprite will be called "hero".

Each animation will be specified as an object, containing the keys
"image", "looping" and "frames". "image" will contain an object
describing the image path and frame dimensions. "looping" indicates
whether this animation should loop or not. "frames" will contain a
list of lists, where each such list points out an index into the image
and a duration in milliseconds. A frame index of -1 indicates that the
sprite will be hidden during the frames duration.

The images path is relative to the sprite definitions path, so a path
of just "goomba.png" means that the image will be found as a neighbour
to the sprite definition.

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
             "looping": true,
             "frames": [
                 [0, 100], [1, 150], [2, 100], [1, 150]
             ]
         }
	}
}
</pre>

#### Tileset file format

A tileset will be specified in a JSON object, containing the key
"image". "image" will contain an object describing the image path and
tile dimensions. The name of the JSON file will determine the tilesets
name, e.g. the filename "blocks.json" means that the tileset will be
called "blocks".

The images path is relative to the tileset definitions path, so a path
of just "blocks.png" means that the image will be found as a neighbour
to the tileset definition.

Example tileset definition:

<pre>
{
    "image": {
        "path":"path/to/blocks.png",
        "width":16,
        "height":16,
    }
}
</pre>

### Methods

*   Init(width, height)

	Creates a window for the application with the given width and
    height.

	width - integer, width in pixels of the draw area

	height - integer, height in pixels of the draw area

*   Redraw(delta)

	Draw all the tiles and sprites. Called once each frame.

	delta - float, seconds since previous call to redraw

*   SetBackgroundColor(red, green, blue)

	Set color to be used when clearing the screen prior to redrawing.

	red - byte, red component, 0-255

	green - byte, green component, 0-255

	blue - byte, blue component, 0-255

*   LoadSprite(file)

	Loads in the provided sprite definition and makes it available to
    the graphics engine.

	file - string, path to the sprite definition file

*   CreateSpriteInstance(name) : id

    Create a sprite instance of a loaded sprite definition.

	name - string, name of the sprite definition, same as the filename
    but without the .json suffix

	id - integer, return value, id used to indicate this sprite instance

*   RemoveSpriteInstance(id)

	Remove the provided sprite instance.

	id - integer, id of a sprite instance

*   ShowSprite(id, show)

	Show or hide a sprite instance.

	id - integer, id of a sprite instance

	show - boolean, indicate whether this sprite instance should be
    visible or not

*   SetAnimation(id, animation)

	Set the sprite instances animation. The animation will start from
    the beginning.

	id - integer, id of a sprite instance

	animation - string, name of an animation

*   SetSpritePosition(id, x, y)

	Set a sprite instances position in screen coordinates.

	id - integer, id of a sprite instance

	x - integer, x component of the position

	y - integer, y component of the position

*   SetupTileLayer(width, height, tile width, tile height)

	Setup a tile background containing width*height tiles. Each
    individual tiles height and width is also specified.

	width - integer, width of the tilemap

	height - integer, height of the tilemap

	tile width - integer, width of a tile

	tile height - integer, height of a tile

*   LoadTileset(file)

	Loads in the provided tileset.

	file - string, path to the tileset definition file

*   SetTile(x, y, tileset, tile x, tile y)

	Sets the tile at position x, y to the tile found in the provided
    tileset at location tile x, tile y.

	x - integer, x position in the tilemap where the tile will be set

	y - integer, y position in the tilemap where the tile will be set

	tileset - string, name of the tileset to use

	tile x - integer, x offset into the tileset

	tile y - integer, y offset into the tileset

*   SetScroll(x, y)

	Set the scroll of the tile layer.

	x - integer, x offset of the tile layer

	y - integer, y offset of the tile layer
