The textual Henshin editor supports the specification of transformation
rules and units with syntax highlighting, quick fixes and content
assist.

# Creating a new henshin_text file

To create a new henshin_text file you need to open New wizard and select
Henshin Text from the category Henshin. The wizard will create a new
henshin_text file and will add the XText nature to your project.

# Specifying transformation rules and units

## Specifying transformation rules

The below code shows the textual definition of the transformation rule
*createAccount* ([bank
example](https://www.eclipse.org/henshin/examples.php?example=bank)).
The definition of this rule starts with the keyword **rule**, followed
by its unique name *createAccount*. The two parameters *client* and
*accountId* are defined by specifying their kind, followed by their name
and their type. The nodes and edges of the rule are then defined within
a **graph** element.

``` text
ePackageImport bank

rule createAccount(IN client:EString,IN accountId:EInt){
    graph{
        node bank:Bank
        preserve node manager:Manager
        preserve node client:Client
        
        create node newAccount:Account{
            create id=accountId
        }
        
        forbid node account:Account{
            id=accountId
        }
        
        edges[(bank->manager:managers),(preserve bank->client:clients),(forbid bank->account:accounts)]
        
        edges[(manager->client:clients),(create client-> newAccount:accounts),(create bank-> newAccount:accounts)]
    }
}
```

The definition of a node starts with its action (**preserve**,
**create**, **delete**, **forbid**, **require**), followed by the
keyword **node**, a unique name for the node and its type. Nodes without
an action, e.g. *bank*, have always the action **preserve**. If a node
has attributes, these are written in curly brackets after its
definition. For each attribute its action can be defined, as for the
*id* of *newAccount* in the code example. Is no action defined an
attribute has the same action as its node. E.g., the attribute *id* of
*account* has the action **forbid**. Edges between nodes are defined as
list after the keyword **edges**. Several lists of edges can be defined,
as shown in the code example. An edge definition is always enclosed in
brackets and consists of its action, the name of the source node, -\>,
the name of the target node and its type. If no action is specified, as
it is for *(bank-\>manager:managers)*, the action of the edge is
**preserve**.

## Specifying multi-rules

The below code shows the textual definition of the transformation rule
scheduleOfferedCourse ([university courses
example](https://www.eclipse.org/henshin/examples.php?example=universitycourses)).
Within the element **graph** the multi-rule to associate students with
the ScheduledCourse is defined after the keyword **multiRule** and its
name moveStudentsToScheduledCourse. Inside a **multiRule** there is
always a **graph** element in which the multi-rule can be defined the
same way as a transformation rule. To use nodes from the kernel rule in
a multi-rule, only their names must be used. For example, inside the
multi-rule the edge *(**delete** student-\>offered:isInterestedIn)*,
defines that an edge from the node *student* (part of the multi-rule) to
the node *offered* (part of the kernel rule) should be deleted.

``` text
rule scheduleOfferedCourse(IN hour:EInt, OUT offered:OfferedCourse, VAR name:EString){
    conditions [hour>=0, hour<24]
    graph{
        node root:University
        
        node offered:OfferedCourse{
            name=name
        }
        
        node lecturer:Lecturer
        
        edges[(root->offered:courses),(root->lecturer:persons),(delete lecturer->offered:canTeach)]
        
        create node scheduled:ScheduledCourse{
            name=name
            startingHour=hour
        }
        
        edges[(create lecturer->scheduled:teaches),(create root->scheduled:courses),(create scheduled->lecturer:lecturers)]
        
        forbid node notScheduled:ScheduledCourse{
            startingHour=hour
        }
        edges[(forbid root->notScheduled:courses),(forbid lecturer->notScheduled:teaches),(forbid notScheduled->lecturer:lecturers)]
        
        require node requiredStudent:Student
                
        edges[(require root->requiredStudent:persons),(require requiredStudent->offered:isInterestedIn)]
        
        
        multiRule moveStudentsToScheduledCourse {
            graph{
                node student:Student
                edges[(delete student->offered:isInterestedIn), (create student->scheduled:isInterestedIn), (root->student:persons)]
            
                forbid node forbidScheduledCourseMove:ScheduledCourse{
                    startingHour=hour
                }
                
                edges[(forbid root->forbidScheduledCourseMove:courses),(forbid student->forbidScheduledCourseMove:isInterestedIn)]
                
            }
        }
        
    }
}
```

## Specifying Application Conditions

There are two ways to specify application conditions with the textual
syntax for Henshin. One way is to annotate nodes with the actions
**forbid** and **require**. An example for such annotated nodes can be
seen in the example code in section 2.1. Specifying Multi-rules. If the
actions **require** and **forbid** are combined, as in the example, a
positive application condition (PAC) and a negative application
conditions (NAC) are created, which are combined using **AND** (more
about PAC and NAC
[here](https://wiki.eclipse.org/Henshin/Transformation_Meta-Model#Advanced_concepts:_Application_conditions_and_rule_nesting)).
The other way is to define individual graphs to specify application
conditions. This option also allows to express more complex application
conditions. The below code shows the textual definition of the
transformation rule *scheduleOfferedCourse* ([university courses
example](https://www.eclipse.org/henshin/examples.php?example=universitycourses)).
In this example the application condition is defined within the element
**matchingFormula**. Inside a **matchingFormula** positive application
conditions are defined as individual graphs. A graph definition starts
with the keyword **conditionGraph** and a unique name, followed by the
graph definition consisting of nodes and edges. Within these graphs,
nodes from the left-hand side (LHS) can be used by using their names.
E.g., within the application condition *StudentIsInterested* the edge
(root-\>requiredStudent:persons) connects the node *root* from the LHS
with the node *requiredStudent* from the PAC. To combine the graphs
defining application conditions and to define NACs the keyword
**formula** within the element **matchingFormula** is used. After the
keyword **formula**, the application conditions can be combined with
**OR**, **AND**, and **XOR** using their names. To define a NAC a **!**
must be written before the name. The below code example shows that the
application condition *teachAnotherScheduledCourseSameTime* is a NAC,
that is combined with the PAC *StudentIsInterested* by an **AND**.

``` text
rule scheduleOfferedCourseFormula(IN hour:EInt, OUT offered:OfferedCourse, VAR name:EString){
    conditions [hour>=0, hour<24]
    graph{
        node root:University
        
        node offered:OfferedCourse{
            name=name
        }
        
        node lecturer:Lecturer
        
        edges[(root->offered:courses),(root->lecturer:persons),(delete lecturer->offered:canTeach)]
        
        create node scheduled:ScheduledCourse{
            name=name
            startingHour=hour
        }
        
        edges[(create lecturer->scheduled:teaches),(create root->scheduled:courses),(create scheduled->lecturer:lecturers)]
        
        matchingFormula {
            formula !teachAnotherScheduledCourseSameTime AND StudentIsInterested
            
            conditionGraph teachAnotherScheduledCourseSameTime{
                node notScheduled:ScheduledCourse{
                    startingHour=hour
                }
                edges[(root->notScheduled:courses),(lecturer->notScheduled:teaches),(notScheduled->lecturer:lecturers)]
            }
            
            conditionGraph StudentIsInterested{
                node requiredStudent:Student
                
                edges[(root->requiredStudent:persons),(requiredStudent->offered:isInterestedIn)]
            }
            
        }
        
        
        multiRule moveStudentsToScheduledCourse {
            graph{
                node student:Student
                edges[(delete student->offered:isInterestedIn), (create student->scheduled:isInterestedIn), (root->student:persons)]
            
                forbid node forbidScheduledCourseMove:ScheduledCourse{
                    startingHour=hour
                }
                
                edges[(forbid root->forbidScheduledCourseMove:courses),(forbid student->forbidScheduledCourseMove:isInterestedIn)]
                
            }
        }
        
    }
}
```

## Specifying Units

The following code examples show how to define the different
[units](https://wiki.eclipse.org/Henshin/Units). The Independent Unit is
taken from the ([example
Ecore2GenModel](https://www.eclipse.org/henshin/examples.php?example=ecore2genmodel))
and the other units are taken from the ([example University
Courses](https://www.eclipse.org/henshin/examples.php?example=universitycourses)).

### Independent Unit

``` text
unit generateGenModel(IN modelFileName:EString, IN pluginName:EString, OUT genModel:GenModel){
    independent [createGenModel(modelFileName, pluginName, genModel), createGenPackage(), createGenClass(), createGenFeatureForAttribute(), createGenFeatureForReference()]
}
```

### Priority Unit

``` text
unit planOrCleanup(IN startHour:EInt){
    priority [ planAllCoursesOrFail(startHour), cleanupUninterestingCourses()]
}
```

### Sequential Unit

``` text
unit planAllCoursesOrFail(IN startHour:EInt){
    existsUnscheduledInterestingCourse()
    planCourseOrIncrement(startHour)
}
```

### Loop Unit

``` text
 
unit cleanUpUniterestingCoursesUnit(){
    while{
        cleanupUninterestingCourses()
    }
}
```

### Iterated Unit

``` text
unit manageCourses(IN startHour:EInt){
    for(2){
        planOrCleanup(startHour)
    }
}
```

### Conditional Unit

The first example code shows a Conditional Unit with **else**-part the
second example is a Conditional Unit without **else**-part.

``` text
unit planCourseOrIncrement(IN currentHour:EInt){
    if (planOneCorse(currentHour)) then {
        planUnscheduledInterestingCourses(currentHour)
    }else {
        incrementIfPossible(currentHour)
    } 
} 
```

``` text
unit planUnscheduledInterestingCourses(IN currentHour:EInt){
    if(existsUnscheduledInerestingCourse())then {
        planCourseOrIncrement(currentHour)
    }
}
```

In the textual syntax it is also possible to nest different unit
definitions. E.g., the below code shows the definition of the
Conditional Unit *planUnscheduledInterestingCourses*, which calls in its
**then**-part the Conditional Unit *planCourseOrIncrement*. But in this
example, the definition of *planCourseOrIncrement* is nested.

``` text
unit planUnscheduledInterestingCourses(IN currentHour:EInt){
    if(existsUnscheduledInerestingCourse())then {
        //Start: nested definition of planCourseOrIncrement
        if (planOneCorse(currentHour)) then {
            planUnscheduledInterestingCourses(currentHour)
        }else {
            incrementIfPossible(currentHour)
        }
        //End: nested definition of planCourseOrIncrement
    }
}
```

# Running a transformation rule or unit

To run a transformation rule or unit you need to right-click on your
henhin_text file and select „Apply transformation". This will invoke the
[interpreter
wizard](https://wiki.eclipse.org/Henshin/Interpreter#Interpreter_Wizard).
It is also possible to generate a henshin file. For this you need to
right-click on your henhin_text file and select „Transform to Henshin".


