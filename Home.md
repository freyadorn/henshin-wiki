**Henshin** is an in-place model transformation language for the [Eclipse Modeling Framework](https://wiki.eclipse.org/Eclipse_Modeling_Framework) (EMF). It supports direct transformations of EMF model instances (endogenous transformations), as well as generating instances of a target language from given instances of a source language (exogenous transformations). Its main features are:

**Basic transformation definition and execution**

* [Rule](Graphical_Editor#editing-transformation-rules)-based transformation paradigm with [units](Units "wikilink") for managing control flow of rules
* [Graphical](Graphical_Editor) and [textual syntax](https://wiki.eclipse.org/Henshin/Textual_Editor), based on a transformation [meta-model](https://wiki.eclipse.org/Henshin/Transformation_Meta-Model "wikilink")
* Native support for endogenous transformations; support of exogenous transformations via [traces](Trace_Model "wikilink")
* Efficient in-place execution of transformations using a dedicated [interpreter](Interpreter "wikilink") with debugging support

Analysis

* A [performance profiler](Performance_Profiler "wikilink") to identify slow spots
* Support for [conflict and dependency analysis](Conflict_and_Dependency_Analysis "wikilink")
* [State space analysis](State_Space_Tools "wikilink") for verification


Advanced rule definition

* Support for [rule variants](Variant_Management "wikilink")
* Support for [automated rule generation](Rule_Generation "wikilink")
* Support for [generating application conditions from OCL constraints](OCL2AC "wikilink")

Integration with other tools

* Integration with [Xtext](Xtext_Adapter "wikilink")
* Support for [massive parallel rule execution](Code_Generator_for_Giraph "wikilink") using Apache Giraph

Resources

* [Official website](http://www.eclipse.org/modeling/emft/henshin)
* [Installation instructions](Installation_instructions "wikilink")
* [Getting started](Getting_started "wikilink")
* [Examples](Examples "wikilink")
* [FAQ](FAQ "wikilink")
* [Release Notes](Release_Notes "wikilink")
* [Projects that use Henshin](Projects "wikilink")