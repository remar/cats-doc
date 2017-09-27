Cats, functional specification #1
=================================

Project
-------

Cats - C# Abstraction for Tiles and Sprites. SDL2 2D general purpose
graphics engine.

This is version 1 of the specification. The API, data structures and
file formats should be regarded as a _work in progress_ and will
probably change down the road. The implementation of this
specification will be versioned as 0.1.


Author
------

Andreas Remar, andreas.remar@gmail.com


Scenarios
---------

### Scenario #1, Side scroller

Andreas wants to try his hand at making a side scrolling game, like
Super Mario Bros. To accomplish this he neads a graphics engine that
can scroll a background and display animated sprites.

To begin with, he calls a single method that sets up a window and a
renderer in that window that is ready to do its work. He then loads in
the animated sprites and the tilesets that will be used in the game.

After that he sets up a single layer of tiles by specifying its width
and height and the tile dimensions. Then he simply specifies which
tile should be located where in the tile layer. And then a player
character sprite is created and displayed at the correct
position. Cats is taking care of all the rendering for him.


Goals
-----

These are the first main features that will be the focus of the
initial version of Cats:

* Basic sprites with animations
* Scrollable tile background

Note that, for simplicity, the initial version will only support one
layer each for tiles and sprites.


Nongoals
--------

The first version of Cats will not support the following features,
though many will be included in later versions:

* More "dynamic" sprite definition with variable frame sizes
* Frame offsets
* Sprite metadata
* Tile metadata
* Read in a tilemap from a TMX file
* Sprite rotation
* Sprite flip (horizontal/vertical)
* Tile layers (and layer info for sprites)
* Tile flip (horizontal/vertical)


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


### Methods

*   Init(width, height)

	Creates a window for the application with the given width and
    height.

	width - integer, width in pixels of the draw area

	height - integer, height in pixels of the draw area

*   LoadSprite(file)

	Loads in the provided sprite definition and makes it available to
    the graphics engine.

	file - string, path to the sprite definition file
