```{=mediawiki}
{{LuaTips}}
```
This a list of groups which are shared across multiple mods and games, or groups which have been accepted by the community to have a standard meaning.

## List of shared groups {#list_of_shared_groups}

### GUI-related {#gui_related}

-   `not_in_creative_inventory=1`: This item must not appear in the [creative inventory](https://wiki.minetest.net/index.php?title=Creative_inventory)
-   `not_in_craft_guide=1`: Crafting recipes which have this item as a result will not be shown in a [crafting guide](http://wiki.minetest.net/Crafting_guide). The item may still be shown as an ingredient.

### Colors

For dyes, the color groups used in Minetest Game have been widely accepted. If you add a dye, you should add the appropriate groups. The groups have names like `basecolor_white`, `excolor_sky_blue` and `unicolor_dark_green`.

See [game_api.txt](https://github.com/minetest/minetest_game/blob/master/game_api.txt), section "Dyes" for a detailed description and see the [dye](https://github.com/minetest/minetest_game/tree/master/mods/dye/init.lua) mod for an example implementation.

### Object classification {#object_classification}

These groups are used to classify something. The rating is assumed to be 1. These groups mean that the item in question *is* something. E.g. the group "`tree=1`" means the item *is a* tree trunk.

#### Basic nodes {#basic_nodes}

-   `tree`: Tree trunk
-   `leaves`: Leaves, (pine) needles, and the like
-   `sand`: Sand
-   `sandstone`: Sandstone
-   `stone`: Stone or stone-like materials
-   `water`: Water (source or flowing)
-   `lava`: Lava (source or flowing)

#### Additional nodes {#additional_nodes}

-   `wood`: Block of wooden planks
-   `wool`: Block of wool
-   `sapling`: Sapling
-   `flower`: Flower

### Items and tools {#items_and_tools}

-   `pickaxe`: Pickaxe
-   `shovel`: Shovel
-   `axe`: Axe
-   `sword`: Sword
-   `hoe`: Hoe (farming tool)

### Damage groups {#damage_groups}

-   `fleshy`: *De facto* considered the "default" damage group for players and mobs when a game has no sophisticated damage system and everything is considered to be in the same "damage class". Otherwise, intended for living things like animals and the player. This could imply some blood effects when hitting.

[Category:Misc](Category:Misc "wikilink")
