In version 0.9.0 of Henshin, we saw the need for making some important
changes to the Henshin transformation model which are incompatible with
the previous model. In order to use transformations written with
pre-0.9.0 versions of Henshin with more recent versions, you need to
migrate your transformation models. To support you in this matter, we
have implemented a **migration wizard** which will convert the old
format into the new one.

### How to use the wizard

To convert a Henshin transformation into the new format, invoke the
migration wizard by right-clicking on a \*.henshin file in the package
explorer and choosing *Henshin -\> Migration Wizard*. Now you will see
the following dialog:

![](Henshin_migration_wizard.png "Henshin_migration_wizard.png")

On the top you choose the transformation file to be converted. On the
bottom, you can additionally specify a diagram file which should be
migrated together with the transformation.

Now you need to decide two things in the wizard:

-   **Optimize nested conditions**: will attempt to perform some
    automatic simplifications for nested conditions.
-   **Keep separate kernel and multi-rules**: if enabled, the copies of
    the kernel and multi-rules of all AmalgamationUnits will be kept on
    the top-level of the transformation system. This is option MUST be
    enabled if you use a kernel or a multi-rule outside of an
    AmalgamationUnit. Please see below for detailed information about
    the new format for amalgamation units.

The wizard will create back-up files before doing the migration.

**Important note on diagram migrations:** You cannot migrate the diagram
file at a later stage \-- you must migrate the *\*.henshin* and the
*\*.henshin_diagram* files together. If you for some reason forgot to
migrate your diagram file and try to open the un-migrated diagram file
in the graphical editor, you will get an error message. If you then
migrate the diagram file from the back-up, you will still get an error
message when trying to open it. The solution is to restart Eclipse.
Then, you will be able to open the diagram file correctly.

### Model changes

From version 0.8.0 to 0.9.0 of Henshin, the following changes where
made:

-   **Namespace URI**: changed from
    *<http://www.eclipse.org/emf/2010/henshin>* to
    *<http://www.eclipse.org/emf/2011/henshin>*. Therefore, you will not
    be able to open old files in the editors anymore.
-   **NestedCondition**: removed the *negated*-flag. Wrap your
    *NestedCondition* in a *Not* instead.
-   **CountedUnit**: REMOVED. If you want to implement a for-loop, use a
    *SequentialUnit* instead. If you want to implement a while-loop use
    *LoopUnit* (see also below).
-   **LoopUnit**: ADDED. New unit type for while-loops.
-   **AmalgamationUnit**: REMOVED. Instead, rules now have a containment
    reference *multiRules* and *multiMappings* which allows you to
    specify nested rules of unbounded depth. As it was in amalgamation
    units, multi-rules have a forall-semantics. The mappings from a
    parent rule into a multi-rule are stored in the *multiMappings*
    field of the multi-rule!!!
-   **Rule**: has to new Boolean attributes: *checkDangling* and
    *injectiveMatching*. With *checkDangling* you can specify whether to
    use DPO (true) or SPO (false) graph transformation semantics. The
    former checks whether there are any dangling edges and forbids the
    rule application in this case. The latter automatically deletes all
    dangling edges. The attribute *injectiveMatching* is used to specify
    whether rules are matched injectively. Note that you can specify
    these properties for all rules separately now.

### API changes

There are also a few changes in the runtime API. Most importantly, the
class *ModelHelper* which was earlier used to load and save models etc.
is no deprecated. Instead, you should use the functionality provided in
*HenshinResourceSet*. For more details, see the example at the [Henshin
Interpreter](Interpreter "wikilink") wikipage.



