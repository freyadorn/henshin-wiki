**Henshin Xtext adapter** is a plugin that allows
[Henshin](Home "wikilink") modules, rules, nodes etc to be referenced
directly from any [Xtext](Xtext "wikilink")-based language.

### Intended use

When defining an Xtext language, you essentially write down an
(annotated) EBNF grammar for it and Xtext will generate a meta-model
etc. for you. One cool feature is that you can explicitly describe
cross-references. The way to do this is by replacing a direct reference
to a non-terminal (say A) with the name of the non-terminal in angular
brackets, possibly followed by a token/data-type to be used to identify
the relevant object. So, for example we could say:

`A : /* … description of what A's look like */ ; `\
\
`B: 'anA:' myA = [A|ID] ;`

This allows us to write

` anA MYA`

asking Xtext to look up and find an instance of A called MYA. From the
grammar above, Xtext generates a meta-class B with a non-containment
reference myA pointing at an A instance. All the name resolution is done
as part of parsing the textual model.

The beautiful thing about this is that it also works across models and
languages. So, after importing the Henshin metamodel in your Xtext
grammar, you can say things like

` GTS: 'gts' 'metamodel' ':' metamodel = [EPackage|QualifiedName] 'henshin' ':' module=[Module|ID] ;`

And get Xtext to do all the resolving for you. By default, any Henshin
module that is visible on the classpath of my current project will be
available (that's configurable) and code completion can help you find
them.

However, for all of this to work, Xtext needs some support. In
particular, it needs ways of (a) finding Henshin files, and (b) giving
names to elements within Henshin files. That\'s what this plugin
provides: everything needed for using Henshin modules while parsing and
manipulating Xtext files, and configuration so that the name provider
can be correctly used in the context of an Xtext-generated editor.

### Usage instructions

-   Add
    `<span style="background:#dddddd">`{=html}org.eclipse.emf.henshin.adapters.xtext`</span>`{=html}
    and
    `<span style="background:#dddddd">`{=html}org.eclipse.emf.henshin.model`</span>`{=html}
    to the dependencies of your Xtext language plugin (and potentially
    your test plugin).
-   Add
    `<span style="background:#dddddd">`{=html}org.eclipse.emf.henshin.adapters.xtext`</span>`{=html}
    to the dependencies of your language\'s .ui plugin.
-   Add `<span style="background:#dddddd">`{=html}referencedResource =
    \"<platform:/resource/org.eclipse.emf.henshin.model/model/henshin.genmodel>\"`</span>`{=html}
    to the *StandardLanguage* block in your .mwe2 file.
-   Add `<span style="background:#dddddd">`{=html}import
    \"http://www.eclipse.org/emf/2011/Henshin\" as
    henshin`</span>`{=html} to your .xtext file.
-   Use
    `<span style="background:#dddddd">`{=html}\[henshin::Module\]`</span>`{=html},
    `<span style="background:#dddddd">`{=html}\[henshin::Rule\]`</span>`{=html}
    etc. in your grammar to reference [Henshin
    meta-classes](Transformation_Meta-Model "wikilink").

By default, links have no name in Henshin. To allow them to be
referenced nonetheless, the plugins generate names following the grammar
rule below:

`LinkName :`\
`   "[" ID "->" ID ":" ID "]"`\
`;`

The first ID is the name of the source node, the second the name of the
target node, while the third is the name of the *EReference*. This
naming scheme can be imported into any class in your own code by
statically importing
*org.eclipse.emf.henshin.adapters.xtext.NamingHelper*. In an Xtend
class, it is recommended to import the class as an extension.

### Credits and links

-   Contributed by Steffen Zschaler. See the original [implementation on
    GitHub](https://github.com/szschaler/henshin_xtext_adapter)
-   [Xtext website](https://www.eclipse.org/Xtext/)


