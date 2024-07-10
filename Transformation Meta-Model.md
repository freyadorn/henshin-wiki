The **Henshin transformation meta-model** defines the concepts used for
the specification of model transformations in Henshin.

A complete specification is encapsulated by a **module**. A module is a
container for a set of units. There are two kinds of units: rules and
composite units. Rule specify atomic building blocks for
transformations. Composite units enable the orchestration of multiple
rules in a control flow.

### Basic building blocks: Rules

[[/images/Henshin_Transformatin_Modules.png]]

**Rules** are the basic building blocks for model transformations. A
rule comprises two graphs, a left-hand side (LHS) and a right-hand side
(RHS) graph. The LHS describes a pattern to be matched in the input
model. The RHS specifies a change on the input model.

LHS and RHS specify model patterns on the abstract syntax level: Nodes
represent model elements, edges represent references between model
elements. Nodes and edges occurring in the LHS or RHS only are deleted
or created, respectively. Node mappings between LHS and RHS declare
identity, that is, nodes and edges being preserved. Nodes possess
attributes representing the attributes of the respective model elements.
Their values may either be literals - e.g. 0 or \"Hello\" -, parameters,
or JavaScript expressions, being evaluated by a JavaScript engine at
runtime. Nodes, edges and attributes have a type, being a class,
reference or attribute in an EMF model. Attribute conditions can impose
additional boolean conditions that are bound to the rule.

For conciseness, the [graphical
editor](Graphical_Editor "wikilink") merges LHS and RHS into an
integrated representation: Nodes and edges are annotated with a
*«preserve»*,
*«create»*, or
*«delete»* tag depending
on their containment in LHS or RHS graphs.

**Dangling condition**: A common beginner-level problem is that rules
are deemed unexecutable by the Henshin interpreter despite looking
correct at the surface. This behavior is often related to the *dangling
condition*: A rule execution may not leave behind dangling edges, being
edges with a missing source or target. It is possible to turn off the
dangling condition check using the *checkDangling* Boolean flag. Setting
the flag to false will lead to the interpreter deleting the dangling
edges.

**Injective matching**: The *injectiveMatching* flag specifies whether
the match-finder is supposed to find matches that assign two or more LHS
nodes to the same model element. Per default, matching is set injective
(that is, not supposed to assign nodes to the same model element).

**Edges**: A few remarks on edges: First, rules do not maintain mappings
of edges between different rule graphs: Edges are considered identical
if their types and their source and target nodes are identical. Second,
if a reference has an *EOpposite* reference, it is not required to
specify an edge for the opposite reference: By its default behavior, EMF
ensures that changes involving one of the EOpposites will be propagated
onto the other. Third, in *ordered* references, the positions of list
entries can be modified using the *index* attribute. An example is shown
in [this
article](http://web.archive.org/web/20160722080730/http://www.ckrause.org:80/2013/05/henshin-098-working-with-lists.html).

### Advanced concepts: Application conditions and rule nesting

[[/images/Henshin_Application_Conditions.png]]

**Application conditions** are graph patterns that restrict the LHS of a
given rule. They may require the presence of additional elements or
relationships not included in the LHS. In this case, they are referred
to as *positive application conditions* (PACs). Furthermore, they may
forbid the presence of elements or relationships. In this case, they are
referred to as *negative application conditions* (NACs). Consequently,
the [graphical editor](Graphical_Editor "wikilink") displays PAC
elements with a
*«require»* and NAC
elements with a
*«forbid»* tag.
Application conditions can be arbitarily nested using the propositional
operators *Or*, *And*, *Xor*, and *Not*. Whether an application
condition graph represents a NAC or a PAC is determined by whether it is
contained in a *Not* formula.\
Since LHS and PACs specify graph patterns required to be present, you
may ask now if PACs just add a syntactic sugar on top of the LHS. This
is not the case: PACs (and NACs) are used to constrain the matching of
the LHS. However, they are not part of the computed match. A match only
contains mappings for LHS nodes. This is particularly important in
scenarios where you want to apply a rule for each computed match
(compare *rule-nesting*, described below). For an example, please refer
to [Movies](Examples/Movies "wikilink").\
Identity of nodes in different graphs, including LHS, PAC, and NAC
graphs, can be specified using mappings. The graphical editor maintains
these mappings implicitly: For each LHS node, it creates a mapped node
inside the NAC or PAC.\
*Examples:* [Bank Accounts](Getting_started "wikilink"),
[Ecore2RDB](Examples/Ecore2RDB "wikilink"),
[Java2StateMachine](Examples/Java2StateMachine "wikilink"),
[Ecore2GenModel](Examples/Ecore2GenModel "wikilink"), [Grid and
Comb Pattern](Examples/GridAndCombPattern "wikilink"),
[Gossiping Girls](Examples/GossipingGirls "wikilink"),
[Probabilistic
Broadcast](Examples/ProbabilisticBroadcast "wikilink"),
[Movies](Examples/Movies "wikilink")

**Rule-nesting** is a powerful concept providing a for-each operator for
rules. In nested rules, the outer rule is referred to as *kernel rule*
and the inner rule as *multi rule*. During execution of a nested rule,
the kernel rule is matched and executed once. Afterwards, the match is
used as a starting point to match the multi-rule as often as possible
and execute it for each match. Multi mappings allow to specify identity
between kernel and multi-rule nodes. In the [graphical
editor](Graphical_Editor "wikilink"), multi-rule nodes are
indicated by a layered representation and an asterisk (\*). Multiple
layers of rule-nesting are allowed. The graphical editor shows the path
constituted by the nested rules in the title bar of the respective
nodes. An extensive example for multiple-layer rule-nesting is found in
[Ecore2RDB](Examples/Ecore2RDB "wikilink").\
*Examples:* [Bank Accounts](Getting_started "wikilink"),
[Ecore2RDB](Examples/Ecore2RDB "wikilink"), [Gossiping
Girls](Examples/GossipingGirls "wikilink"), [Probabilistic
Broadcast](Examples/ProbabilisticBroadcast "wikilink")\
*Further Reading: Biermann, Ermel, Taentzer. \"Formal foundation of
consistent EMF model transformations by algebraic graph
transformation.\" Software & Systems Modeling 11.2 (2012): 227-250.
Chapter 5.
[SpringerLink](http://link.springer.com/article/10.1007/s10270-011-0199-7)*

### Control flow: Units

[[/images/Henshin_Transformation_Units.png]]

*Main article (with examples): [Units](Units "wikilink")*

In Henshin, control flow is specified using units. Units have a fixed
number of sub-units, allowing for arbitrary nesting. The following unit
types are available:

**Loop Units** have one sub-unit. The sub-unit is executed as often as
it is executable.

**Iterated Units** have one sub-unit. The sub-unit is executed as often
as specified in the *iterations* property. The Boolean flag *strict*
determines the behavior if not all of the specified iterations can be
executed: In strict mode the execution is considered unsuccessful in
this case, otherwise, the execution is considered successful if at least
one iteration has been executed successfully. The Boolean flag
*rollback* determines whether, in case of an unsuccessful unit
execution, successful iterations are reverted.

**ConditionalUnits** have either two or three sub-units: *if*, *then*,
(and *else*). If a match for the *if* unit can be found, the *then* unit
is executed. Otherwise, if present, the *else* unit is executed. Note
that any changes specified in the if unit are performed; if you don\'t
want to specify changes, you can use a rule with *preserve* nodes only.

**Sequential units** have an arbitrary number of sub-units that are
executed in the given order. The Boolean flag *strict* determines the
behavior if one of the sub-units cannot be executed: In strict mode, the
execution stops, otherwise, the next sub-unit is executed. The Boolean
flag *rollback* determines whether in the case of a stopped execution
previous executions are reverted. Stopped executions are always
considered unsuccessful. Otherwise, executions are considered successful
if at least one sub-unit has been executed successfully, or if the unit
contains no sub-units.

**Priority Units** have an arbitrary number of sub-units that are
checked in the given order for executability. One sub-unit - the first
one found to be executable - is executed.

**Independent Units** have an arbitrary number of sub-units that are
checked in nondeterministic order for executability. One sub-unit - the
first one found to be executable - is executed.

### Execution-time variability: Parameters

*Main article (with examples):
[Parameters](Parameters "wikilink")*

[[/images/Henshin_Parameters.png]]

**Parameters** allow to shape the behavior of units, including rules,
with variable information. A unit can have an arbitary number of
parameters. Parameters have a name, a description, a kind, and,
optionally, a type. The kind specifies the time when the parameter is
bound to a concrete value, and whether the parameter is intended to be
accessed after the unit has been applied. There are four parameter kinds
(*in*, *out*, *inout*, *var*) and an additional legacy parameter kind
(*unknown*):

-   *in* parameters need to be set externally before the unit
    application; they cannot be read once the unit has been applied,
-   *out* parameters are set automatically during the unit application;
    they are intended to be read once the unit has been applied,
-   *inout* parameters need to be set externally before the unit
    application; they are intended to be read once the unit has been
    applied,
-   *var* parameters (variables) are set automatically during the unit
    application; they cannot be read once the unit has been applied,
-   *unknown* parameters can either be set externally before or
    automatically during the unit application; they may be read once the
    unit has been applied.

The type of a parameter is an arbitrary *EClassifier*, that is, either
an *EDataType* such as *EString*, *EInt* or *EBoolean*, or any *EClass*.

Declared parameters are *used* inside the unit by referencing them by
name. Parameters can be used at any place in the unit where a string
value is expected: in rules, this is the case for node names, attribute
values, edge indices, and attribute conditions. In iterated units, this
is the case for the *iterations* condition.

[[/images/Henshin_Annotations.png]]

### Adding meta-information: Annotations

**Annotations** are a mechanism for supporting non-intrusive extensions
of the Henshin language: Each model element from a Henshin module can be
annotated with Annotations. To this end, each metaclass transitively
inherits from a metaclass *Model Element*, which has an arbitrary number
of annotations. An annotation has a key and a value, both being strings.

*Example:* In Henshin\'s [variant
management](Variant_Management "wikilink") feature, variant
management information is added to rules and rule elements via
annotations.



