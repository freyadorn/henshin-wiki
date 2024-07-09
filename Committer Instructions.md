This page provides two important types of **Instructions to Henshin
committers**: The set-up for working on the codebase with Gerrit, and
the instructions for importing projects and resolving their dependencies
in your local workspace.

## Set-up {#set_up}

Henshin uses [Gerrit](https://git.eclipse.org/r/#/) for handling code
reviewing and merging. This implies specific processes when setting up
the development environment and pushing changes. See also the official
primer for [Contributing to Eclipse Projects via
Gerrit](https://www.eclipse.org/community/eclipse_newsletter/2014/july/article3.php).

The following steps are for setting up the development environment based
on eGit, Eclipse\'s git integration. Steps may vary if you don\'t want
to use eGit.

1.  To be able to push commits to Gerrit, please register an Eclipse
    account and log into
    [Gerrit](https://git.eclipse.org/r/#/dashboard/self).
2.  If you don\'t have one yet, generate a SSH RSA key pair: do
    *ssh-keygen -t rsa* in the git shell.
3.  Register your public key at
    <https://git.eclipse.org/r/#/settings/ssh-keys>
4.  Make sure your local git\'s committer e-mail address matches the one
    registered at <https://git.eclipse.org/r/#/settings/contact>
    -   To view the local e-mail address, do *git config user.email*. An
        empty result means the e-mail address has not been set yet.
    -   To change the local e-mail address, do *git config \--global
        user.email \[your email address here\]*
    -   In the same manner, you can view and change your committer name
        using the *user.name* environment variable.
5.  To ensure correct handling of line endings when using Windows,
    please use the following git configs: *core.autocrlf=true* and
    *core.filemode=false*. If you have dirty projects due to Windows
    line endings, try a hard reset after setting the config.
6.  In Eclipse, clone the Henshin repository using one of the URLs (git,
    http, ssh) given at the [repository
    website](https://git.eclipse.org/c/henshin/org.eclipse.emft.henshin.git).
    We recommend the following workflow. If relevant, see below for a
    frequent error related to push rights.
    -   For cloning the initial state of the repo, use the (read-only)
        [GIT repository
        URL](git://git.eclipse.org/gitroot/henshin/org.eclipse.emft.henshin.git).
        Cloning the entire Repo via the SSH URL may lead to timeouts.
    -   After cloning, change the repository location to the [SSH
        repository
        URL](ssh://user_id@git.eclipse.org:29418/henshin/org.eclipse.emft.henshin)
        (do *git remote set-url origin `<gerrit-url>`{=html}*, or follow
        the [eGit
        instructions](https://www.eclipse.org/community/eclipse_newsletter/2014/july/article3.php)).
        Don\'t forget to substitute *user_id* in the URL by your actual
        Gerrit username, which can be found
        [here](https://git.eclipse.org/r/#/settings/).
7.  In the Git perspective, open the Repository: *Remotes -\>* Right
    click on *origin -\> Gerrit Configuration\... -\> Finish*.
8.  Follow the steps outlined in the next section.

After pushing a change, a corresponding entry on
[Gerrit](https://git.eclipse.org/r/#/) has been created. Please invite
(a subset of) the other committers as reviewers, doing this for all
changes.

### Frequent problem: no push rights {#frequent_problem_no_push_rights}

A frequent error message is:

`Repository `[`https://username@git.eclipse.org/r/henshin/org.eclipse.emft.henshin`](https://username@git.eclipse.org/r/henshin/org.eclipse.emft.henshin)\
`prohibited by Gerrit: not permitted: update`\
`error: branch refs/heads/master:`\
`To push into this reference you need 'Push' rights.`\
`User: username`\
`Contact an administrator to fix the permissions."`

This error message occurs if you\'re pushing to one of the read-only
repository URLs. Please follow the instructions above, especially the
part about changing the repository location.

[category:Henshin](category:Henshin "wikilink")

## `<span id="imports">`{=html}`</span>`{=html}Importing projects into your workspace and resolving dependencies {#importing_projects_into_your_workspace_and_resolving_dependencies}

Henshin\'s codebase is divided into several plug-in projects with
different responsibilies, for example:

-   Plug-in *org.eclipse.emf.henshin.model*: The
    [meta-model](Henshin/Transformation_Meta-Model "wikilink")
-   Plug-in *org.eclipse.emf.henshin.diagram*: The [graphical
    editor](Henshin/Graphical_Editor "wikilink")
-   Plug-in *org.eclipse.emf.henshin.interpreter*: The
    [interpeter](Henshin/Interpreter "wikilink")
-   Plug-ins *org.eclipse.emf.henshin.{edit,editor}*: The tree-based
    editor

\
All plug-in projects are contained in the *plugins* folder in the root
directory of our Git repo. Import the plug-ins you want to work on **as
Eclipse projects** into your workspace. Depending on your selection, you
may encounter various build errors. To resolve them, install
dependencies as necessary. The provided links refer to *update sites*
and **cannot** be opened in the browser. Use them to install the
dependencies in Eclipse via *Help* -\> *Install New Software*.

-   [Papyrus for UML](http://download.eclipse.org/releases/oxygen/):
    needed for *org.eclipse.emf.henshin.diagram*. In the update site,
    find Papyrus in the *Modeling* category, or by using the filter.
    Papyrus provides some legacy components of GMF Tooling that are
    required by Henshin\'s GMF-based diagram editor.
-   [Maven Integration for
    Eclipse](http://download.eclipse.org/technology/m2e/releases):
    needed for *org.eclipse.emf.henshin.giraph*.
-   [Eclipse
    Xtext](http://download.eclipse.org/modeling/tmf/xtext/updates/composite/releases/):
    needed for *org.eclipse.emf.henshin.text* projects. On further build
    errors, trigger the Xtext generation process: navigate to
    *org.eclipse.emf.henshin.text -\> GenerateHenshin_text.mwe2*, right
    click and do *Run As -\> MWE2 Workflow*.
-   [QVT
    Operational](http://download.eclipse.org/mmt/qvto/updates/releases):
    needed for *org.eclipse.emf.henshin.text.transformation*. Navigate
    to the *Metamodel Mappings*: right click the project
    *org.eclipse.emf.henshin.text.transformation* -\> *Properties* -\>
    *Metamodel Mappings*. There add mappings for
    *org.eclipse.emf.henshin.model/model/henshin.ecore* and
    *org.eclipse.emf.henshin.text/model/generated/Henshin_text.ecore*:
    *Add* -\> *Browse* -\> navigate to .ecore-file.
-   [EMF
    Compare](http://download.eclipse.org/modeling/emf/compare/updates/releases):
    needed for *org.eclipse.emf.henshin.interpreter.ui*,
    *org.eclipse.emf.henshin.multicda.cpa* and
    *org.eclipse.emf.henshin.text.transformation.tests*.
-   [AGG
    Runtime](http://download.eclipse.org/tools/orbit/downloads/drops/R20160520211859/repository/):
    needed for *org.eclipse.emf.henshin.multicda.cpa*. During
    installation filter for *AGG Runtime* to find the package.

To resolve further build errors in
*org.eclipse.emf.henshin.adapters.xtext* right click the project -\>
*Properties* -\> *Resource* and set the text file encoding to *Other:
UTF-8*.

[Category:Modeling](Category:Modeling "wikilink")
