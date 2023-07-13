This page contains information useful for developing Minetest for the Steam Deck. Contact rubenwardy with any questions.

## Usage as an end user {#usage_as_an_end_user}

See [rubenwardy\'s blog post](https://blog.rubenwardy.com/2022/12/02/minetest-steam-deck/)

## Installing a dev version {#installing_a_dev_version}

### Uploading an AppImage using Deck DevKit {#uploading_an_appimage_using_deck_devkit}

Source: <https://partner.steamgames.com/doc/steamdeck/loadgames>

-   Name: MinetestAppImage
-   Local folder: `/home/user/dev/tmp/minetestdeck/`
-   Upload filtering \[Include only\] `Minetest-dev.AppImage`
-   Start command: `Minetest-dev.AppImage`
-   Save config
-   Click Upload
-   On deck: `cp /usr/lib/libcrypto.so ~/devkit-game/MinetestAppImage/libcrypto.so.1`
-   Minetest should now be in Non-steam games as \"Devkit games: MinetestAppImage\"

Note, I had issues reusing the layout from the stable version of the game. Pressing \"apply layout\" did nothing. I needed to follow this bizarre workaround <https://www.reddit.com/r/SteamDeck/comments/10tsp1a/i_cant_change_the_default_controller_layout/>

### Uploading a build using Deck DevKit {#uploading_a_build_using_deck_devkit}

**Note: Untested**

Source: <https://partner.steamgames.com/doc/steamdeck/loadgames>

-   Name: Minetest
-   Local folder: `/home/user/dev/minetest/`
-   Upload filtering \[Include only\] `/bin/*** /builtin/*** /games/devtest/*** /games/minetest_game/*** /textures/base/pack/*** /client/shaders/***`
-   Check \"Delete remote files not present in local folder\"
-   Start command: `bin/minetest`
-   Save config
-   Click Upload
