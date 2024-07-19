The Henshin profiler offers different visualizations especially for the matching process of Henshin transformation rules. We use the example [Ecore2RDB](https://wiki.eclipse.org/Henshin/Examples/Ecore2RDB) to explain how to invoke the profiler and what information is displayed. We have changed the transformation rule CreateTableInterrelations in such a way that we have named the nodes to be able to identified them in the visualizations of the profiler (the picture shows only a part of the rule CreateTableInterrelations).


# Using the profiler

The profiler can be invoked via the [interpreter wizard](https://wiki.eclipse.org/Henshin/Interpreter#Interpreter_Wizard) or it can be programmatically invoked via the API of the Henshin interpreter. To invoke the profiler via the interpreter wizard the check box “Use Profiler” must be selected.

**Invoke Profiler Wizard**

To call the profiler via the API, a PerformanceMonitorImpl-object must be created and passed to the method execute(ApplicationMonitor monitor). After all transformations are finished, the method showVisualizations() of the PerformanceMonitorImpl-object must be called to start the analysis and to open the GUI. showVisualizations() also terminates the monitoring. Since the terminated monitoring cannot be used to monitor further transformations, one should always ensure that showVisualizations() is called at the end. The following code shows an excerpt from Ecore2Rdb.java, which was modified to call the profiler via the API. To execute the modified code the class Ecore2Rdb must import org.eclipse.emf.henshin.monitoring.kieker.monitoring.PerformanceMonitorImpl and the two dependencies org.eclipse.emf.henshin.monitoring.kieker and org.eclipse.emf.henshin.monitoring.ui must be added to the MANIFEST.MF.

Unit unit = module.getUnit("main");
UnitApplication unitApp = new UnitApplicationImpl(engine, graph, unit, null);
 
unitApp.setParameterValue("packageName", packageName);
 
//Create a new PerformanceMonitorImpl-object
PerformanceMonitorImpl monitor = new PerformanceMonitorImpl(); 
 
//Monitor Transformation execution	
unitApp.execute(monitor);
 
EObject result = (EObject) unitApp.getResultParameterValue("schema");
System.out.println("Generated Rdb model.");
//start analysis and open GUI
monitor.showVisualizations();


# Visualizations

The Profiler offers four different visualizations, which are explained below.

## Call Graph

The Call Graph is always the first visualization that is shown by the profiler. At the top, it shows the total execution time, which is also used as maximum value of the heat scale. The tree structure represents the call hierarchy of the individual units and rules, and the execution sequence is shown from top to bottom. Each entry in the tree represents a rule or unit execution and consists of the name and the execution time in milliseconds. Each entry is also color-coded depending on its execution time. To get detailed information about the execution of a rule, one can either select a rule execution from the tabs above or double-click on a rule execution directly in the tree.

Call Graph Visualization

For our example we see that the unit main took 359ms and called the two rules CreateSchema and CreateTableInterrelations.

## Backtracking Graph

The Backtracking Graph shows detailed information about a rule execution. Information: The Backtracking Graph shows how often a binding had to be revoked for a node of the LHS. X-axis: The x-axis shows from left to right the order of the nodes of the LHS as determined by the search plan. Y-axis: The y-axis shows how often a binding had to be revoked.

Backtracking Graph

For the execution of CreateTableInterrelations one can see that for the node tabConTrace 34 times a binding was found, which was then revoked. This also hints that 34 times all available candidates for node tabConEClass were examined to see that none of them matches.

## Model Elements Graph

The Model Elements Graph shows detailed information about a rule execution. Information: The Model Elements Graph shows how many model elements were investigated during a transformation execution and how many candidates are contained in the input model. Candidates are all model elements of the same type as the node. Y-axis: The y-axis shows from top to bottom the order of the nodes of the LHS as determined by the search plan. Rectangle: The number at the bottom left (on the left side of /) indicates how many model elements were investigated during the transformation execution. The number at the bottom right (on the right side of /) indicates how many model elements were contained in the input model which were suitable candidates for the node. The percentage at the top indicates the relationship between the model elements investigated and existing candidates. This percentage also determines the color of the rectangle according to the heat scale.

Model Elements Graph

For the execution of CreateTableInterrelations, one can see that although many bindings for tabConTrace had to be revoked, not many model elements were investigated for tabConEClass. But one can also see that more model elements were investigated for tabConTrace than were contained in the input model. This means that some model elements were examined more than once.

## Candidates Graph

The Candidates Graph shows detailed information about a rule execution. Information: The Candidates Graph shows how bindings found during the transformation execution influence the possible candidates of the remaining nodes on average. X-axis: The x-axis at the top shows from left to right the order of the nodes of the LHS as determined by the search plan. Y-axis: The y-axis on the right side shows from top to bottom the changes per binding found. Rectangle: The label indicates how many candidates are available on average. This number also determines the color of the rectangle using the heat scale. To interpret the information correctly, this visualization must be interpreted row by row. The first row of rectangles (row 0) indicates how many candidates are contained in the input model at the beginning of the transformation execution. The next row (row 1) indicates how many candidates are available on average for the remaining nodes after a binding for tabConEClass has been found. The next row (row 2) shows what is happening after bindings for tabConEClass and tabConTrace were found, and so on.

Candidates Graph

For the execution of CreateTableInterrelations one can see that the candidates for tabConEReference are reduced to one after a binding for tabConEClass was found (see row 1). In addition, the candidates for tabConEClass are reduced to one after also a binding for tabConTrace has been found (see row 2).