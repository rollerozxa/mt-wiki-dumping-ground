\_\_NOTOC\_\_ This article lists various free software tools which help in development of Minetest and mods for Minetest.

## Minetest development {#minetest_development}

-   [Development Test](https://wiki.minetest.net/Games/Development_Test): A testing game for testing various engine features
-   [/minetest/util](https://github.com/minetest/minetest/tree/master/util): Various maintenance utilities

## Mod development {#mod_development}

### Standalone software {#standalone_software}

-   [NodeBoxEditor](https://forum.minetest.net/viewtopic.php?f=14&t=2840): Build [node boxes](Node_boxes "wikilink") by dragging their edges.
-   [MTS Editor](https://forum.minetest.net/viewtopic.php?f=14&t=23724): A program to create, view and edit Minetest schematics, but it supports other file formats (e.g. Minecraft schematics) as well
-   [Schematic Creator](https://forum.minetest.net/viewtopic.php?f=14&t=18992): A Java program to create schematics
-   [Model Creator](https://forum.minetest.net/viewtopic.php?f=14&t=18780&): A Java program to create meshes
-   [RocketLib Toolkit](https://forum.minetest.net/viewtopic.php?t=23891): Lua-based SQLite3 map reader
-   [luacheck](https://github.com/mpeterv/luacheck/): Lua linter and static code analyser (see also [the chapter rubenwardy\'s modding book](https://rubenwardy.com/minetest_modding_book/en/quality/luacheck.html))
-   [busted](https://olivinelabs.com/busted/): Lua unit testing framework (see also [the chapter rubenwardy\'s modding book](https://rubenwardy.com/minetest_modding_book/en/quality/unit_testing.html))

### Minetest games and mods {#minetest_games_and_mods}

See the [Developer Tools](https://content.minetest.net/packages/?tag=developer_tools) tag in ContentDB.

## Scripts

#### Formspecs

-   [Minetest Formspec Editor](https://luk3yx.gitlab.io/minetest-formspec-editor/): A great online tool with drag and drop that allows you to import and export formspecs in different versions
-   [Perlin noise tuner](https://codepen.io/treer/pen/gOPZyov?editors=0010) Visualizes 2D Perlin noise that Minetest will generate with different noiseparams. (Emulation of Minetest Perlin noise can be wrong in extremes/edge-cases due to precision of JavaScript number type)

#### Translation

-   [update_translations](https://github.com/FaceDeer/update_translations): Python script to create and update mod translation files (\*.tr)
-   [minetest_check_translations](https://codeberg.org/Wuzzy/minetest_check_translations): Python script to check mod translation files for syntax errors
-   [findtext.lua](https://notabug.org/pgimeno/minetest/src/translation-toolchain/util/findtext.lua): Create mod translation template (buggy!) ([see also](https://forum.minetest.net/viewtopic.php?f=47&t=23330))
-   [updatetext.lua](https://notabug.org/pgimeno/minetest/src/translation-toolchain/util/updatetext.lua): Update mod translation template (buggy!) ([see also](https://forum.minetest.net/viewtopic.php?f=47&t=23330))

[Category:Misc](Category:Misc "wikilink")
