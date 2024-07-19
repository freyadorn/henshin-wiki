[Henshin](Home "wikilink")\'s **critical pair analysis** (CPA)
feature enables the detection of all potential conflicts and
dependencies of a set of
[rules](Transformation_Meta-Model#Basic_building_blocks:_Rules "wikilink").
The result of the analysis is a set of critical pairs, each reporting on
a potential conflict or dependency between two rules. A critical pair
describes a minimal conflict or dependency situation, consisting of the
first rule and the second rule as well as a minimal model affected by
the conflict or dependency.

As Henshin transformations are formally based on graph transformations,
the critical pair analysis is also based on algebraic graph
transformations.

<span style="background:#ffff00">As per release 1.6, this
feature has been superseded by Henshin\'s new [Conflict and Dependency
Analysis](Conflict_and_Dependency_Analysis "wikilink").</span>



## Kinds of conflicts and dependencies

Two kinds of relations among transformation rules can be detected:
conflicts and dependencies. In the case of a conflict, the application
of the first rule renders the second rule unapplicable on the produced
model. In the case of a dependency, the application of the first rule
enables the second rule to be applied on the produced model.

### Conflicts

A conflict generally describes a situation in which the two involved
rules are both applicable on a minimal model. Applying the first of
these rules on the minimal model leads to a changed result model. While
the second rule could have been applied on the model in the beginning,
it is no more applicable on the result model. In what follows, possible
kinds of conflicts are described.

-   **Delete-use-conflict:** The first rule deletes at least one element
    (an EClass or EReference) of the minimal model, such that the second
    rule is no more applicable.

<!-- -->

-   **Produce-forbid-conflict:** The first rule creates at least one
    element (EClass or EReference) which is forbidden by the second
    rule. The second rule becomes inapplicable on the resulting model,
    accordingly.

<!-- -->

-   **Change-use-conflict:** A change-use-conflict describes a similar
    situation to the delete-use-case, but with the focus on the values
    of EAttributes: the application of the first rule changes the value
    of an EAttribute. The second transformation is then no more
    applicable since it is only applicable when the old value is
    present.

<!-- -->

-   **Change-forbid-conflict:** Change-forbid-conflicts describe the
    situation where the application of the first rule changes the value
    of an EAttribute in such a way that it becomes equal to a forbidden
    value specified in the second rule.

### Dependencies

A dependency generally describes a situation in which the application of
the second transformation rule becomes enabled by applying the first
transformation rule. The minimal model is the result of the application
of the first rule and it allows the second rule to be applied. Various
situations can be encountered, which are described by the following
different kinds of dependencies.

-   **Produce-use-dependency:** A produce-use-dependency describes the
    situation where the application of the first rule creates a new
    element (EClass or EReference) which is required by the second rule.
    Based on the creation of the element the second rule becomes
    applicable. Note that it is irrelevant how the second rule deals
    with the created element, that is, if the element is preserved or
    deleted. The minimal model contains the created element which is
    required to apply the second rule.

<!-- -->

-   **Delete-forbid-dependency:** A delete-forbid-dependency describes
    the situation where the first rule deletes an element (EClass or
    EReference) which is forbidden by the second rule. The second rule
    becomes applicable since its negative application condition (NAC),
    which forbids the presence of the element, is now fulfilled. The
    minimal model is the instance after the application of the first
    rule, such that the second rule becomes applicable on it.

<!-- -->

-   **Change-use-dependency:** The change-use-dependency is similar to
    the produce-use-dependency with the focus on the value of an
    EAttribute. The dependency describes the situation where the second
    rule becomes applicable after the application of the first rule. The
    root cause is the change of the value of an EAttribute such that it
    fulfills the pattern of the second rule.

<!-- -->

-   **Change-forbid-dependency:** The change-forbid-dependency describes
    the situation where the first rule changes an EAttribute value that
    is forbidden by the second rule. With the new value, the second rule
    becomes applicable. The minimal model is the result after applying
    the first rule.

## Features

Several language features of the Henshin [model transformation
language](Transformation_Meta-Model "wikilink") and the [Eclipse
Modeling Framework](Eclipse_Modeling_Framework "wikilink") are
supported. The supported and unsupported features are listed in what
follows.

### Supported Features

-   Basic model transformations involving the actions *preserve*,
    *create* and *delete* of EClasses and EReferences as well as the
    change of the values of EAttributes
-   Positive application conditions (PAC) and negative application
    conditions (NAC), limited to the usual situation of AND
    concatination. In consequence this means application conditions
    containing OR and XOR ModelElements are not supported.
-   Ecore modeling concepts of inheritance and abstract eClasses and
    eOpposite references.

### Unsupported Features

-   Model transformations for more than one meta-model, for example
    outplace model transformations involving more than one Ecore
    instance. Affected rules are identified by more than one import in
    their containing *module*.
-   Multi-rules (formally, rules with amalgamtion).
-   Transformation units.

## Applying the Analysis

The analysis can be invoked either using a wizard or programmatically
using an API.

### CPA Wizard

[[/images/CPA_wizard_1.png]]

The wizard
enables the quick and easy usage of the critical pair analysis, notably
if the set of rules is currently being developed.

To invoke the wizard, please switch to the Java perspective in Eclipse.
Select one or more \*.henshin files in the Package Explorer, right-click
and choose *Henshin→Calculate Critical Pairs*. The selection of multiple
files is supported, but the user needs to ensure that all included rules
are specified for the same meta-model.

To execute the analysis, the rules have to be selected and the user
needs to decide if conflicts, dependencies, or both kinds of
relationships are to be considered. The required time for the analysis
depends on the number of rules and the size of each rule. **A large
number and/or size of selected rules can lead to considerable execution
times!**

After selecting at least one rule and at least one kind of relationship,
the analysis can be started using the *Finish* button. Alternatively,
the *Next* button leads to the second page *Option Setting* for advanced
configuration.

[[/images/CPA_results_view.png]]

After the analysis finished, all critical pairs as produced by the
user-specified configuration are listed in the *CPA Result View*. The
critical pairs are grouped by the rule combinations of which they
consist. This means that all analysed conflicts and dependencies of a
combination of two rules are listed together. To view them individually,
the entry for the rule combination can be expanded. Each single critical
pair can be further inspected. Double-clicking or pressing the enter
button opens the critical pair in the *Critical Pair Editor*. Each
critical pair consists of the first rule on the left side, the second
rule on the right side, and the minimal model in between. Additionally,
the results of the analysis are saved in the workspace within a
directory tree. To open a critical pair from the workspace, right-click
on a \*.henshinCp file in the Package Explorer and select *Henshin→Open
Critical Pair*.

### CPA API

The CPA API allows to use the CPA programmatically. As a prerequisite,
all considered transformation rules have to be loaded upfront. A
description of this process is given in the [Henshin Interpreter
page](Interpreter#Loading_.26_Saving "wikilink").

Depending on the desired result, either one or two lists of the rules to
be analysed have to be set up. To obtain all pair-wise conflicts or
dependencies within a set of rules, a single list is sufficient. If only
the dependencies or conflicts for a special set of rule pairs is needed,
a list of the first rules (*r1*) and a list of the second rules (*r2*)
is required as input. Even though the same output could be produced by
analysing all rules and filtering the result afterwards, consider that
the superfluous analysis might be very time consuming.

Another required initialisation parameter is a *CPAOptions* object. This
object is easily obtained using its default constructor and can be
adapted using its Setter and Getter methods.

The user mainly interacts with the CPA feature using the *CpaByAGG*
class, which is instantiated using its default constructor. Providing
the aforementioned list(s) of rules and CPAOptions object as input, the
analysis can be initialized with the suitable *init(...)*-method. Please
note that the *init(...)*-method might throw an
*UnsupportedRuleException* if one of the rules exhibits an [unsupported
feature](#Unsupported_Features "wikilink"). The method
*check(List`<Rule> rules)* can be used to avoid this exception
by checking the rules upfront.

After successful initialisation, the methods *runConflictAnalysis()* or
*runDependencyAnalysis()* can be used to start the analysis. Both
methods optionally take a *ProgressMonitor* object as input that can
monitor the analysis progress and give a rough estimate for the required
time to finish the analysis.

## Usage example

For a usage example of this feature, see the [Simple Class Modeling
Refactoring](https://www.eclipse.org/henshin/examples.php?example=simpleclassmodelingrefactoring).



