[Henshin](Home "wikilink")\'s **Java2StateMachine** example is an
exogenous transformation solving the Reengineering case of the
[Transformation Tool Contest (TTC)
2011](https://www.transformation-tool-contest.eu/). This example primarily
shows the use of transformation unit and parameter passing. Some of the
details could be modeled differently and more efficiently using nested
rules.

A detailed description of the transformation can be found in *Stefan
Jurack, Johannes Tietje: [Solving the TTC 2011 Reengineering Case with
Henshin](http://arxiv.org/abs/1111.4752v1). Proceedings of
[TTC\'11](http://planet-research20.org/ttc2011/), Electronic Proceedings
in Theoretical Computer Science (EPTCS), 2011, Volume 74*.

The whole transformation is depicted in the following screenshot. The
models, transformation, source code and example input / output models
can be found
[here](https://github.com/eclipse-henshin/henshin/tree/master/plugins/org.eclipse.emf.henshin.examples/src/org/eclipse/emf/henshin/examples/java2statemachine).

[[/images/henshin-java2statemachine.png]]

We ran the transformation for three input models of different sizes.
These input models were provided by the TTC organizers. We measured the
time needed for transforming the Java models into state machines. The
benchmark was conducted on a Intel(R) Xeon(R) CPU @ 2.50GHz with 8GB of
main memory using Henshin 0.9.2. All times do not include the loading of
the models and the initialization of the transformation engine. The
correctness of the transformation was automatically checked by comparing
the generated result with the reference model provided by the TTC
organizers.

| Input model             | Number of objects | Transformation time |
|-------------------------|-------------------|---------------------|
| 1-java-model-small.xmi  | 6.117             | 1372ms              |
| 2-java-model-medium.xmi | 6.400             | 1418ms              |
| 3-java-model-big.xmi    | 737.102           | 1729ms              |

*contributed by Stefan Jurack, Johannes Tietje and Christian Krause*


