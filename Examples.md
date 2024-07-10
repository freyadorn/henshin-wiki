
This page lists documented **examples** of [Henshin](Henshin "wikilink")
transformations. All examples can be found in the [Henshin examples
plug-in](https://git.eclipse.org/c/henshin/org.eclipse.emft.henshin.git/tree/plugins/org.eclipse.emf.henshin.examples).

### Endogenous Transformations

Endogenous transformations are transformations on a single model. Please
take also a look under *Higher-order (HO) Transformations*, *State Space
Analysis* and *Giraph Code Generation* for examples of endogenous
transformations.

1.  [Bank Accounts](Getting_started "wikilink") (Tutorial):
    shows the basic concepts, the graphical editor and the interpreter
    wizard.
2.  [Sierpinski Triangle](Examples/Sierpinski "wikilink"): A
    simple example for benchmarking the interpreter.
3.  [University
    Courses](Examples/University_Courses "wikilink"): An example
    showing the capabilities and usage of Units.

### Exogenous Transformations

Exogenous transformations are transformations on multiple models, e.g.,
translations between different DSLs. Please take also a look under
*Higher-order (HO) Transformations* for examples of exogenous
transformations.

1.  [Ecore2RDB](Examples/Ecore2RDB "wikilink"): Classic class
    diagram to relational database example.
2.  [Java2StateMachine](Examples/Java2StateMachine "wikilink"):
    Translating a Java model into a state machine. This is the
    Reengineering case of TTC\'11.

### Higher-Order (HO) Transformations

Higher-order transformations modify or translate transformation models,
e.g. Henshin transformations.

1.  [Ecore2GenModel](Examples/Ecore2GenModel "wikilink"):
    Translating an Ecore model into a GenModel. This was one of the
    challenges in the TTC\'10.
2.  [Grid and Comb
    Pattern](Examples/GridAndCombPattern "wikilink"):
    Construction of grid structures, a higher-order transformation for
    modifying a Henshin rule, and several benchmarks. This example was
    initially described in a technical report on benchmarks for graph
    transformation.

### State Space Analysis

These are examples showing how to use the state space generation and
analysis features of Henshin.

1.  [Dining
    Philosophers](Examples/DiningPhilosophers "wikilink"):
    Simple state space generation example and benchmark for the
    classical dining philosophers problem.
2.  [Gossiping Girls](Examples/GossipingGirls "wikilink"):
    Solution of the gossiping girls problem in Henshin.
3.  [Probabilistic
    Broadcast](Examples/ProbabilisticBroadcast "wikilink"):
    Example of a probabilistic graph transformation system for analyzing
    message broadcasting in wireless sensor networks. This example is
    taken from an ICGT\'12 paper.

### Giraph Code Generation

These are examples of uses of the Giraph code generator provided by
Henshin.

1.  [Movies](Examples/Movies "wikilink"): Simplified version of
    the example used in the paper *Implementing Graph Transformations in
    the Bulk Synchronous Parallel Model*.

### Critical Pair Analysis

These are examples showing how to use the critical pair analysis feature
of Henshin.

1.  [Simple Refactoring](Examples/SimpleRefactoring "wikilink"):
    Example of simple class modeling refactorings analysed for their
    conflicts and dependencies.

\
*If you want to share your Henshin transformation with us and all other
users, do not hesitate to send your example to our [mailing
list](https://dev.eclipse.org/mailman/listinfo/henshin-dev).*


