**Parameters** allow to shape the behavior of
[units](Units "wikilink"), including rules, with variable
information. A unit can have an arbitary number of parameters.
Parameters have a name, a description, a kind, and, optionally, a type.

## Parameter kinds

The kind specifies the time when the parameter is bound to a concrete
value, and whether the parameter is intended to be accessed after the
unit has been applied. There are four parameter kinds (*in*, *out*,
*inout*, *var*) and an additional legacy parameter kind (*unknown*).

| Parameter kind                            | IN | OUT | INOUT | VAR   | UNKOWN |
|-------------------------------------------|----|-----|-------|-------|--------|
| must be set externally before application | x  | x   | \(x\) |       |        |
| must be set during application            | x  | x   | \(x\) |       |        |
| readable after application                | x  | x   | x     | \(x\) |        |

_Feature comparison_

Examples: [Bank Accounts](Getting_started "wikilink") (*in*,
*var*), [Ecore2RDB](Ecore2RDB "wikilink") (*in*, *out*,
*var*), [Ecore2GenModel](Ecore2GenModel "wikilink")
(*in*, *out*, *var*), [Grid and Comb
Pattern](GridAndCombPattern "wikilink") (*in*, *inout*,
*out*), [Gossiping Girls](GossipingGirls "wikilink")
(*out*), [Probabilistic
Broadcast](ProbabilisticBroadcast "wikilink"), (*in*,
*var*)

## Parameter mappings

Parameters that need to be set externally can be set by the user using
the [API](#interpreter-api "wikilink") or the [Interpreter
Wizard](#interpreter-wizard "wikilink"). In addition, they can be passed
in from another unit using a **parameter mapping**. A parameter mapping
assigns a source parameter to a target parameter between a unit and its
sub-unit. Parameters that need to be set automatically during unit
application are either set during the match finding process (in the case
of LHS elements) or after the creation of new elements (in the case of
RHS elements). This behavior can be used to propagate values between LHS
and RHS elements, as exemplified in the *transferMoney* rule in the
[Bank Accounts](Getting_started "wikilink") example.

| From → / to ↓ | IN | OUT | INOUT | VAR | UNKOWN |
|---------------|----|-----|-------|-----|--------|
| IN            | x  | x   | x     |     |        |
| OUT           | x  | x   | x     |     |        |
| INOUT         | x  | x   | x     | x   |        |
| VAR           | x  | x   | x     |     |        |   

  : Allowed mappings

The legacy parameter kind *unknown* can be mapped arbitrarily depending
on its usage.

Examples: [Ecore2RDB](Ecore2RDB "wikilink") (*in*→*in*,
*out*→*out*),
[Ecore2GenModel](Ecore2GenModel "wikilink") (*in*→*in*,
*out*→*out*), [Grid and Comb
Pattern](GridAndCombPattern "wikilink") (*in*→*in*,
*out*→*out*, *in*→*inout*, *inout*→*in*, *inout*→*out*)

## Usage during definition

Parameters can be created using the tree-based or the
[graphical](Graphical_Editor "wikilink") editor. They can be
edited using the latter or the *Properties* view.

Declared parameters are used inside the unit by referencing them by
name. Parameters can be used at any place in the unit where a string
value is expected: in rules, this is the case for node names, attribute
values, edge indices, and attribute conditions. In [iterated
units](Units#Iterated_Unit "wikilink"), this is the case for the
iterations condition.

### Parameter creation and editing in graphical editor

[[/images/Henshin_Parameters_GraphicalEditor.png]]

To create or edit a parameter with the [graphical
editor](Graphical_Editor "wikilink") open the according
*\*.henshin_diagram* file. Select the name of a unit or rule by clicking
on it. Click a second time to edit the name. You can now - text-based -
add, edit and remove parameters which follow the unit/rule name
encompassed by parentheses and separated by commas. The parameter
entries adhere to the following scheme: *`<kind>
`<name>:`<type>* . Both *kind* and *type* are optional.
If you omit a *kind* the parameter kind will be *unknown*.

### Parameter creation in tree-based editor

[[/images/Henshin_Parameters_Creation_TreeEditor.png]]

To create a parameter with the tree-based editor open the according
*\*.henshin*-file. Right-click on the desired rule or unit and navigate
to *New Child → Parameter*. You can continue with editing the parameter
in its *Properties* view.

### Parameter editing in properties view

[[/images/Henshin_Parameters_Editing_PropertiesView.png]]

In the *Properties* view you can edit a parameter after selecting it in
the tree-based editor. To edit a value click in the according row of the
*Value* column.

### Parameter mapping creation

In the graphical editor parameter mappings are maintained implicitly
based on overlapping parameter names: each parameter of a unit is mapped
to all parameters of the same name in all sub-units; mappings in the
opposite direction exist as well.

[[/images/Henshin_Parameters_Creation_TreeEditor.png]]

Using the tree-based editor mappings for parameters can be created
manually. Therefore right-click the unit and select *New Child →
Parameter Mapping*. The mapping can be edited using its *Properties*
view.

[[/images/Henshin_Parameters_Editing_PropertiesView.png]]

## Usage during execution

Before unit or rule execution parameters of kind *in* and *inout* have
to be set externally. Parameters of kind *unknown* may be set externally
depending on their usage in their units/rules. This can be done using
the [Interpreter
Wizard](Interpreter#Interpreter_Wizard "wikilink") or the
[Interpreter API](Interpreter#Interpreter_API "wikilink").

### Interpreter Wizard


[[/images/Henshin_Parameter_Usage_Wizard.png]]

To set parameters using the interpreter wizard you have to open the
wizard for your unit or rule of interest. In the lower part of the popup
window is a table with the available parameters. You can set them by
editing the cells in the *Values* column.

### Interpreter API

See *Setting and Getting Parameter Values* in
[Henshin/Interpreter#Transforming_and_more](Interpreter#Transforming_and_more "wikilink").

## Remarkable usage examples

There may be some use cases for parameters, which do not obviously
emerge from the documentation so far. The following sections will
describe a variety of those use cases.

### Populate values between sub-units

A common use case for parameters in sequential units is the passing of
values between sub-units. This can be done with any parameter kind in
the sequential unit to which the according units are sub-units. If you
do not intend to use the value outside the unit, you should use the
parameter kind *var*.

![Usage example for
parameters](Henshin_Parameters_Usage_Example.png "Usage example for parameters")

In the example unit *getFatherCreateChild* the parameter *temp* stores
the intermediate result mapped from *getNode*\'s *out* parameter. The
*var* parameter *temp* is then mapped to *createChild*\'s *in*
parameter. Further parameter passings between sub-units can reuse the
same *var* parameter as an intermediate step to a subsequent sub-unit.

### Alter parameter value during rule application

Especially when using the parameter kind *inout* you may want to alter
the parameter value during rule application to output another value
different from the input. While this is easily achievable for units by
the use of parameter mappings, the approach for rules is less obvious.

In the depicted example the rule *createChild* has the parameter *param*
of type *Node* and is of the parameter kind *inout*. It creates a new
node as child of the node passed in via *param*. The notation
\"param-\>?:Node\" in the preserved node indicates that this node is
referred to by the name *param* before the transformation and is
nameless afterwards. The newly created child node is assigned to the
name *param* after the transformation. This automatically creates a
parameter mapping to the rule\'s *inout* parameter of the same name.
Overall the *inout* parameter\'s input value is replaced with the new
child node.

Please note that you can only assign nodes to an already set parameter.
It is not possible to assign the value of a node attribute to an already
set parameter.

For an additional usage illustration, consider the rule *extendColumn*
in the section *Generation of a sparse grid* of the [Grid and Comb
Pattern](GridAndCombPattern "wikilink") example.



