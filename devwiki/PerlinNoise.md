```{=mediawiki}
{{UnofficialLua}}
```
A [perlin noise](http://en.wikipedia.org/wiki/Perlin_noise) generator. Can be created either via

    PerlinNoise(seed, octaves, persistence, scale)

or

    minetest.get_perlin(seeddiff, octaves, persistence, scale)

. Note that [PerlinNoiseMap](PerlinNoiseMap "wikilink") works faster.\

## Methods

-   get2d(pos)

    --- 2d noise value at

        pos={x=,y=}

-   get3d(pos)

    --- 3d noise value at

        pos={x=,y=,z=}

\

## Examples

TODO\

## See also {#see_also}

-   [PerlinNoiseMap](PerlinNoiseMap "wikilink")
-   [:Category:Mapgen](:Category:Mapgen "wikilink")

\

## External Links {#external_links}

-   [Minetest forum thread: \"minetest.get_perlin() and minetest.get_perlin_map() Return nil\"](https://forum.minetest.net/viewtopic.php?f=6&t=8157)
-   [Explanation of perlin noise](http://web.archive.org/web/20160305194643/http://freespace.virgin.net/hugo.elias/models/m_perlin.htm)
-   [Explanation of perlin noise and simplex noise](http://webstaff.itn.liu.se/~stegu/simplexnoise/simplexnoise.pdf)
-   [Perlin noise values exceed \[-1; 1](https://forum.minetest.net/viewtopic.php?id=4032)\]
-   [Faster mapgen noises](https://forum.minetest.net/viewtopic.php?f=7&t=5146)

\

[Category:Objects](Category:Objects "wikilink")
