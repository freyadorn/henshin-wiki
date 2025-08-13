The **Henshin Compact API** extends Henshin's default API with simplified ways to specify and execute modules, units, and rules. It provides an alternative to the EMF-generated code for the [meta-model](Transformation_Meta-Model "wikilink"), while being compatible to it.

The standard way of creating and maintaining Henshin modules is to use one of the three editors ([graphical editor](GraphicalEditor "wikilink"), [textual editor](Textual-Editor "wikilink"), tree-based editor). However, there are two main use cases where using an
API is preferable:

-   situations where modules, rules, and units need to be created and
    updated programmatically (for example, as part of a larger automated
    tool chain)
-   developers who strictly prefer working with GPL code editors to
    working with model editors (even if this means, in this case, to do
    without automatic validation as provided by the Henshin editors -
    proceed with caution!)

We consider as running example the existing [Bank Example](Getting_started "wikilink") and [University Courses Example](Examples/University_Courses "wikilink").

## Specifying a simple Henshin Module

For this part of the tutorial we will use the transformation rule
*createAccount* from the Bank Example. We will specify this
transformation rule step by step using the package
*org.eclipse.emf.henshin.model.compact*.

[[/images/Henshin_compact_rule-create-account.png]]

**CModule:** the first thing we need to create is a CModule (relevant
class: *org.eclipse.emf.henshin.model.compact.CModule*). A CModule
represents a Henshin module and is the container of all transformation
rules and units we will specify. To create a CModule you can call the
constructor, specifying a name for the new module.

[[/images/Henshin_compact_specifyNewCModule.png]]

If you already got an existing Henshin module, you can use the static
method *loadFromFile* to create a CModule from this .henshin file. Next
we only need to add our used ecore package(s) to the imports of our
module. To do this we can either add a single package, given as EPackage
object or add all ecore packages contained in an .ecore-File.

[[/images/Henshin_compact_addImportsFromFile.png]]

**CRule:** Once we got a CModule we can then specify transformation
rules by use of the *createRule* method. Given a name for the new rule
this method will return a newly created CRule (relevant class:
*org.eclipse.emf.henshin.model.compact.CRule*).

[[/images/Henshin_compact_createRule.png]]
**Parameters:** Our new created transformation rule now needs two
parameters. To create this parameters, we need to call the
*createParameter* method twice. This method takes the kind, the name and
the type of parameter we want to specify. You can specify these by using
their String representation.

[[/images/Henshin_compact_createParameter.png]]

Note: The literals EInt, EDouble, EString... can not be derived from a
String.

Until now we specified a transformation rule with two parameters.

[[/images/Henshin_compact_ruleWithoutNodes.png]]

**Nodes:** Now that we have a transformation rule, we want to fill this
rule with nodes. To create a node inside a transformation rule, we call
the *createNode* method. Again the class CRule can derive the type of
node from its String representation.

[[/images/Henshin_compact_createNode.png]]
The default action when specifying a node is the `<<preserve>>`
action. To specify another action when creating a node, we just add the
action to the parameters of *createNode*.

[[/images/Henshin_compact_createNodeWithAction.png]]

As you can see, actions can be parsed from a String, too.

**Attributes:** To add an attribute to our freshly created nodes, we
call the *createAttribute* method from the respective CNode. This method
takes type and value of the attribute. By default an attribute inherits
the Action from its container. If you want to change this, you can
simply add another Action or parseable String as parameter for
*createAttribute*.

[[/images/Henshin_compact_createAttribute.png]]

**Edges:** To create the edges of our rule, we use *createEdge*. Again
the type of the edge can be derived from its String representation.

[[/images/Henshin_compact_createEdge.png]]

## Additional Elements

We successfully recreated the transformation rule *createAccount* using
the *henshin.compact* package. To save our result into a .henshin
file, we call the *save* method in our CModule. This will save the
module either at the given file path, or at the current working
directory in a file that has the same name as the module itself.

After we created the transformation rule *createAccount*, we will now
see what other elements the other two transformation rules need and how
to specify them using *henshin.compact*

**AttributeConditions:** The transformation rule *transferMoney* uses
two attribute conditions. These are specified by the
*createAttributeCondition* method in CRule.

[[/images/Henshin_compact_createAttributeCondition.png]]

**Updating attributes:** For some attributes you want their value to
change when the transformation rule is applied. To realize this you can
use the operator `->` inside of an attributes value String. The value
to the left of the arrow will be the value before applying the rule. The
value to the right of the arrow will be the value after applying the
rule.

**Multi-rules:** A multi-rule is a transformation rule nested inside
another rule. The transformation specified by a multi-rule will be
applied to all matching objects instead of just one specific match. The
transformation rule *deleteAllAccounts* uses one multi-rule. To specify
a multi-rule, you simply specify its elements, using the `*` operator at
the end of the parseable action String. 

[[/images/Henshin_compact_MultiRule.png]]

You can also specify a Multi-Rule
path to create more complex constructs of different multi-rules. The
path of the rule an element is contained in is specified behind the
`*`-operator.

[[/images/Henshin_compact_MultiRulePath.png]]

**NestedConditions:** NestedConditions, by name NACs and PACs, are
contained in the LHS graph of a rule. `<<forbid>>` Nodes and Edges are
always part of NACs. `<<require>>` Nodes and Edges are always part of
PACs. You can name the conditions they are contained in, by adding the
#-Operator at the end of the parseable Action String. By default all
conditions inside the LHS graph are contained in a conjunction. However,
these conditions can be used to build bigger logical conditions. With
CRule you can extract these conditions by using the methods *getNAC* and
*getPAC*. Henshin provides logical operators to build up your own
logical conditions.

[[/images/Henshin_compact_advancedConditions.png]]

## Specifying Units

Now that we can fully specify transformation rules, we want to put Units
to use, to get the most out of our transformation rules. We will
consider the universitycourses example.

The CModule class provides a own method for every type of unit there is.
We will now construct some Units to see how similar unit types are
created.

**UnaryUnit:** As example for a UnaryUnit we use the LoopUnit
*cleanupUninterestingCoursesUnit*.

[[/images/Henshin_compact_universityCourses-cleanupUninterestingCoursesUnit.png]]

This unit is created by the *createLoop* method.

[[/images/Henshin_compact_loopUnit.png]]

**ConditionalUnit:** ConditionalUnits are a special case of unary Units.
To create a ConditionalUnit you need to specify a If- and Then-Subunit.
You can specify a Else-Subunit too but this is only optional.
ConditionalUnits are the only unary unit type that does not derive its
name by the name of its subunit. Therefore a name for the unit needs to
be specified too.

[[/images/Henshin_compact_ConditionalUnit.png]]

**MultiUnits:** Creating a multi unit uses the same method as adding
subunits to this multi unit uses. The method will add the specified Unit
to the subunits of the multi unit. The multi unit is given by its name.
If no unit with the given name exists CModule will instead create the
unit. For example, if we want to create *incrementHour* we can use the
following code:

[[/images/Henshin_compact_multiUnit.png]]

## Mapping Parameters

Some units have Parameters that need to get mapped between the unit and
its subunits. To do this you call the method *mapParameterToSubunit*
from the container CUnit. This method takes three names. The name of the
parameter to be mapped, the name of the unit to be the target and
finally the name of the parameter inside the target unit. For example,
the mappings inside of *incrementHour* are created like this:

[[/images/Henshin_compact_paramMapping.png]]

## Executing Units programmatically

Before we have learned how to specify transformation rules and unit
using *henshin.compact*. Now that we saved our specified Module into a
.henshin file, we can apply our rules. We will again use the Bank
example to show how to use the Interpreter and Debugger (relevant
classes: (relevant classes:
*org.eclipse.emf.henshin.interpreter.impl.Interpreter* and
*org.eclipse.emf.henshin.interpreter.impl.Debugger*).

Before we can start applying our units, we need to load our module and
graph. To load the resources we need a HenshinResourceSet and then load
the graph and the module with the given methods.

[[/images/Henshin_compact_loadResources.png]]

Once we got our resources we can then create a new Interpreter. The
Interpreter is provided a file path, specifying the location where the
transformation result will be saved.

After we constructed our Interpreter we can start executing our Units.
Using the method *executeUnit* we only need to specify the Graph and
Module instances to use. Additionally we need to specify the name of the
unit we want to be applied, as well as a value for every parameter of
this unit.

[[/images/Henshin_compact_executeUnit.png]]

Note: The list of parameter values **must** have the same order as the
parameters inside of the unit, as well as the same typing

After applying the transformation the transformed graph gets returned.
To save this graph back to a file, we simply call the *saveGraph* method
in our Interpreter.

[[/images/Henshin_compact_saveGraph.png]]

## Interpreter vs Debugger

If you only want to simply execute your transformation units, the
Interpreter class will do everything you need. For more advanced
operations like applying Rules on certain matches, you will need to use
the Debugger class. The Debugger class
(*org.eclipse.emf.henshin.interpreter.impl.Debugger*) provides methods
for undoing and redoing units, as well as choosing matches for your
transformation rules. Once you created your desired match, you call the
method *executeRuleOnMatch*, giving it the match to execute on.

**Limitation:** support for undo and redo within these classes has yet
to be implemented.

## Credits

The Compact API was provided by Johannes Ludwig as part of his bachelor's thesis.
