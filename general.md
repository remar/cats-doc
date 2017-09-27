General ideas for Cats
======================

The graphics engine will have two distinct parts, sprites and
tiles. Sprites are things like game characters, enemies, pickups and
stuff like that. Tiles are the static background and foreground. The
tiles will be arranged in a grid.

See the functional specification for a technical view of Cats.


Sprites
-------

Each sprite will have a name and one or more animations. Each animation
will reference an image and have one or more frames. Each frame will
point into the animations image and provide offsets and the like. Each
frame also has a duration specified in milliseconds.

Sprites will perhaps also contain metadata, e.g. for bounding boxes.

To have sprites moving around on the screen, sprite instances will
have to be created. Each instance will reference a sprite. Each sprite
instance has an animation, position, rotation, flip, visibility and
layer.


Tiles
-----

Tiles will be arranged in grids, each in their own layer. Tiles will
be arranged in tilesets, where each tileset will reference an image
and specify the tile width and height.

One idea is to allow animated tiles. Or maybe do that with sprites
instead.

Layers will need to be set up with some kind of specification of width
and height.

Layers will be scrollable independently (e.g. for parallax scrolling).

Would be nice to be able to use Tiled for making tile maps. Either
support the "native" Tiled format or make an extension to Tiled.

Tiles might have metadata, e.g. for collision detection.
