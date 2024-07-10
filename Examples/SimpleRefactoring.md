
The **Simple Class Modeling Refactoring example** illustrates Henshin\'s
capabilities for [Conflict and Dependency
Analysis](Conflict_and_Dependency_Analysis "wikilink"). It is
described in the paper *Analyzing Conflicts and Dependencies of
Rule-Based Transformations in Henshin* published at FASE 2015. We
explain how to use the analysis (wizard and programmatically) to find
all potential conflicts and dependencies in a set of rules.

The transformation model and source code can be found
[here](https://git.eclipse.org/c/henshin/org.eclipse.emft.henshin.git/tree/plugins/org.eclipse.emf.henshin.examples/src/org/eclipse/emf/henshin/examples/simpleclassmodelingrefactoring).

### Metamodel

[[/images/henshin-ref-classModel.png]]

The metamodel is defined in the file *ClassModel.ecore*. We used the
Ecore Tools to draw a diagram (depicted above). The diagram is stored in
the file *ClassModel.ecore_diagram*.

The EMF Ecore specification of a simple class modeling language can be
used to model the main static aspects of a given domain. Such a *Model*
consists of a number of *Classes* where each one can have a number of
*Attributes*. Finally, a class may be associated to other classes by
*references*.

### Refactoring Transformation Rules

[[/images/henshin-ref-refactoringRules.png]]

Since refactoring is a specific kind of model transformation,
refactorings for EMF-based models can be specified in Henshin and then
integrated into a model refactoring framework such as [EMF
Refactor](http://www.eclipse.org/emf-refactor/). Rule *Add Attribute*
inserts a new attribute into an exiting class whose name is given by
parameter clName. Rule *Move Attribute* shifts an attribute from its
owning class to an associated one along the corresponding reference.
Finally, rule *Remove Empty Class* deletes an existing class if it has
no relation to further model elements, i.e., the class has neither
attributes or references nor it is referenced by another class.

### Scenario

[[/images/henshin-ref-scenario.png]]

Let\'s assume that a software developer wants to know which refactoring
rules need to be applied in order to restructure a class model. More
specifically, we refer to the class model shown in the following figure
representing an early analysis model in the development of an address
book system.

Typically, many different refactoring rules may be applicable, and it is
not easy to find out the best way to apply these rules. On the one hand,
simultaneous applications of specific refactoring rules may not be
possible due to parallel dependencies between them, and on the other
hand, some refactoring rules may sequentially depend on other ones.
Considering the refactorings and the class model presented above, we
observe the following:

-   Class *Account* is not used so far and makes no sense. Two
    refactorings are applicable here. Either the class is extended by a
    new attribute, e.g. accountnumber, or it is removed from the model
    (refactorings *Add Attribute* respectively *Remove Empty Class*).
    However, if one refactoring is executed the other becomes
    inapplicable, i.e., these refactorings are in conflict.

<!-- -->

-   Attribute *landlineNo* of class *Person* should be shifted to either
    class *Home* or class *Office* (both by using refactoring \'\'Move
    Attribute). However, if it is shifted to class Home the other
    refactoring becomes inapplicable (and vice versa). This means,
    refactoring Move Attribute is in conflict with itself.

### Applying the Critical Pair Analysis

`<span style="background:#ffff00">`{=html}As per release 1.6, the
Critical Pair Analysis has been superseded by Henshin\'s new [Conflict
and Dependency
Analysis](Conflict_and_Dependency_Analysis "wikilink") - see the
illustrations there.`</span>`{=html}

The CPA in Henshin can be used in two different ways: Its application
programming interface (API) can be used to integrate the CPA into other
tools of different application domains. In addition, a user interface
(UI) is provided supporting domain experts in developing rules by
providing the CPA interactively.

#### Execution using the CPA Wizard

`<span style="background:#ffff00">`{=html}As per release 1.6, the
Critical Pair Analysis has been superseded by Henshin\'s new [Conflict
and Dependency
Analysis](Conflict_and_Dependency_Analysis "wikilink") - see the
illustrations there.`</span>`{=html}

The CPA is called on a Henshin file in the Eclipse Package Explorer
(Right click → Henshin → Calculate Critical Pairs).

[[/images/henshin-ref-callCPAWizard.png]]

This brings up a wizard to specify the rule set to be analyzed and the
kind of critical pairs that shall be analyzed.

[[/images/henshin-ref-CPAWizard.png]]

After the CPA, a dedicated results view provides an overview on all
conflicts and dependencies found. The resulting list of critical pairs
is ordered along conflicts and dependencies as well as rule pairs. For
each pair, a set of conflicts (dependencies) is listed named by their
kinds.

[[/images/henshin-ref-CPAResultView.png]]

Here, three conflics are found. The conflicts of the first and the third
rule pair in the results view present a *produce-forbid-conflict* and a
*delete-use-conflict*, as informally described in item (1) at the
scenario description above. Item (2) of the running example describes
the *delete-use-conflict* for rule pair (*Move Attribute*, *Move
Attribute*) which is also listed in the CPA results view.

For each critical pair, a minimal EMF model can be viewed after
double-clicking on a specific conflict (dependency). The following
figure shows a minimal model in the tree-based Ecore instance editor.

[[/images/henshin-ref-CPAEditor.png]]

The minimal model (see center of the figure above) is related to the
first *produce-forbid-conflict* of the example. Both rules (shown on the
left- and the right-hand-side of the figure above; here using the
tree-based Henshin editor) overlap in model elements of types *Class*
and *Attribute*. Element *Attribute* is marked by hash tags to state
that it causes the conflict. Numbers indicate corresponding matches of
rule nodes to model nodes.

#### Execution using the API

`<span style="background:#ffff00">`{=html}As per release 1.6, the
Critical Pair Analysis has been superseded by Henshin\'s new [Conflict
and Dependency
Analysis](Conflict_and_Dependency_Analysis "wikilink") - see the
illustrations there.`</span>`{=html}

The usage of the API is straightforward. There are two main steps to
obtain a result: (1) Initialize the meta-model and the rules, and (2)
start the execution of the conflict (dependency) analysis. The following
figure shows the corresponding API.

[[/images/henshin-ref-ICriticalPairAnalysis.png]]

The initialization can be done with one or two rule sets containing
Henshin transformation rules. Providing one rule set using method *init(
List rules, CPAOptions options)* leads to the analysis of all
combinations between them leading to a quadratic number of pairs.
Passing two sets of rules using method *init( List r1, List r2,
CPAOptions options)* is useful to define sequences of rules which means
that only conflicts (dependencies) with rules of set *r1* as first rule
an rules of set *r2* as second rule are analyzed. Another parameter is
the *CPAOptions*. Generally it can be used with its default values. In
detail, this provides options to stop the calculation after finding a
first conflict (dependency) \[isComplete\], to ignore critical pairs of
same rules \[ignore\], and to reduce the result by critical pairs which
are based on the same match \[reduceSameMatch\]. After rule
initialization, an optional check can be performed (method *check()*).
This proves for instance whether the passed rules are based on a common
meta model. The calculation of the conflicts (dependencies) starts by
the corresponding methods *runConflictAnalysis()* and
*runDependencyAnalysis()*. Since the calculation can be time-consuming
the interface provides additionally the methods
*runConflictAnalysis(IProgressMonitor monitor)* and
*runDependencyAnalysis(IProgressMonitor monitor)* to get feedback by the
monitor.

The following figure shows the results API.

[[/images/henshin-ref-CPAResultAPI.png]]

All conflicts (dependencies) are within the container *CPAResult*.
Conflict and Dependency are concrete instances of the general concept
*CriticalPair*. A critical pair always contains two rules and a minimal
model showing the conflict or dependency. Two mappings are required to
map rule elements to corresponding ones in the minimal model. Although
*Dependency* and *Conflict* share this requirement, it is not part of a
critical pair in general, since different mappings are reported. A
conflict is based on the left-hand sides (LHSs) of both rules, such that
both mappings are *matches*. A dependency is based on the right-hand
side (RHS) of the first rule *r1* and the LHS of the second rule *r2*.
Therefore, the first mapping is a *comatch* of RHS to the minimal model.
Finally, there are different kinds of conflicts and dependencies as
introduced above. Two enumerations list these kinds (see following
figure). Note that dependency and conflict kinds are strictly separated.

[[/images/henshin-ref-CPKinds.png]]

*contributed by Thorsten Arendt and Kristopher Born*


