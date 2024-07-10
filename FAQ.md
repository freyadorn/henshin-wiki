Here we answer some **frequently asked questions** about
[Henshin](Home "wikilink").



## How can I execute transformations?

Use the [Henshin Interpreter](Interpreter "wikilink") to execute
transformations, available via a wizard or programatically.

## My rule cannot be applied, even though I expect that it should.

The two most common problems are as follows:

**Dangling edges:** This is the most common problem if some of your
rules work, but some rules, in particular those that delete something,
do not. The problem is that a rule can become inapplicable if the
deletion of a particular model element would leave behind *dangling
edges*, that is, references pointing to a non-existing model element.
This is because, per default, Henshin follows the Double-Pushout
Approach to graph transformations, which requires that all deletions,
including deletions of edges, are specified explicitly in the rule. To
ensure validity of the transformation, the interpreter performs a check
that the rule application does not leave behind dangling edges.\
To make the rule applicable, you can either modify the rule to specify
the problematic edges to be deleted as well (potentially using
[multi-rules](Transformation_Meta-Model#Advanced_concepts:_Application_conditions_and_rule_nesting "wikilink"),
to establish a for-all semantics), or switch to Single-Pushout semantics
(by setting the *checkDangling* property of the rule to *false*).

**Metamodel schizophrenia:** This problem is the most common if you
cannot get any of your rules to work. The term *metamodel schizophrenia*
refers to situations where the same metamodel is referenced differently
from the Henshin module than from the input model. For instance, the
Henshin module might refer to the metamodel dynamically, via a file path
in the workspace, whereas the input model refers to it statically, via
the package URI for the EMF-generated code. Or, you might have two
different copies of the meta-model stored in your workspace, and both
are referenced from your Henshin module and input model. If metamodel
schizophrenia applies to your case, the metamodel will be loaded twice
and considered as different entities, which makes it natural behavior
that the rule cannot be applied.\
To solve this issue, you need to fix either your Henshin module or your
input model, so that they agree on the way in which they reference the
used metamodel. See also: [How can I use dynamic EMF with
Henshin?](#How_can_I_use_dynamic_EMF_with_Henshin.3F "wikilink").

If you have ruled out these issues, we recommend to check out the
[Debugging
Support](Interpreter#Debugging_using_the_interpreter "wikilink").

## Does Henshin support automatic tracing for exogenous transformations?

Henshin follows a rewrite approach. Tracing is not build into the
transformation language but can be easily realized using a generic
[Henshin Trace Model](Trace_Model "wikilink").

## How can I use dynamic EMF with Henshin?

Define your EMF models as usual. Make sure you set the namespace URIs
and prefixes for all packages. To use the packages in Henshin,
right-click in the Henshin editor and select *Import Package \-- From
Workspace* and choose the packages from your Ecore files. To create an
dynamic instance model, right-click on an Ecore file and select *Henshin
\-- Create Dynamic Instance*. This will create an XMI file which you can
then edit in the Sample Reflective Model Editor of EMF.

If you **don\'t** want to use dynamic EMF, import the packages from the
registry and use the runtime version (not the development version).

## I get NullPointerExceptions when I try to execute my transformation

Make sure all node, edge and attribute types are correctly set. Also, it
is important that you reference the same version of the metamodels in
your Henshin files as in your instance models. For example, if you have
a UML instance model created with the UML editors, you must make sure
that you use the same UML metamodel in your transformation. In this
case, you would have to import the UML metamodel from the registry and
use the runtime version. See also the entries before and after this one.

## I get exceptions during state space generation

Try setting the property *collectMissingRoots=true*.

## How can I define a higher-order (HO) transformation in Henshin?

Import the Henshin and the Ecore metamodel into your transformation
(import from the registry and use the **runtime** versions). Check out
the examples repository on our website for more details.

## Why is my rule with a String constant not matched even though the constant is correct?

The attribute in the Henshin rule must look like this: \"CONSTANT\"
where CONSTANT is the string to be matched. Make sure you use double
quotes here. In the instance model, the object\'s attribute must have
the value CONSTANT \-- in the EMF editor you must not use any quotes
here!

## What sort of automatic analysis is supported?

You can use the [Henshin State Space
Explorer](State_Space_Tools "wikilink") to generate a state
space for a transformation system, check structural invariants and do
model checking.

The [Critical Pair Analysis](Critical_Pair_Analysis "wikilink")
allows to uncover conflicts and dependencies in a rule set, which can be
used for many purposes, including validation and testing.

[Category:FAQ](Category:FAQ "wikilink")


