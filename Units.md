In Henshin, control flow is specified using **units**. Units have a
fixed number of sub-units, allowing for arbitrary nesting. This article
describes the available units.

Units can be categorized by their possible number of sub-units. For
**unary units** the number of sub-units is exactly one. **Multi-units**
have an arbitrary number of sub-units. The specific unit *Conditional
Unit* has two or three sub-units.

Each unit checks the executability of at least one sub-unit, if a
sub-unit exists. This check means the sub-unit is executed.

## Usage

### Unit creation in graphical editor

[[/images/Henshin_Units_Creation_GraphicalEditor.png]]

To add a unit to a Henshin project in the [graphical
editor](GraphicalEditor "wikilink"), first open the
*\*.henshin_diagram* file. Then select *Unit* from the *Palette* on the
right of the window. Afterwards left-click any empty space of the
Henshin diagram and choose the desired unit from the appearing menu.

[[/images/Henshin_Units_Add_Invocation.png]]

If the selected unit is a Multi-Unit, some sub-units have to be added
manually. Therefore select *Invocation* from the *Palette* and click on
the according Multi-Unit. Then type the name of the desired rule or
unit. Currently every sub-unit is inserted behind all existing
sub-units, which is especially relevant for *PriorityUnit*s and
*SequentialUnit*s. If that\'s not the desired behaviour it is
recommended to use the *Properties* view or the tree-based editor for
sub-unit insertion or reordering.

### Unit creation in tree-based editor

To add a unit to a Henshin project in the tree-based editor, first open
the *\*.henshin* file. Then right-click on the parent *Module*. Select
*New Child* and afterwards the desired unit from the context menu. After
creation the units can be edited in their *Properties* view.

[[/images/Henshin_Units_Creation_TreeEditor.png]]

## Unary Units

A Unary Unit has exactly one sub-unit.

### Loop Unit

_Apply A as often as possible_

[[/images/Henshin_Loop_Unit.png]]

-   **Number of sub-units:** 1
-   **Available Flags/Properties:** *none*
-   **Execution successful:** always
-   **Control flow:** The sub-unit is executed as often as it is
    executable.
-   **Examples:**
    [Java2StateMachine](Java2StateMachine "wikilink"),
    [Ecore2GenModel](Ecore2GenModel "wikilink")

### Iterated Unit

_Apply A a specified number of times_

[[/images/Henshin_Iterated_Unit.png]]

-   **Number of sub-units:** 1
-   **Available Flags/Properties:** iterations (integer), strict,
    rollback (both of type boolean)
-   **Execution successful if:**
    -   **strict=true:** all iterations successful
    -   **strict=false:** at least one iteration successful
-   **Control flow:** The sub-unit is executed as often as specified in
    the *iterations* property.
    -   **strict=false, rollback=true/false:** If one of the iterations
        cannot be executed, the next iteration is executed. As the
        execution never stops, the *rollback* flag has no effect.
    -   **strict=true, rollback=false:** If one of the iterations cannot
        be executed, the execution stops.
    -   **strict=true, rollback=true:** If one of the iterations cannot
        be executed, the execution stops and previous executions are
        reverted.
-   **Examples:** [Grid and Comb
    Pattern](GridAndCombPattern "wikilink")

## Conditional Unit

_If A applicable then apply B, else apply C_

[[/images/Henshin_Conditional_Unit.png]]

-   **Number of sub-units:** 2 or 3
-   **Available Flags/Properties:** *none*
-   **Execution successful if:** *if* unit and *then* unit are
    successful or *if* unit is unsuccessful while *else* unit is
    successful or not present.
-   **Control flow:** If a match for the *if* unit can be found, the
    *then* unit is executed. Otherwise, if present, the *else* unit is
    executed.
-   **Examples:**
    [Java2StateMachine](Java2StateMachine "wikilink")

## Multi-Units

A Multi-Unit has an arbitrary number of sub-units.

### Sequential Unit

_Apply all sub-units in the specified order (first A, then B, then C...)._

[[/images/Henshin_Sequential_Unit.png]]

-   **Number of sub-units:** arbitrary (0..\*)
-   **Available Flags/Properties:** strict, rollback (both of type
    boolean)
-   **Execution successful if:**
    -   **strict=true:** all sub-units successful or no sub-units
        existing
    -   **strict=false:** at least one sub-unit successful or no
        sub-units existing
-   **Control flow:** The sub-units are executed in the given order.
    -   **strict=false:** If one of the sub-units cannot be executed,
        the next sub-unit is executed. As the execution never stops, the
        *rollback* flag has no effect.
    -   **strict=true, rollback=false:** If one of the sub-units cannot
        be executed, the execution stops.
    -   **strict=true, rollback=true:** If one of the sub-units cannot
        be executed, the execution stops and previous executions are
        reverted.
-   **Examples:** [Ecore2RDB](Ecore2RDB "wikilink"),
    [Java2StateMachine](Java2StateMachine "wikilink"),
    [Ecore2GenModel](Ecore2GenModel "wikilink"), [Grid
    and Comb Pattern](GridAndCombPattern "wikilink"),
    [Movies](Movies "wikilink")

### Priority Unit

_Apply all sub-units in the specified order, until one is applicable ("Try to apply A. If A not applicable, try B etc.")._

[[/images/Henshin_Priority_Unit.png]]

-   **Number of sub-units:** arbitrary (0..\*)
-   **Available Flags/Properties:** *none*
-   **Execution successful if:** one sub-unit successful (Empty priority
    units always fail.)
-   **Control flow:** The sub-units are checked in the given order for
    executability. The first sub-unit found to be executable is
    executed.
-   **Examples:**
    [Java2StateMachine](Java2StateMachine "wikilink")

### Independent Unit

_Apply all sub-units in non-deterministic order, until one is applicable ("Choose A, B or C in non-deterministic order. If not applicable, try another one in non-deterministic order. If it is not applicable, etc.")._


[[/images/Henshin_Independent_Unit.png]]


-   **Number of sub-units:** arbitrary (0..\*)
-   **Available Flags/Properties:** *none*
-   **Execution successful:** one sub-unit succesful (Empty independent
    units always fail.)
-   **Control flow:** The sub-units are checked in nondeterministic
    order for executability. The first sub-unit found to be executable
    is executed.
-   **Examples:**
    [Ecore2GenModel](Ecore2GenModel "wikilink")



