```{=mediawiki}
{{LuaTips}}
```
This page lists the various groups which are used in various games and mods, but are more or less specific to those. Keep in mind the meaning of most of these groups is not "standardized", the actual meaning depends on the particular game/mod implementation. For generic groups, see [Groups#Special groups](Groups#Special_groups "wikilink") and [Groups/Shared groups](Groups/Shared_groups "wikilink").

(**Note**: If not group rating is specified, a rating of 1 is assumed.)

## Games

### Minetest Game {#minetest_game}

Version: 0.4.14

These are the custom groups used by Minetest Game. Many groups listed here have also been adopted by mods and games which are not directly related to Minetest Game, but be careful, the intention may not always be equal!

#### Damage groups {#damage_groups}

Used by weapons and tools, and by entities to determine damage on a successful hit.

-   `fleshy`: Living things like animals and the player. This could imply some blood effects when hitting.

Note: `fleshy` is currently the only damage group used in Minetest Game, all tools and weapons in Minetest Game only affect the `fleshy` group.

#### Digging time groups {#digging_time_groups}

The following groups determine digging times. The rating can be between 1 and 3; higher ratings result in faster digging.

-   `crumbly`: dirt, sand
-   `cracky`: tough but crackable stuff like stone.
-   `snappy`: something that can be cut using fine tools; eg. leaves, small plants, wire, sheets of metal
-   `choppy`: something that can be cut using force; eg. trees, wooden planks
-   `explody`: Especially prone to explosions
-   `oddly_breakable_by_hand`: Can be added to nodes that shouldn\'t logically be breakable by the hand but are. Somewhat similar to dig_immediate, but times are more like `{[1]=3.50,[2]=2.00,[3]=0.70}` and this does not override the speed of a tool if the tool can dig at a faster speed than this suggests for the hand.

#### ABMs

Groups which are mainly used by ABMs for block interaction:

-   `tree`: node is the trunk of a tree (like "Tree", "Jungle Tree", etc.). Prevents blocks in `leafdecay` group from decaying.
-   `leafdecay`: node decays if no block in the `tree` group is in a radius of the rating.
-   `leafdecay_drop`: if node decays because of `leafdecay`, it will always drop as an item.
-   `flora`: node will slowly spread when placed on a "soil"-group node but it will turn into a dry shrub when it is placed on desert sand. Usually used for small plants.
-   `soil`: node is some sort of soil which supports the growth and spread of plants of all sorts, including farmable plants
    -   `soil=1`: basic soil: Supports growth of saplings and spread of `flora` nodes. These plants can of course also grow on soil of higher ratings.
    -   `soil=2`: dry farming soil: Node has been cultivated by a hoe, but is not wet yet.
    -   `soil=3`: wet farming soil: Supports the growth of farming plants like wheat and cotton
-   `cools_lava`: node turns adjacent lava sources into obsidian and adjacent flowing lava into stone
-   `lava`: node is lava.
-   `water`: node is water. Used by the buckets mod. Also acts like `cools_lava`

##### Fire

Used by the `fire` mod:

-   `flammable`: can be set on fire. Higher values mean more flammable.
-   `puts_out_fire`: if in a node with this group is near fire the fire gets extinguished.
-   `igniter`: sets flammable nodes on fire. Rating defines the radius.

#### Crafting

These groups are *mostly* used in crafting recipes:

-   `sand`: node is sand.
-   `stone`: node is stone.
-   `wood`: node is a type of wooden planks.
-   `wool`: node is wool (default rating: `1`).
-   `leaves`: node represents leaves or needles from a tree.
-   `stick`: item is a stick.
-   `coal`: item is some kind of coal lump.
-   `water_bucket`: Item is a bucket filled with some kind of water.

##### Color

-   `dye`: item is a dye.
-   `basecolor_white`, `excolor_yellow`, `unicolor_dark_blue_s50`, and the like: Declares the color of an item. See [game_api.txt](https://github.com/minetest/minetest_game/blob/master/game_api.txt).

**Note**: The various color groups in the dyes mod can be considered as a standard in the modding community. They are also in widespread use, independently of Minetest Game

#### Functional

-   `not_in_creative_inventory`: the item is not added to the creative inventory. **Note**: This group is also widespread across other games and mods.
-   `vessel`: item is a vessel (small liquids container) and can be put in a [vessels shelf](http://wiki.minetest.net/Vessels_Shelf).
-   `book`: item is a book and can be put in a [bookshelf](http://wiki.minetest.net/Bookshelf).

#### Informational

Misc. groups where their main purpose seems to group similar things together. They may be used for many different tasks

-   `sapling`: node is a sapling.
-   `plant`: node is a small-sized plant.
-   `flower`: node is a flower.
-   `pane`: node is a "pane" like Glass Pane or Iron bar. Defined by the `xpanes` mod.
-   `wall`: node is a wall, used to connect to other nodes of the wall group together
-   `bed`: node/item is a bed
-   `door`: node/item is a door
-   `liquid`: node is a liquid
    -   `liquid=2`: lava (see also: `lava`)
    -   `liquid=3`: water (see also: `water`)

### Pedology

<https://forum.minetest.net/viewtopic.php?f=11&t=9429>

Version: 3.0

-   `wet`: Node is wet, rating ranges from 1-5 (higher = wetter). If this group is not present, the node is seen as dry.
-   `oozing`: Node will slowly transfert its wetness towards neighboring "sucky" nodes. Rating does not matter.
-   `sucky`: Node is able to receive wetness from neighboring wet and oozing nodes. Rating does not matter.
-   `sun_dry`: Node will dry off over time when exposed to sunlight. Rating must be 1.

This was only a quick overview. Refer to the mod\'s README.md for a more detailed and up-to-date information.

### Realistic Suffocation {#realistic_suffocation}

<https://forum.minetest.net/viewtopic.php?f=9&t=15304>

Version: 1.0.0

This mod introduces 2 groups:

-   `disable_suffocation`: This mod won\'t add suffocation (drowning damage for solid full cube blocks) to nodes in this group (rating: 1)
-   `real_suffocation`: This group is added by this mod to all nodes it modified (added drowning damage) (rating: 1)

### Slime blocks and liquids {#slime_blocks_and_liquids}

<https://forum.minetest.net/viewtopic.php?f=11&t=10423>

Version: 1.0.0

This mod introduces one digging group:

-   `slimey`: For blocks made out of slime. Rating ranges from 1-3 (higher = more slimey)

## Editing this page {#editing_this_page}

Feel free to add your own games and mods. There are some guidelines for editing:

-   Do not add groups which are intended to be used only internally by the mod or game
-   Don\'t forget to mention the meaning of the group rating. If you omit it, we assume the group rating to be always 1 or it does not matter.
-   Specify the mod\'s / game\'s version number, if possible, so the users can see whether this page is still up to date

[Category:Misc](Category:Misc "wikilink")
