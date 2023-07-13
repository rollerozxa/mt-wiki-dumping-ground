```{=mediawiki}
{{UnofficialLua}}
```
See [MetaDataRef](https://minetest.gitlab.io/minetest/class-reference/#metadataref) and [Metadata](https://minetest.gitlab.io/minetest/metadata/) in the Lua API documentation.

**Warning** all metadata is sent to client, don\'t store sensitive stuff here.

## Example

This adds a chatcommand, that opens a formspecs in that you can change the color and name of an item, if you havethe rename priv.

    minetest.register_privilege("rename", {
        description = "Can rename Items and Nodes",
        give_to_singleplayer = false
    })

    minetest.register_chatcommand("rename", {
        func = function(name, param)
            minetest.show_formspec(name, "rename:renameform",
                    "size[4,4.5]" ..
                    "label[0,0;rename]" ..
                    "field[1,1.5;3,1;name;New Node/Item name;]" ..
                                    "field[1,2.5;3,1;color;New Color;]" ..
                    "button_exit[1,3;2,1;exit;Rename Now!]")
        end
    })


    minetest.register_on_player_receive_fields(function(player,
            formname, fields)
        if formname ~= "rename:renameform" then

            return false
        end

        local has, missing = minetest.check_player_privs(player:get_player_name(), {
                rename = true})

        if has then
          local itemstack = player:get_wielded_item()
          local meta = itemstack:get_meta()
          meta:set_string("description", fields.name)
                meta:set_string("color", fields.color)
          player:set_wielded_item(itemstack)

          return true
        else
            minetest.chat_send_player(player:get_player_name(), "You have no privilige to rename things :( ")
        end

    local meta = minetest.get_meta(pos)
    meta:set_string("formspec",
            "invsize[8,9;]"..
            "list[context;main;0,0;8,4;]"..
            "list[current_player;main;0,5;8,4;]")
    meta:set_string("infotext", "Chest");
    local inv = meta:[NodeMetaRef]get_inventory()
    inv:set_size("main", 8*4)
    print(dump(meta:[NodeMetaRef]to_table()))
    meta:from_table({
        inventory = {
            main = {
    [1] = "default:dirt",
    [2] = "",
    [3] = "",
    [4] = "",
    [5] = "",
    [6] = "",
    [7] = "",
    [8] = "",
    [9] = "",
    [10] = "",
    [11] = "",
    [12] = "",
    [13] = "",
    [14] = "default:cobble",
    [15] = "",
    [16] = "",
    [17] = "",
    [18] = "",
    [19] = "",
    [20] = "default:cobble",
    [21] = "",
    [22] = "",
    [23] = "",
    [24] = "",
    [25] = "",
    [26] = "",
    [27] = "",
    [28] = "",
    [29] = "",
    [30] = "",
    [31] = "",
    [32] = ""}
        },
        fields = {
            formspec = "invsize[8,9;]list[context;main;0,0;8,4;]list[current_player;main;0,5;8,4;]",
            infotext = "Chest"
        }
    })

## An excerpt from an e-mail between Minetest developers {#an_excerpt_from_an_e_mail_between_minetest_developers}

    > When I attach a Lua table to a node I seem to have a
    > choice: to store things with many calls to
    > meta.set_int/meta.set_string etc. or via meta:from_table.
    > meta:from_table lets me store an arbitrary table under "fields":
    > 
    > local meta = minetest.get_meta(pos)
    > local mt = meta:to_table()
    > 
    > len = tonumber(mt.fields.length_remaining)
    > 
    > ... only I have noticed that every key under fields can only store
    > strings. So as a general purpose 'table' store, this is not so useful.
    > I came upon this because I wanted to store the initial position of a
    > node. This requires either:
    > 
    > meta:set_int('x', pos.x)
    > meta:set_int('y', pos.y)
    > meta:set_int('z', pos.z)
    > 
    > or something like:
    > 
    > mt.fields.tail_pos = "return {" .. pos.x .. "," .. pos.y .. "," .. pos.z .. "}"
    > 
    > and then something like this to read it back:
    > tail_pos = loadstring(mt.fields.tail_pos)()
    > 
    > 
    > ...correct? Or am I missing an obvious alternate solution? The api
    > docs are not detailed on this.

    Answer by celeron55:

    For simplicity, all the fields are internally strings, and don't store
    any type information. I can see the inconvenience though.

    You shouldn't use loadstring() directly, because then somebody could
    create a world which stores arbitrary Lua code, which can eg. remove
    all files, or something similar.

    For storing Lua variables with type information, you can use
    minetest.serialize() and minetest.deserialize(). They take any
    serializable Lua table, which can contain:
    - tables
    - strings
    - numbers
    - functions that don't access anything outside of what is stored in
      the data itself and what is passed as a parameter))
    and make it into a string, and the other way around, and they
    will not execute any foreign code (unless you do certain
    complicated-ish stupid things, which probably won't happen).

    You can check out the implementation in builtin/serialize.lua.

    So if you want to store a position in the data of a node, you can use:
      local pos = {x=this, y=and, z=that}
      meta:set_string("tail_pos", minetest.serialize(pos))
    and
      local pos = minetest.deserialize(meta:get_string("tail_pos")).

    Keep in mind though that that thing isn't very largely in use, and
    there might be unnoticed problems.

[Category:Objects](Category:Objects "wikilink")
