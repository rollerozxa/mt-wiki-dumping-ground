## Checklist

    - Feature freeze
      - [ ] Announced
      - [ ] Release candidate advertised
      - [ ] Weblate sources regenerated and strings frozen
    - Pre-release (day of release)
      - [ ] Autogenerate files
      - [ ] Correct special strings on Weblate (`LANG_CODE`)
      - [ ] Update translations from Weblate
      - [ ] Update changelog
      - [ ] Update credits
      - [ ] (Minor/Major) Write blog post
      - [ ] Ensure protocol version codes have been bumped
    - Release
      - [ ] Run bump_version and verify
      - [ ] Push new tag
      - [ ] Build and upload Windows version
      - [ ] Build and upload Android version
      - [ ] (Minor/Major) Publish blog post
      - [ ] Create forum topic
      - [ ] Notify rubenwardy to announce on Twitter, etc.
      - [ ] Notify downstream maintainers

## Feature Freeze {#feature_freeze}

### Announce a feature freeze {#announce_a_feature_freeze}

(*Skip for patch releases*) Usually, a **feature freeze for one week is announced in #minetest-dev**. New features aren\'t accepted in this time and people focus on finding and fixing bugs. To find high priority issues faster, consider linking a release candidate binary to get more test results (for master: GitLab pipeline). This release candidate is usually also posted on the forums (News section).

The feature freeze and release date is set by core developers.

### Autogenerate files {#autogenerate_files}

Ensure that the `language` setting enum values contain `en`: there is no \"en\" directory, but Minetest supports it. Autogenerate config file examples such as `minetest.conf.example` (see bottom of `builtin/mainmenu/dlg_settings_advanced.lua`), etc.

Also update [translation](Translation "wikilink") templates (engine, builtin and any default games). Note that before you regenerate translations, you most likely want to [import existing changes](#Update_translations_from_Weblate "wikilink") first.

Look into the `util` sub-directory for tools like `updatepo.sh`.

### Update source strings on Weblate {#update_source_strings_on_weblate}

Make sure that the source strings on hosted.weblate.org are up-to-date. Please also notify translators so they can start working.

Do **not** just blindly run the scripts without checking. Check if the source strings on Weblate were *actually* updated. A simple way to check is to look at any translation that was at 100% (on Weblate) *before* the update. After the update, the percentage should drop because of new strings. If it didn\'t drop but you know there are new strings in Minetest, this means the update failed.

## Before releasing {#before_releasing}

### Verify special translation strings {#verify_special_translation_strings}

The translation files contain a special string: [LANG_CODE](https://hosted.weblate.org/translate/minetest/minetest/en/?q=LANG_CODE&checksum=&offset=1#translations) (see [Translating](Translating "wikilink")).

Verify that all \*.po files have a valid value for these strings because translators frequently misunderstand them and enter an invalid value. Fix any invalid values on Weblate by either entering the correct one or by removing the bad translation.

### Update translations from Weblate {#update_translations_from_weblate}

**How to do this** -\> [Translating#How_to_merge_translations_from_Hosted_Weblate](Translating#How_to_merge_translations_from_Hosted_Weblate "wikilink")

If doing a backported release, you can use the following command to cherry-pick all translation commits from weblate:

    git log --reverse --pretty=format:"%h" $BASE..weblate/master -- po | xargs -L1 git cherry-pick

BASE is the commit on master to start from when looking for translation commits. This commit should be newer than the last translation commit on the backport-X branch.

### Update changelog {#update_changelog}

Changelog can be found [here](Changelog "wikilink").

### Ensure protocol version codes have been bumped {#ensure_protocol_version_codes_have_been_bumped}

If not a patch release: ensure that PROTOCOL_VERSION has been increased since the last release. ([deciding discussion](https://github.com/minetest/minetest/issues/11603))

If formspec features have been added: ensure LATEST_FORMSPEC_VERSION has been increased since the last release.

## The process {#the_process}

This is mostly done by several core developers.

### Update version in source {#update_version_in_source}

The process with patch releases is slightly different but the script will take care of it correctly in any case.

-   **Define** a new version number by running `util/bump_version.sh`. Verify that this script correctly:
    -   changes TRUE to FALSE for the line *set(DEVELOPMENT_BUILD TRUE)* in CMakeLists.txt
    -   updates the version number and release date in misc/net.minetest.minetest.appdata.xml
    -   increments *versionCode* in build/android/build.gradle by 2
    -   and commits.
-   **Tag** the version in local git (the script does this) to allow the cmake versioning script to remove the git hash from the version. Do not push the tag to GitHub yet.
-   **Build**, get newest minetest_game, run and check if the thing seems to be working.

### Build Windows version {#build_windows_version}

-   These are built for 32-bit and 64-bit using either Visual Studio or MinGW. (sfan5 typically provides MinGW builds)
-   If at all possible, debug builds (for both MSVC and MinGW) should be produced along with the regular release builds.
-   As of April 2018, this GitLab pipeline can provide win32/64 packages as a **fallback**: <https://gitlab.com/minetest/minetest/pipelines>
    -   Since 5.0, the 64-bit builds are compiled with GC64-enabled LuaJIT (to avoid [#2988](https://github.com/minetest/minetest/issues/2988))

→ → → ***Make sure that the Windows builds work before continuing to do anything*** ← ← ←

#### Mini checklist of things to test {#mini_checklist_of_things_to_test}

*Note*: Don\'t cheat on this by testing in Wine, it has happened that things crash/break in wine while they are fine on real Windows.

-   click some menu buttons
-   create world with MTG, enter it, exit back to menu
-   open multiplayer tab, attempt to join a server
-   install a package from CDB, uninstall it again
-   enable dynamic shadows, join ingame and look

### Upload packages to somewhere {#upload_packages_to_somewhere}

-   All official builds are hosted at Github: <https://github.com/minetest/minetest/releases>
-   Windows builds are uploaded by whoever builds them
-   The macOS build is created by [Github Actions](https://github.com/minetest/minetest/actions/workflows/macos.yml)
    -   you will only be able to grab the build **after** the release has been pushed
    -   download the artifact, unpack it once, you should have a `minetest-5.x.x-osx.zip`. This is then uploaded.
-   Android APKs are also uploaded here when they\'re done

### Update branches and tags of minetest and minetest_game on GitHub {#update_branches_and_tags_of_minetest_and_minetest_game_on_github}

Tagging is handled by the script for the engine. For MTG you can just create an annotated tag pointing to the latest commit and push it.

The new release should be merged to the stable-5 branch on both minetest and minetest_game. **Its important to merge, and not just rebase**, so that git describe works.

#### The problem on the stable-5 branch {#the_problem_on_the_stable_5_branch}

Usually, merging releases onto the stable branch just consists of adding the commits to the branch, as it contains direct ancestors of master commits, and git can do a fast forward. During release/freeze of 5.0.1 (both minetest and minetest_game), the ancestor rule has been broken.

Therefore, you\'ll generate merge commits, but this shouldn\'t be a problem. In the case of merge conflicts, ensure that the changes on stable-5 are all discarded in favor of the tagged commit at master, by doing a merge commit like:

`   git checkout version-tag`\
`   git merge -s ours origin/stable-5`\
`   git push origin HEAD:stable-5`

### Update Launchpad stable build to get Ubuntu builds for the new version {#update_launchpad_stable_build_to_get_ubuntu_builds_for_the_new_version}

celeron55, rubenwardy, and ShadowNinja have access.

Process:

-   Go to [minetest-c55/upstream](https://code.launchpad.net/~minetestdevs/minetest-c55/+git/upstream) and [minetest-c55/upstream_game](https://code.launchpad.net/~minetestdevs/minetest-c55/+git/upstream_game) and click \"start import\".
-   First, find out the commit hashes of the minetest and minetest_game git repos corresponding to the release.
-   Now visit the [recipe](https://code.launchpad.net/~minetestdevs/+recipe/minetest-stable).
-   At the bottom of the page there is a section called \"Recipe contents\". In this section you need to edit the recipe. Make sure you update:
    -   The version number at the end of the first line. Doing this is a must otherwise there would be duplicate packages which would lead to a fail. The version number has a format like `5.1.1-ppa0`. You should keep the ppa postfix so that it\'s easy to differentiate the package by origin, ppa or upstream Debian.
    -   The commit hash of the main minetest repo in the second line.
    -   The commit hash of the minetest_game repo in the last line.
-   Check whether everything has been updated correctly.
-   Click the green \"Request builds\" link, enable the newer distro versions, and click confirm.

The build has two steps: first it assembles the source code and uploads it, then it builds the code. If the first step completed successfully but the second one failed, you need to update the version number in the recipe (e.g. 1.2.3-ppa1) before rebuilding.

### Build and publish Android APK {#build_and_publish_android_apk}

**nerzhul** or **rubenwardy** have access to Google Play. Both also hold the signature keys for the app.

#### Signing APKs for the Play Store {#signing_apks_for_the_play_store}

-   Run the Android build process as usual
-   **Sign the package:** `"~/Android/Sdk/build-tools/29.0.3/apksigner" sign --ks keystore-minetest.jks .../app-armeabi-v7a-release.apk`
-   EDIT: You can use Android Studio to generate bundles/apks if you upgrade the jks

#### Creating Native Debug Symbols {#creating_native_debug_symbols}

-   Copy native/build/intermediates/ndkBuild/release/obj/local/ to new folder
-   Remove all but .so files from new folder
-   Check the .so contain debug info using the file command
-   `zip -r symbols.zip .`

## After releasing {#after_releasing}

### Reenable -dev version suffix {#reenable__dev_version_suffix}

(*Skip for patch releases*) Check that the util/bump_version.sh script did the following steps:

-   **Update** the version number in CMakeLists.txt and the titles of doc/client_lua_api.txt and doc/menu_lua_api.txt
-   **Change** FALSE to TRUE for the line *set(DEVELOPMENT_BUILD FALSE)* in CMakeLists.txt. This will add the -dev suffix to the version name again.
-   **Commit.**

### Write a release notice {#write_a_release_notice}

-   Don\'t forget to edit the minetest.net page ([Repo](https://github.com/minetest/minetest.github.io)).
    -   currently you will need to update machine readable metadata in \_data/release.yml and downloads.html itself.
-   Post a new topic in the [News section](https://forum.minetest.net/viewforum.php?f=18) of the forum. **See [Changelog](Changelog "wikilink").**
    -   It is customary to sticky the newest release topic and lock older ones.
-   Add a new post to the [blog](https://github.com/minetest/blog/)
-   Announce the release on the [Twitter account](https://twitter.com/MinetestProject). rubenwardy has access.

### Update wiki version template {#update_wiki_version_template}

There is a special wiki template containing the version number at [1](https://wiki.minetest.net/Template:Version), used to make updating various wiki pages by hand less tedious. Update it so it includes the latest version.

### Notify known package maintainers {#notify_known_package_maintainers}

-   **Arch Linux**/Manjaro: can be flagged outdated on the [package page](https://archlinux.org/packages/community/x86_64/minetest/)
-   **Alpine Linux**: can be flagged outdated on the [package page](https://pkgs.alpinelinux.org/package/edge/community/x86_64/minetest)
-   **Debian**/Ubuntu: has [own version tracking](https://tracker.debian.org/pkg/minetest), no need to contact
-   **F-Droid**: has volunteer maintainers, if nobody notices consider opening an issue [here](https://gitlab.com/fdroid/fdroiddata/-/issues)
-   **Snap**: open an issue (or contribute) [here](https://github.com/snapcrafters/minetest)
-   **Flatpak**: open an issue (or contribute) [here](https://github.com/flathub/net.minetest.Minetest)
-   **Gentoo**: has [own version tracking](https://packages.gentoo.org/packages/games-action/minetest), no need to contact

You can find out how quick various distro are to adopt new versions [here on repology](https://repology.org/project/minetest/history).

### ContentDB

#### Add a new version {#add_a_new_version}

Add the new version to the drop-down list of compatible Minetest versions that authors can select for their things.

Note that CDB tells Minetest versions apart by their protocol version so this is obviously not applicable to patch releases.

People who have access: rubenwardy + ???

#### Package devtest {#package_devtest}

The Minetest Game package on ContentDB makes a new release automatically when a new tag is made.

The [Development Test](https://content.minetest.net/packages/Minetest/devtest/) package however needs to be released **manually**. Make a new release, upload a ZIP file with Development Test like it looks like the Minetest source tree in the stable branch, and set the minimum and maximum Minetest versions to the Minetest version it is intended for.

[Category:Core Engine](Category:Core_Engine "wikilink") [Category:Rules and Guidelines](Category:Rules_and_Guidelines "wikilink")
