This page provides two important types of **Instructions to Henshin
committers**: The set-up for working on the codebase with Gerrit, and
the instructions for importing projects and resolving their dependencies
in your local workspace.

## Importing projects into your workspace and resolving dependencies 

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
