\_\_NOTOC\_\_

[Henshin](Henshin "wikilink")\'s **Ecore2Rdb** example is an exogenous
transformation for translating an Ecore model to a relational database
model. This example makes extensive use of nested multi-rules. The
purpose of this example is to show how this concept can be used to
construct complex and powerful rules. Note that this example could be
also modeled with simple (but more) rules in Henshin. The
transformation, source code and example input & output models can be
found
[here](http://git.eclipse.org/c/henshin/org.eclipse.emft.henshin.git/tree/plugins/org.eclipse.emf.henshin.examples/src/org/eclipse/emf/henshin/examples/ecore2rdb).

### The Transformation {#the_transformation}

The transformation consist of one main unit and two rules. The main unit
is depicted below.

![](henshin-ecore2rdb-main.png "henshin-ecore2rdb-main.png"){width="400"}

The main unit is a sequential unit. It first executes the rule
*CreateSchema* and then the rule *CreateTableInterrelations*. The two
parameters of the main unit are passed to the *CreateSchema* and back
again. Note that there is no loop involved: the transformation literally
consist of two rule application, no matter how large and complex the
input model is.

The rule *CreateSchema* is shown in the screenshot below. This rule
creates the schema, all tables, and all columns. In general, the
transformation makes use of Henshin\'s generic [trace
model](Henshin/Trace_Model "wikilink"). The *CreateSchema* rule contains
the following multi-rules with these tasks:

**CreateSchema:** Creates a schema for a given package.

***tab***: Creates a table for every class in the parent package.

***col***: Creates a column for every attribute in the parent class.

***PKey***: Marks the columns of ID-attributes as key columns.

***newPKey***: Creates a new key column if there exists no
ID-attributes.

![](henshin-createSchema.png "henshin-createSchema.png"){width="700"}

The second rule, called *CreateTableInterrelations*, is shown below.
This rule creates the for all EReferences the corresponding table
relations. For 1..1 references, the rule creates a new column in the
table and a foreign key pointing to the target column. For 1..n
references, the rule creates a whole new table that contains the links.

![](henshin-createRelations.png "henshin-createRelations.png"){width="700"}

### Example

You can find the example Ecore file *CarRental.ecore* in the same
directory. To apply the transformation to this model, open the file
*ecore2rdb.henshin* in the tree-based Henshin editor. Right-click on the
main unit and select *Apply Transformation*. In the dialog, click on
*Browse Workspace* and choose the *CarRental.ecore* file. Now change the
parameter type of the parameter *packageName* to String and enter
*CarRentalModel* as its value. You can now click on Preview and you
should see this compare dialog:

![](henshin-ecore2rdb-diff.png "henshin-ecore2rdb-diff.png"){width="400"}

It shows that the created Schema object and its contents. There is also
a new Trace object which could be used to check the correspondences
between the Ecore and the RDB model. There is also a stand-alone Java
application in the example directory which you can use to execute the
transformation programmatically.

This example requires Henshin 0.9.3 or higher.

*contributed by Stefan Jurack and Christian Krause*

[`category:Henshin`](category:Henshin "wikilink")
