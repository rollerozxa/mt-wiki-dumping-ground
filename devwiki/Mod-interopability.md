```{=mediawiki}
{{LuaTips}}
```
This page is aimed towards modders and lists a couple of mods and conventions which are known in the community.

The main goal is to improve interoperability between mods and reduce code duplication. Often it is better to use existing APIs rather than trying to reinvent the wheel to reduce code duplication, redundancy and overall chaos. Of course, deciding to not use existing APIs may still be a viable choice if they do not suit your needs.

This page will be a permanent work of progress since there are many different mods out there.

## Mods

One way to ensure interoperability is to know what other mods exist out there. This section will show you mods which you can use to achieve a particular task or point you to mods which might need some "special treatment" for maximum interoperability.

### Basics

This section is about mods and practices which apply to almost every mod.

#### Player physics {#player_physics}

Using `set_physics_override` to change player physical attributes is potentially dangerous! This is because it always sets the raw value of e.g. player speed, thus destroying any previous changes made by other mods. This is poison for any attempt at mod interoperability and an open invitation to race conditions and lots of other potential bugs. So please be very cautions before you use `set_physics_override`. You should take other mods into account.

There\'s a mod that solves the problem of interoperability:

-   [Player Physics API](https://content.minetest.net/packages/Wuzzy/playerphysics/) \[playerphysics\]: Lightweight API to modify player physics without interference. Setting the attributes directly is banned in this mod, instead mods can add and remove factors to speed, gravity and jump strength. The final physical attribute will be the product of all factors.

Note the idea of this mod requires that ALL mods use it. If even just one mod (except \[playerphysics\], of course) uses `set_physics_override` directly, interoperability is destroyed.

For further discussion of the problem with `set_physics_override`, see [Player physics](Player_physics "wikilink").

#### Internationalization

Since version 5.0.0, the preferred way to make mods and games translatable is with help of the Lua API with `minetest.get_translator`. No additional mod is required. Refer to `lua_api.md` or [the modding book chapter on translation](https://rubenwardy.com/minetest_modding_book/en/quality/translations.html) to learn how it works.

#### Help

"Help" refers to any form of in-game help, explanations or other things which aim to aid the player in understanding the game.

##### Help modpack {#help_modpack}

The [Help](https://forum.minetest.net/viewtopic.php?f=9&t=15912) modpack is a sophisticated in-game help system. It allows you to write help texts for almost everything; it could be used to document items, mods or even an entire game, entirely within the game only.

The following Help mods are relevant for modding:

-   [Item Help](https://forum.minetest.net/viewtopic.php?f=9&t=15912) \[doc_items\]: Allows you to add long item explanations, an usage help and automatically generated help texts for items
-   [Lookup Tool](https://forum.minetest.net/viewtopic.php?f=9&t=15912) \[doc_identifier\]: The lookup tool opens up the appropriate help entry on almost everything it punches. It almost works out of the box, but manual intervention from the modder is required for any custom entity
-   [Documentation System](https://forum.minetest.net/viewtopic.php?f=9&t=15912) \[doc\]: Low-level core API (for advanced users). Allows you to add your own entries and even entire help categories from scratch. Also has many utility functions

To get started with this modpack quickly, there\'s an example mod to demonstrate how to use these mods: [doc_example](http://repo.or.cz/minetest_doc_example.git)

### General gameplay {#general_gameplay}

#### Status effects {#status_effects}

If you want to add temporary or permanent status effects for players (such as poisoning), you can use this:

-   [Player Effects](https://forum.minetest.net/viewtopic.php?f=11&t=9689) \[playereffects\]
-   [Player Monoids](https://forum.minetest.net/viewtopic.php?f=11&t=14895) \[player_monoids\]

#### RPG-like elements {#rpg_like_elements}

This includes things usually found in RPGs (role-playing games) like stats, leveling up, experience points, quests, etc.

-   [Mana](https://forum.minetest.net/viewtopic.php?f=11&t=11154) \[mana\]: Adds a new player attribute (mana), could be used as resource for magic spells, weapons, tools, etc.
-   [Sunburn](https://forum.minetest.net/viewtopic.php?f=9&t=13199) \[sunburn\]: Makes sunlight dangerous, by causing sunburn (damage in direct sunlight)
-   [SkillsFramework](https://forum.minetest.net/viewtopic.php?f=9&t=11406) \[skillsframework\] Skills
-   [Quest framework](https://forum.minetest.net/viewtopic.php?f=11&t=11265) \[quests\]: Quest framework
-   [sys4_quests](https://forum.minetest.net/viewtopic.php?f=9&t=14222) \[sys4_quests\]: Another quest framework, depends on \[quests\]

#### Mobs

Mobs are non-player creatures to interact with the world, they may be peaceful or attack on sight. Usually they are based on entities. Minetest does not support mobs natively, but if you still want to add custom mobs, you have various mods to choose from, each of them with somewhat different priorities. Most APIs are not compatible which each other, so in general you should only use the API which suits your needs in the best way. If you want to create a game, you have to be very careful with interoperability.

Note: Only mature mods with their own dedicated API are listed. Note: There are multiple mods with the short name "mobs", you can only use one of these mods at once.

-   [Mobs Redo](https://content.minetest.net/packages/TenPlus1/mobs/) \[mobs\] Fork of Simple Mobs, is backwards-compatible to Simple Mobs
-   [Mobkit](https://content.minetest.net/packages/Termos/mobkit/) \[mobkit\]
-   [Creatura](https://content.minetest.net/packages/ElCeejo/creatura/) \[creatura\] An animal focused API

See also: [Mobs page on the Community Wiki](http://wiki.minetest.net/Mobs)

#### Protection

"Protection" refers to a way to own a certain area in the world and preventing other players to make any changes in this area.\
There are mods which add protection nodes, which protect within a range around the placed node, and mods which allow protecting whole chunks.

Mods should use `minetest.is_protected` when testing whether a player owns a specific position. Protection mods simply override this function.

This section needs to be finished. Notes for general interoperability requirements/recommendations should be added by someone more experienced.

#### Other gameplay concepts {#other_gameplay_concepts}

-   [Achievements](https://content.minetest.net/packages/rubenwardy/awards/) \[awards\]: API for adding your own achievements
-   [Treasurer](https://content.minetest.net/packages/Wuzzy/treasurer/) \[treasurer\]: API for randomly selecting a treasure out of a pool of previously registered treasures

#### Testing for pipeworks fake players {#testing_for_pipeworks_fake_players}

[Pipeworks](https://content.minetest.net/packages/VanessaE/pipeworks/) \[pipeworks\] provides automation and creates fake players for that purpose. If you need to, these can be distinguished from real players by the field `is_fake_player` in their definition.

### Nodes

#### Templates

Many mods offer convenience functions to add new new nodes which follow a simple template, like a fence made from a different kind of wood.

-   Standalone mods
    -   [Furniture](https://forum.minetest.net/viewtopic.php?f=11&t=14910) \[furniture\]: Tables, chairs and stools
    -   [more shelves](https://forum.minetest.net/viewtopic.php?f=11&t=15724) \[moreshelves\]: Shelves
    -   [flower pots](https://forum.minetest.net/viewtopic.php?f=9&t=15265) \[flowerpots\]: Flower pots
    -   [Minetest Game Plus](https://forum.minetest.net/viewtopic.php?f=9&t=15731) \[mtg_plus\]: Cups
    -   [corrupt shapes](https://forum.minetest.net/viewtopic.php?f=9&t=15399) \[csh\]: Additional shapes: Cylinders and related shapes
    -   [mymillwork](https://forum.minetest.net/viewtopic.php?f=11&t=10425) \[mymillwork\]: Additional shapes: Crown moulds, beams, columns, baseboards
    -   [kaeza\'s/VanessaE\'s signs library](https://forum.minetest.net/viewtopic.php?f=11&t=13762) \[signs_lib\]: Advanced signs
    -   [Biome library](https://forum.minetest.net/viewtopic.php?f=11&t=12999) \[biome_lib\]: Growing plants
    -   [Display API](https://github.com/pyrollo/display_api) \[display_api\]: Nodes with a dynamic display, e.g. digital clocks. See [Display Modpack](https://forum.minetest.net/viewtopic.php?f=11&t=13563)
    -   [Font API](https://github.com/pyrollo/font_api) \[font_api\]: Make textures from text and fonts. See [Display Modpack](https://forum.minetest.net/viewtopic.php?f=11&t=13563)
    -   [Signs API](https://github.com/pyrollo/signs_api) \[signs_api\]: Combining Font API and Display API, easy creation of any kind of signs. See [Display Modpack](https://forum.minetest.net/viewtopic.php?f=11&t=13563)
-   [Minetest Game](https://content.minetest.net/packages/Minetest/minetest_game/)
    -   \[default\]: Fences
    -   \[beds\]: Beds
    -   \[doors\]: Doors and trapdoors
    -   \[farming\]: Farming plants
    -   \[stairs\]: Stairs and slabs
    -   \[tnt\]: Explosives
    -   \[walls\]: Walls
    -   \[xpanes\]: Panes

#### Modifications

These mods extend the possibilities of node definitions:

-   [Walkover](https://content.minetest.net/packages/lordfingle/walkover/) \[walkover\]: Adds callback function which is triggered when a player walks over the node

#### Causing explosions {#causing_explosions}

Whenever you want to cause an explosion which affects other nodes, make sure you to try to call the node\'s `on_blast` method first before using your own explosion handling. The reason for this is that e.g. your explosion could otherwise destroy a node which is normally immune to explosions.

#### Suffering explosions {#suffering_explosions}

If you are happy with what explosions caused by other mods do to your block (e.g. destroying it), you don\'t need to do anything. If you want to customize the behavior, define the on_blast method.

#### Indestructible nodes {#indestructible_nodes}

Making a block truly indestructible in normal gameplay is not really obvious, so here\'s a tested and proven template which should prevent all well-programmed mods from destroying your block accidentally:

     minetest.register_node("example:indestructible", {
             description = "Indestructible Block",
             is_ground_content = false,
             on_blast = function() end,
             on_destruct = function () end,
             can_dig = function() return false end,
             diggable = false,
             drop = "",
     })

Note: There is no way a node can "defend" against `VoxelManip`. If a mod for gameplay directly uses this without any checking, this is probably a bug in this mod, rather than the mod with the indestructible block.

#### Destroying nodes {#destroying_nodes}

Be very careful when you want to destroy a node by calling `minetest.remove_node` directly. There is no way a node can "defend" against such a call, so this needs to taken into consideration for interoperability.

You should only use `minetest.remove_node` directly for technical and/or debugging mods, but for actual gameplay, you should do some checks first. Otherwise, you might end up accidentally destroying blocks which are supposed to be indestructible (see above).

Recommendation: First check if any of the node\'s callback functions are appropriate to be called (e.g. call on_blast when you create an explosion). If you want to destroy a block in another manner, first check if the node is not indestructible (checking fields like `diggable` and other fields mentioned above might usually work). If the block does not seem to be indestructible, calling `minetest.remove_node` should normally be safe.

#### Preventing pistons from pushing or pulling your node {#preventing_pistons_from_pushing_or_pulling_your_node}

The [Mesecons](https://forum.minetest.net/viewtopic.php?f=11&t=628) modpack has pistons which are able to push and pull other blocks around. By default, this happens for pretty much all blocks. If you want to prevent this behavior for your block, you have to explicitly exclude your block.

To do this for a node "example:myblock", add `mesecons_mvps` to your optional dependencies in `mod.conf` and the following code after the registration function of your node:

    if minetest.global_exists(mesecon.register_mvps_stopper) then
            mesecon.register_mvps_stopper"example:myblock"
    end

### Interface

#### Showing status information {#showing_status_information}

If you create a new player attribute/stat (like health) and you want to expose it to the interface somehow, you can use:

-   [HUD Bars](https://content.minetest.net/packages/Wuzzy/hudbars/) \[hudbars\]: Add your own indicators like the "hearts" for health. Displayed in either a "progress bar"-like fashion, the classic statbars like in vanilla Minetest or a variant of the classic statbars with a simple background

#### Messages

For showing messages to players.

-   [Central Message](https://forum.minetest.net/viewtopic.php?f=9&t=12720) \[central_message\]: Simple API to show messages at the center of the screen for a short time

#### Inventory menu enhancements {#inventory_menu_enhancements}

Some mods enhance or change the inventory menu and provide an API to add buttons and other things to the inventory menu. This can come in handy if you have made your own formspec and seek for ways to access them.

-   Simple Fast Inventory \[sfinv\]: Part of Minetest Game, you can add custom tabs to the inventory menu. See <https://rubenwardy.com/minetest_modding_book/en/chapters/sfinv.html>
-   [Simple Fast Inventory Buttons](https://forum.minetest.net/viewtopic.php?f=9&t=16079&p=244441) \[sfinv_buttons\]: Adds a new tab "More" to the inventory menu of Minetest Game, in which mods can add buttons for doing any action you want
-   [Unified Inventory](https://forum.minetest.net/viewtopic.php?f=11&t=12767) \[unified_inventory\]: Supports adding buttons for doing any action you want (undocumented API)
-   [Inventory++](https://forum.minetest.net/viewtopic.php?f=11&t=6204) \[inventory_plus\]: Supports adding buttons for adding "sub-menus" to the inventory menu. This mod does not work well together with newer versions of Minetest Game but it may still be usable for some other games

#### Formspecs

-   [Smart Formspecs](https://forum.minetest.net/viewtopic.php?f=11&t=7553&p=116028) \[smartfs\]: Simplification and enhancement of the formspec API
-   [Select Item](https://forum.minetest.net/viewtopic.php?t=20109) \[select_item\]: Dialog to select an item

### Decorational

#### Colors

If your mod provides blocks in multiple colors, there are various mods and conventions to consider for ensuring the best interoperability.

It is strongly recommended you only use colors which follow an agreed-upon set of predefined colors. A basic set of 15 colors is defined in the "dye" mod in Minetest Game. Read the source code to learn more. Other mods provide an extended set of dyes. The mod [Unified Dyes](https://forum.minetest.net/viewtopic.php?id=2178) \[unifieddyes\] extends the set of colors to 90 colors.

Other specific mods for consideration:

-   Colorization by crafting: Consider if you want to add crafting recipes by adding a dye and your item (like wool in Minetest Game, read it source code to learn how it works)
-   [Color Machine](https://forum.minetest.net/viewtopic.php?f=11&t=6670) \[colormachine\]: Coloring support will be provided by the mod itself, as long as the authors update the mod. Your task would to to make sure that the names of the textures and nodes follow naming conventions
-   [Paint Roller](https://forum.minetest.net/viewtopic.php?f=11&t=9170) \[paint_roller\]: Use the API of this mod to enable the paint roller to colorize your blocks
-   [mypaint](https://forum.minetest.net/viewtopic.php?f=11&t=14274) \[mypaint\]: This mod allows to paint in arbitrary colors using paint buckets and brushes; internally it seems to work very different than the other mods

#### Other

-   [Font API](https://github.com/pyrollo/display_modpack/tree/master/font_api) \[font_api\]: A library for displaying text on entities. Part of [Display Modpack](https://forum.minetest.net/viewtopic.php?t=19365)

## Groups

Another way for interoperability is by using groups. If used properly, they can greatly increase ease-of-use when other modders want to work with our mod. The rule of thumb here is that you generally use groups which are already widely used if you can. Only if your needed group does not exist, you should define your own group.

On this wiki, the [Groups/Custom groups](Groups/Custom_groups "wikilink") page tries to document the usage of groups and lists some commonly used groups. If your mod defines its own groups and they are \*not\* just to be used internally, please edit the Groups page to help other modders.

[Category:Mods](Category:Mods "wikilink")
