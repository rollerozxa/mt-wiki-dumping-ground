**Git is a version control system.** It handles projects as *repositories* (often called *repos*) with a history. The history consists of commits. A commit is a change to the code with the description.

Git allows people to create forks of a repository. A fork is a new repository with the same history and content, but when committing to the fork, the main repository is not affected. This allows developers to work on projects without access to the main repo.

To bring other peoples work into the main repository, they can open a pull request. This can easily be done in the GUI of GitHub.

Git also has the ability to create branches in one repository. This allows developers to work on multiple features at the same time.

Minetest has two repositories: the engine is in the "minetest" repository, whereas the main game, Minetest Game, is in the "minetest_game" repositories. The repositories are hosted on GitHub.

## Use Git to get the latest updates {#use_git_to_get_the_latest_updates}

You can use Git to get the latest updates of Minetest in a RUN_IN_PLACE version. To do this, clone the main repository:

``` bash
# Get the engine
git clone https://github.com/minetest/minetest.git # Creates a directory called "minetest" with all the content.

# Get Minetest Game
cd minetest/games/
git clone https://github.com/minetest/minetest_game.git # Creates a directory called "minetest_game" with all the content.
```

To get the latest updates of the engine (you have to compile Minetest after this), do this in the cloned Minetest directory:

``` bash
git pull
```

To get the latest updates of Minetest Game:

``` bash
cd games/minetest_game/
git pull
```

## Using Git for development {#using_git_for_development}

### Creating a fork {#creating_a_fork}

```{=mediawiki}
{{Template:Note|In this example only the engine repository is used, but you can do the same with the minetest_game repository.}}
```
To use Git for development, you have to fork the main repository on GitHub first. After this, you can clone your repository:

``` bash
git clone git://github.com/Your_user_name_on_GitHub/minetest.git #Creates a directory called "minetest" with all the content
cd minetest/
```

### Coding a feature or a bug fix {#coding_a_feature_or_a_bug_fix}

Create a new branch to code your feature (be sure to give them good names (not just "patch" or "fix")):

``` bash
git checkout -b feature_branch
```

Now, you can code your feature or bug fix. If you are done, you have to commit your changes and push them to your repository on GitHub:

``` bash
git add . # Adds all changes; alternatively you can specify single files
git commit -m "Your feature"
git push origin feature_branch
```

Commit messages should start with a capital letter and use the present tense. They should sum up the feature/fix.

You can now access your code in your repository on GitHub and open a pull request for it to the main repository.

To go back to your master branch do:

``` bash
git checkout master
```

If you have to update your feature, do:

``` bash
git checkout feature_branch
# Do changes
git add .
git commit -m "Update message"
git push origin feature_branch
```

This will automatically be added to the pull request (if there is one).

### Updating your fork {#updating_your_fork}

You can use Git to update your Minetest fork with the latest changes of the official repository.

#### Prerequisites

If you haven\'t already you need to tell git where to get the changes.

``` bash
git remote add upstream https://github.com/minetest/minetest
cd games/minetest_game
git remote add upstream https://github.com/minetest/minetest_game
```

#### Updating the fork {#updating_the_fork}

You need to download the changes of the main repository first and then merge them.

**For the engine:**

``` bash
# Get the changes
git fetch upstream

# Merge the changes
git rebase upstream/master

# Push the result to GitHub
git push
```

**For the game:**

``` bash
cd games/minetest_game
# Get the changes:
git fetch upstream

# Merge the changes
git rebase upstream/master

# Push the result to GitHub
git push
```

\

## Tips

Simplify making commits:

``` bash
echo 'git add -A && git commit -m "$*

$(git status -s)"' | sudo tee /usr/local/bin/gitdirectcommit && sudo chmod +x /usr/local/bin/gitdirectcommit
```

to make a commit, just do changes in your repository and run e.g.

``` bash
gitdirectcommit my changes
```

Quotes are not needed and the files (their path relative to the repository path) which are changed are annexed to the end of the commit message automatically.\

## See also {#see_also}

-   [*Pro Git* (book)](http://git-scm.com/book)
-   [The big git and github Pull Request thread on the forums](https://forum.minetest.net/viewtopic.php?f=3&t=14262)

[Category:Git](Category:Git "wikilink")
