All map generator algorithms have a few common parameters, plus any mapgen-specific options such as noise parameters or threshhold values.

# Mapgen parameter source precedence {#mapgen_parameter_source_precedence}

There are several sources which the map generator may retrieve parameters from. In order from highest precedence to lowest they are:

-   Hard-coded
-   Lua mods
-   map_meta.txt
-   Game-specific config file
-   Global config file
-   Default settings (set in defaultsettings.cpp)
-   Default object constructor values

# List of mapgen parameters {#list_of_mapgen_parameters}

## Mapgen name {#mapgen_name}

The name of the map generator algorithm being used. Currently supported:

  Mapgen name   Developer responsible   Description
  ------------- ----------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  v5            missing info            Description needed
  v6            kwolekr, celeron55      The current default map generator. Uses an improved Mapgen v2 algorithm for base terrain; terrain is generated entirely using 2D Perlin noise.
  v7            kwolekr                 The next generation map generator with an emphasis on generating interesting and playable terrain. Has full support for the internal biome infrastructure and uses a mixture of 2D and both positive and negative 3D noise for terrain.
  valleys       missing info            Description needed
  carpathian    missing info            Description needed
  fractal       missing info            Generates maps based on Mandelbrot fractal series
  flat          missing info            Generates completely flat world. Replaces the flat flag in mg_flags. Caves, dungeons, decorations, and biomes need to be explicitly excluded to make a flat and empty world.
  singlenode    celeron55               Places only a single node everywhere, namely the one aliased to mapgen_singlenode (by default air). Intended to be used by mods that perform total mapgen takeovers.

example: `mg_name = v7`

## Seed

A 64-bit unsigned integer value. At this time, only the lower 32 bits are currently used for randomization.\
example: `seed = 1246106421`

## Water level {#water_level}

The y coordinate at which water starts being placed, for mapgens that do place water.\
This is also the position at which blocks are assumed to be underground if no block above is present, and thus is not given sunlight.\
example: `water_level = 1`

## Mapgen Limit {#mapgen_limit}

Limit of map generation, in nodes, in all 6 directions from (0, 0, 0).

-   Only mapchunks completely within the mapgen limit are generated.
-   Value is stored per-world.
-   type: int min: 0 max: 31000\

example : `mapgen_limit = 4500`

## Flags parameter {#flags_parameter}

The flags parameter is a set of booleans indicating whether or not a certain option is enabled. Global flags applying to all map generators are listed in [mg_flags](mg_flags "wikilink"). Other map generator-specific flags are shown below.\
Like all other config and Lua flag fields, they are represented as a comma-delimited string. E.g. the flag string \"trees, caves, flat\" would direct the Mapgen to create flat terrain with trees and caves.\
An exhaustive list of currently recognized Mapgen flags:

  Lua/config flag name   Defaults                                                    Allowed values
  ---------------------- ----------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------
  mgv5_spflags           caverns                                                     caverns, nocaverns
  mgv6_spflags           jungles,biomeblend,mudflow,snowbiomes,trees                 jungles, biomeblend, mudflow, snowbiomes, flat, trees, nojungles, nobiomeblend, nomudflow, nosnowbiomes, noflat, notrees
  mgv7_spflags           mountains,ridges,nofloatlands,caverns                       mountains, ridges, floatlands, caverns, nomountains, noridges, nofloatlands, nocaverns
  mgcarpathian_spflags   caverns                                                     caverns, nocaverns
  mgflat_spflags         nolakes,nohills                                             lakes, hills, nolakes, nohills
  mgvalleys_spflags      altitude_chill,humid_rivers,vary_river_depth,altitude_dry   altitude_chill, noaltitude_chill, humid_rivers, nohumid_rivers, vary_river_depth, novary_river_depth, altitude_dry, noaltitude_dry

Other fine-tuning parameters are available for each map generator, and can be found in lua_api.md.

## Chunk size {#chunk_size}

The side length (in MapBlocks) of the cubic area that is generated at once. Default is 5; larger values take longer to generate but blocks generate quicker on average and could produce larger, more intricate caves and dungeons.\
Don\'t mess around with this if you don\'t know what you\'re doing! This parameter cannot be modified through the Lua API.

```{=mediawiki}
{{Incomplete}}
```
[Category:Core Engine](Category:Core_Engine "wikilink")
