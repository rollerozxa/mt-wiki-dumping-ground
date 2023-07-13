Minetest has a scripting **API**, which is used to program games and mods, creating whole new experiences or extending existing ones.

The API is accessed using Lua, an easy-to-use programming language. Version 5.1 of Lua is used, but many people run LuaJIT for greater performance.

The only thing you will need is *basic* programming knowledge.

## Documentation

### Tutorials

The [Minetest Modding Book](https://rubenwardy.com/minetest_modding_book/) is a friendly introduction to Minetest modding and game creation, introducing you to various aspects of the API.

It is recommended you start here, even if you are already apt at programming, to get a good understanding of how Minetest mods work and are structured.

### Lua API Reference {#lua_api_reference}

The official Lua API documentation is `lua_api.md`. It\'s available as [markdown](https://github.com/minetest/minetest/blob/master/doc/lua_api.md) or [HTML](https://minetest.gitlab.io/minetest/). You can find the plaintext version in your Minetest installation, in the `doc` directory.

This is a concise description of the entire API, explaining functions, data structures, registration templates & more. The core developers of minetest maintain it, changes going through a quality control process.

Any functions not listed here are subject to change and not guaranteed to be compatible across versions, though usually they are.

### The Minetest-Docs Project {#the_minetest_docs_project}

A work-in-progress project is underway to create new, more detailed, documentation. They can be read from its [GitHub repo](https://github.com/minetest/minetest_docs/), contributions are greatly appreciated.

## Useful tools {#useful_tools}

Here are some useful tools that most modders use when making Minetest mods:

-   [Visual Studio Code](https://code.visualstudio.com/)/[VSCodium](https://vscodium.com/), powerful code editor with a [Minetest extension](https://marketplace.visualstudio.com/items?itemName=GreenXenith.minetest-tools) available for code completion.
-   [luacheck](https://github.com/lunarmodules/luacheck), static analysis tool for Lua. See [this modding book chapter](https://rubenwardy.com/minetest_modding_book/en/quality/luacheck.html) for more information on how to use it with Minetest.

## Other useful links {#other_useful_links}

-   Check out [ContentDB](https://content.minetest.net/) to see mods that have been published by the community.
-   Get mod help from the community:
    -   [Forums](https://forum.minetest.net/viewforum.php?f=47)
    -   [Discord](https://discord.gg/minetest)
    -   [Matrix](https://matrix.to/#/#minetest:tchncs.de)
    -   \...[more](https://www.minetest.net/get-involved/)
-   Suggest a mod idea in the [mod request thread](https://forum.minetest.net/viewtopic.php?f=9&t=2434).

[Category:Modding API](Category:Modding_API "wikilink")
