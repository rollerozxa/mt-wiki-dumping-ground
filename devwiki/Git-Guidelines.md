This page is mostly directed to core team members with commit or triage access to upstream repositories.

For instructions on basic usage, see [Git](Git "wikilink").

For determining who is allowed to do what, see [Organisation](Organisation "wikilink").

For guidelines about overall pull request quality, see [Merging core pull requests to upstream](Merging_core_pull_requests_to_upstream "wikilink").

## Rules

### Upstream branch rules {#upstream_branch_rules}

Feature freezes take effect in the master branch, and while the freeze is active, a separate development branch (\"dev\" or some other suitable name) is made into which pull requests are merged. It is then rebased onto master once the freeze is over. However, creating the branch may be skipped (**it usually is!**) when nobody feels like merging features during a feature freeze.

The minetest and minetest_game contain the stable-0.4 branch, which has to be updated to the latest stable 0.4 series version at each release.

### Upstream pull requests {#upstream_pull_requests}

-   Don\'t mix multiple things in one commit. The same applies to codestyle cleanup.
-   People considering merging pull requests are not required to look anything up anywhere else than the pull request and its comments. If there is something blocking the merging of a pull request, the reason must be added as a comment to the pull request. This goes both ways: If you check a pull request to be mergeable, write a simple \"+1\" comment to it.

### Upstream commit rules {#upstream_commit_rules}

1.  You can push something to upstream \[1\] only if two members of the core team \[2\] agree on it. (See also [Organisation](Organisation "wikilink"))
2.  Commit messages must start with a capital letter and must be in the present tense. (look at the commit log)
3.  Do not modify history older than 10 minutes.
4.  Use rebase, not merge, to get linear history. \[3\]
5.  Do not rush with anything, unless our users\' data is about to be corrupted otherwise.

## Rule 1 in practice {#rule_1_in_practice}

Tell people openly what you do, and if someone finds a problem in what you do, allow resolving to take its time.

If you have a **small patch**, fixing some compiler error or other trivial mistake, notify about fixing it on #minetest-dev, wait for 5\...15 minutes and push it. To save time, you should notify when *finding* the problem, not when *having it fixed*. If someone asks something about it, delay pushing and link the patch \[4\] or tell whatever else people want to know.

Rule 1 is **only** applied to the minetest/minetest and minetest/minetest_game repositories. For the other repos apply some common sense: Check who last worked on it or who wrote most of the code (if applicable), consider consulting them for changes especially if they\'re large. If nobody has cared about a repo for a long time you don\'t have to worry either.

#### Notes

\[1\] Upstream is at <https://github.com/minetest/minetest>

\[2\] The team: <https://github.com/orgs/minetest/people>

\[3\] On Github, press the \"Rebase and merge\" button. Of course you can rebase a remote branch in a local repository for more in-depth tools. There\'s also the ancient workflow of appending .patch to the pull request URL, getting into your project directory and doing `git am ``<patch>`{=html}. Similarly for single commits.

\[4\] Patches can be linked using a pastebin or by using GitHub (pull request or not).

## Issue and Pull-Request Management {#issue_and_pull_request_management}

-   If an issue is a duplicate, post \"duplicate of #ISSUENUM\", label as [Duplicate](https://github.com/minetest/minetest/labels/Duplicate), and close the issue. Newer issues should be considered duplicates of older issues, unless the newer issue has more useful conversation. Information from the duplicate issue can also be edited into the open issue.
-   If a pull request or an issue does not get a response from its author within one month when requiring more details, it is closed.
    -   Use [\"Action / change needed\"](https://github.com/minetest/minetest/labels/Action%20%2F%20change%20needed) and [\"User feedback needed\"](https://github.com/minetest/minetest/labels/User%20feedback%20needed)
-   [WIP](https://github.com/minetest/minetest/labels/WIP) / draft pull-requests that are not updated within 6 months should be closed.
-   Use [Project Boards](https://github.com/minetest/minetest/projects) to prioritise and order issues and pull requests.
-   The [Possible Close](https://github.com/minetest/minetest/labels/Possible%20Close) label can be used to warn authors of impending closure.

### Triagers

-   Triagers are members of the project that are not core developers, but have the ability to manage issues - see above.
    -   Examples include labelling issues, asking for necessary information, and managing boards to help with long-term planning.
-   They may close issues or PRs, but cannot approve them (the act of approving an issue implies there is a dev willing to review a contribution).
-   They should err on the side of caution - if they don\'t understand the issue, they should wait for feedback.
-   They should consider ways to improve project management further.

[Category:Rules and Guidelines](Category:Rules_and_Guidelines "wikilink") [Category:Git](Category:Git "wikilink")
