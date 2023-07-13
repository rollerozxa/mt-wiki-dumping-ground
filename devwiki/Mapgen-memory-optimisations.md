```{=mediawiki}
{{LuaTips}}
```
*This page is based on Paramat\'s [forum topic](https://forum.minetest.net/viewtopic.php?t=16043) and has been updated for Minetest 5.0.0.*

Lua map generators can use excessive memory if they are not using these 3 optimisations. Not using these optimizations can eventually lead to OOM (out-of-memory) errors because the Lua mapgen wasted way too much memory. Applying this advice is **strongly recommended**, and should make OOM errors much less likely. But there is no guarantee this will fix all OOM errors, your mapgen might still use excessive memory for other reasons, or the computer just has very limited memory.

## Perlin noise objects: Only create once {#perlin_noise_objects_only_create_once}

The noise object is created by `minetest.get_perlin_map()`. It has to be created inside `register_on_generated()` to be usable, but only needs to be created once, many mapgen mods create it for every mapchunk, this consumes memory unnecessarily.

Localise the noise object outside `register_on_generated()` and initialise it to `nil`. See the code below for how to create it once only. The creation of the perlin noise tables with `get_3d_map_flat()` etc. is done per mapchunk

    -- Initialize noise objects to nil

    local nobj_terrain = nil
    local nobj_biome = nil

    -- ...
    -- ...

    -- On generated function
    minetest.register_on_generated(function(minp, maxp, seed)
       -- ...
       -- ...
       nobj_terrain = nobj_terrain or minetest.get_perlin_map(np_terrain, chulens3d)
       nobj_biome = nobj_biome or minetest.get_perlin_map(np_biome, chulens2d)
       
       nobj_terrain:get_3d_map_flat(minpos3d, nvals_terrain)
       nobj_biome:get2d_map_flat(minpos2d, nvals_biome)
       -- ...
       -- ...
    end)

## Perlin noise tables: Re-use a single table {#perlin_noise_tables_re_use_a_single_table}

The Lua table that stores the noise values for a mapchunk is big, especially for 3D noise (80^3^ = 512000 values). Many Lua mapgens are creating a new table for every mapchunk, while the previous tables are only cleared out slowly by garbage collection, resulting in a large and unnecessary memory use.

A `buffer` parameter was added in 0.4.13 to avoid this, a single table is re-used by overwriting the former values.

`` For each of the functions with an optional `buffer` parameter:  If `buffer` is not ``\
`nil, this table will be used to store the result instead of creating a new table.`\
` `\
`#### Methods`\
`` * `get_2d_map_flat(pos, buffer)`: returns a flat `<size.x * size.y>` element array of 2D noise ``\
`` with values starting at `pos={x=,y=}` ``\
`` * `get_3d_map_flat(pos, buffer)`: Same as `get_2d_map_flat`, but 3D noise ``

Localise the noise buffer outside `register_on_generated()` Use the `buffer` parameter in `get_3d_map_flat()` etc.

    -- Localise noise buffers
    local nvals_terrain = {}
    local nvals_biome = {}
    -- ...
    -- ...
    -- On generated function
    minetest.register_on_generated(function(minp, maxp, seed)
       -- ...
       -- ...
       nobj_terrain:get3d_map_flat(minpos3d, nvals_terrain)
       nobj_biome:get2d_map_flat(minpos2d, nvals_biome)
       -- ...
       -- ...
    end)

## Lua voxelmanip table: Re-use a single table {#lua_voxelmanip_table_re_use_a_single_table}

The Lua table that stores the node content IDs for a mapchunk plus the mapblock shell is big (112^3^ = 1404928 values). Many Lua mapgens are creating a new table for every mapchunk, while the previous tables are only cleared out slowly by garbage collection, resulting in a large and unnecessary memory use.

A `buffer` parameter was added in 0.4.13 to avoid this, a single table is re-used by overwriting the former values.

In Minetest 0.4.15, a `buffer` parameter was also added to `get_param2_data()`.

`` * `get_data([buffer])`: Retrieves the node content data loaded into the `VoxelManip` object ``\
`    * returns raw node data in the form of an array of node content IDs`\
``     * if the param `buffer` is present, this table will be used to store the result instead ``\
`` * `get_param2_data([buffer])`: Gets the raw `param2` data read into the `VoxelManip` object ``\
``     * Returns an array (indices 1 to volume) of integers ranging from `0` to `255` ``\
``     * If the param `buffer` is present, this table will be used to store the result instead ``

Localise the data buffer outside `register_on_generated()`. Use the buffer in `get_data()` or `get_param2_data()`.

    -- Localise data buffer
    local data = {}

    -- On generated function
    minetest.register_on_generated(function(minp, maxp, seed)
       -- ...
       -- ...
       local vm, emin, emax = minetest.get_mapgen_object("voxelmanip")
       local area = VoxelArea:new{MinEdge = emin, MaxEdge = emax}
       vm:get_data(data)
       -- ...
       -- ...
    end)

[Category:Mapgen](Category:Mapgen "wikilink")
