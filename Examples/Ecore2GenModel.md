[Henshin](Home "wikilink")\'s **Ecore2GenModel** example is an
exogenous transformation for translating an Ecore model to a GenModel.
This transformation was implemented for the [Transformation Tool Contest
(TTC) 2010](http://planet-research20.org/ttc2010). It includes a
higher-order (HO) transformation for Henshin. Please note that this
example is intended as an illustration for Henshin, not as a replacement
for EMF\'s [GenModel generation
functionality](https://eclipsesource.com/blogs/tutorials/emf-tutorial/).

A detailed description of the transformation can be found here: *Enrico
Biermann, Claudia Ermel, Stefan Jurack: [Modeling the \"Ecore to
GenModel\" Transformation with EMF
Henshin](https://core.ac.uk/download/pdf/11476089.pdf). Proceedings of
[TTC\'10](https://www.transformation-tool-contest.eu/).*

The transformation consists of two parts: the generation of the GenModel
and a customization which removes trace links using a higher-order (HO)
transformation. The models, transformation, source code and example
input / output models can be found
[here](https://git.eclipse.org/c/henshin/org.eclipse.emft.henshin.git/tree/plugins/org.eclipse.emf.henshin.examples/src/org/eclipse/emf/henshin/examples/ecore2genmodel).

### Generating the GenModel

The following screenshots depicts the rules and transformation units for
the generation of the GenModel.

[[/images/henshin-ecore2genmodel-rules1.png]]

### Customization (HO-transformation)

The following screenshots depicts the rules, meta-rules and
transformation units for the customization part. For details, please
take a look at the paper cited in the beginning.

[[/images/henshin-ecore2genmodel-rules2.png]]

This example requires Henshin 0.9.3 or higher.

*contributed by Enrico Biermann, Claudia Ermel, Stefan Jurack and
Christian Krause*


