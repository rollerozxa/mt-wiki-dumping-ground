```{=mediawiki}
{{LuaTips}}
```
## Introduction

Managing and declaring dependencies are a core part of modding for Minetest. However, sometimes it is more desireable to write mod code which is only *optionally* dependent on another mod, not mandatorily, epspecially if only a small part of the actual code is actually dependent on the other mod. One example are crafting recipes; often, they are not that important, since the core part of a mod, namely, the registered items, can still function without them.

This short guide describes how to convert any mandatory dependency to an optional one and briefly discusses this technique.

\_\_TOC\_\_

## The "standard" technique {#the_standard_technique}

One way to make a dependency optional is to insert the code which causes the mandatory dependency into a simple `if` block and optionally insert fallback code into the `branch`

With `minetest.get_modpath` it can be checked whether a mod is present and loaded, as it will return nil if not.

    if minetest.get_modpath("example") then
        --[[ Insert code which depends on the example mod here ]]
    else
        --[[ Optionally insert fallback code here when the mod is not available.
             If you intend no fallback, you can leave this section empty. ]]
    end

In addition, you need to add the depending mod into your mod\'s optional dependencies list in `mod.conf`.

### Examples

The following example code has a mandatory dependency on the default mod, because the crafting recipe requires items from the default mod:

    minetest.register_craft({
        output = "example:awesome_item",
        recipe = {
            { "default:wood", "default:stone", "default:mese" },
            { "", "default:wood", "" },
        }
    })

By putting above code into the template, we get the following code, which is now *optionally* dependent on the default mod.

    if minetest.get_modpath("default") then
        minetest.register_craft({
            output = "example:awesome_item",
            recipe = {
                { "default:wood", "default:stone", "default:mese" },
                { "", "default:wood", "" },
            }
        })
    end

Note that the `else` has been left out because we don\'t intend to use any fallback option here.

## Discussion

This technique is useful when you have small and simple chunks of code causing a mandatory dependency. This technique is especially useful for crafting recipes, as they are often not that neccessary; other mods may be more interested in the registered items and may even register their own crafting recipes.

In theory, all dependencies can be made optional using this technique. But sometimes, it more practical to keep a mandatory dependency, especially if large portions of code are dependent on another mod and writing a fallback option would be unreasonable.
