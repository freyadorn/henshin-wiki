Henshin\'s **variant management** feature allows to express variants of
the same rule in a compact way, by making the commonalities and
differences (*variabilities*) between the variants explicit. The key
idea is to annotate rule elements (such as nodes and edges) with
presence conditions over a set of *features* in which the variants
differ. A presence condition specifies a condition under which an
element is present.

Presence conditions are propositional formulas, based on the connectives
`& | !` (e.g., `(A & B) | (C & !D)`). We also support the `xor`
connective in a prefix notation (e.g, `xor(A,B,C)`). Since it\'s
cumbersome to edit presence conditions by hand, we provide dedicated
[editing support](#integration-with-graphical-editor "wikilink") to reduce that effort.

We also provide an extension of the
[interpreter](Interpreter "wikilink") that can deal with such
annotated rules, either by removing the variability before one variant
is applied, or by considering all variants at the same time (executing
them in *batch mode*).

### Example

*This example is available as an Eclipse project with rules, an example
model, and execution code:
[henshin-refactoring-variants-example](https://github.com/eclipse-henshin/henshin/wiki/files/henshin-refactoring-variants-example.zip)*

Consider a family of three refactoring rules for refactoring a class
diagram. The rules, shown below, express variants of the *move method*
refactoring for class models. The first rule specifies the relocation of
a method between two classes. The second one additionally creates a
\"wrapper\" method of the same name in the source class. The third one
adds an annotation to mark the wrapper method as deprecated.

[[/images/Henshin_vb_exampleClassic.png]]

These three rule variants can be expressed using one rule with
*variability*, shown below. Several elements are annotated with presence
conditions over the features *wrapper* and *deprecate*. The variants are
obtained by configuring the rule, i.e., binding the features to *true*
or *false* and removing elements whose presence condition evaluates to
*false*. Configuration *{wrapper=false; deprecate=false}* yields the
base rule, a rule isomorphic to rule *moveMethod* in the Figure above.
The rules induced by the configurations *{wrapper=true;
deprecate=false}* and *{wrapper=true; deprecate=true}* produce the
additional variants. To avoid the illegal configuration *{wrapper=false;
deprecate=true}*, the rule has a *configuration constraint*, shown in
the title bar, requiring *wrapper* to be true if *deprecate* is true.

[[/images/Henshin_vb_exampleCompact.png]]

<div id="editorsupport">
</div>

### Integration with graphical editor

A user interface supporting the management of rules with variability is
integrated with Henshin\'s [graphical
editor](GraphicalEditor "wikilink"). The main components of
this user interface, a graphical editor and its attached *properties*
view, are shown in the left of the image below. As custom components, we
provide the *variability* and *sanitizing* views, shown in the right.
The variability view comprises features for the definition and
configuration of variability points. The variant produced from the
current configuration is displayed in the editor. The sanitizing view
can be used to sanitize legacy transformations. Both views are opened
from the *Windows -\> Show View -\> Other\...* menu in Eclipse.



[[/images/Henshin_vb_perspective.png]]


The variability view allows features in a rule to be created and
deleted. To view and edit variants individually, the user configures the
rule by setting the bindings for these features. Three literals are
supported: *true*, *false*, and *unbound*. Per default, each variability
point is *unbound*, yielding the maximal rule, all elements regardless
of their annotations. Configurations are validated against the given
variability model, *deprecate -\> wrapper* in this case. Invalid
configurations and rules lead to error messages being displayed.

To navigate variants efficiently, frequently used configurations can be
saved as favorites using the 
[[/images/star_grey.png]] button in the toolbar.
The star appears in yellow if a favored configuration is currently
active. Each configuration has a user-specified name. In the figure to
the right the user has created two favorites, *WrapperWithDeprecate* and
*WrapperWithoutDeprecate*, the latter one being active. Upon selection,
the configuration is loaded and shown in the table at the bottom of the
view.

A *view mode* feature allows to access distinguished variants rapidly.
In the *maximum rule* mode, represented by the
[[/images/rule_full.png]] icon, all elements
included in the rule are shown regardless of the configuration. In the
*variant mode*
([[/images/rule_configured.png]]), elements
absent in the current configuration are concealed. In the *base rule*
mode ([[/images/rule_base.png]]), elements with a
non-empty presence condition are concealed.

To further improve the handling of variability, the view allows the
users to choose a *concealing strategy*, depicted in the figure below.
First, elements can be turned invisible. This avoids a cluttered
representation of the rule and lets users focus on the variant at hand.
On the other hand, to allow the comparison of a variant with the full
rule, users may choose to have the elements toned down instead.

[[/images/Vbrule_fade.png]]

*Concealing strategy: tone down*


[[/images/Vbrule_invisible.png]]

*Concealing strategy: invisible*


Using the [[/images/Creation_mode.png]] button,
users can select an *editing mode* to define which variants are affected
by edits to the rule. The supported options are: all variants, variants
included in the selected configuration, or variants associated to the
current view mode. In particular, the editing mode determines which
presence condition is assigned during the addition or deletion of
elements to a rule.

**Context menus**: Additional context menu entries allow to manage
variability at the level of individual elements. Multiple nodes, edges
and attributes can be selected and simultaneously \"moved to a different
configuration\" (that is, have their presence conditions changed to
match the current configuration).

### Execution via API

Applying a rule with variability is supported via an extension of the
interpreter engine. A minimal self-contained usage example is available
from the [example
project](https://github.com/eclipse-henshin/henshin/wiki/files/henshin-refactoring-variants-example.zip).
Loading the relevant resources and setting up the components of the
transformation (EGraph, Engine etc.) etc. works the same as for standard
rules (see the [Interpreter API
description](Interpreter#interpreter-api "wikilink")).

Instead of using a standard *RuleApplicationImpl*, rules with
variability are applied by using a class *VarRuleApplicationImpl*, which
(optionally) takes as input a configuration, represented by a map of
feature names:

``` java
   System.out.println("### Applying rule variant with wrapper and deprecated ###");
   Map<String, Boolean> configuration = new HashMap<String, Boolean>() {{
      put("wrapper", true);
      put("deprecated", true);
   }};
   RuleApplication vbRuleApp = new VarRuleApplicationImpl(engine, egraph, rule, configuration, null);
   vbRuleApp.execute(null);
```

\
In this snippet, the user configures the rule to activate the *wrapper*
and *deprecated* features, which produces a rule variant identical to
*moveMethodAddDeprecatedWrapper* in the first figure. This rule variant
is applied to the input model.

It\'s allowed to provide a *partial* configuration, which does not
include a value for each feature, and to provide *null* as the input
configuration. In this case, if multiple variants are applicable, an
applicable one will be non-deterministically applied. This behavior is
particularly useful for batch transformations, where all rules of a
module are applied as long as one is applicable (see [Strüber et al.\'s
paper at FASE 2015](https://doi.org/10.1007/978-3-662-46675-9_19)).

### Technicalities/To-Dos

-   To support variant-specific differences of the *injectiveMatching*
    flag, rules with variability have a separate attribute
    *injectiveMatchingPresenceCondition*, which, if set, overrides
    *injectiveMatching*
-   All variant management information (presence conditions, feature
    lists, configuration constraint, injective matching presence
    condition) is represented using the
    [Annotation](Transformation-Meta-Model#annotations "wikilink")
    metaclass. The UI shows these annotations as entries in the
    *Properties* view.
-   Elements of the same application condition graphs cannot have
    different presence conditions \-- a NAC or PAC as a whole is either
    present or absent in a variant.
-   Interaction with multi-rules is currently undefined.
-   Integration with the transformation wizard is an open to-do.


