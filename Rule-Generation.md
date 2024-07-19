Apart from creating Henshin [transformation rules](https://wiki.eclipse.org/Henshin/Transformation_Meta-Model#Basic_building_blocks:_Rules) from scratch, you can also generate a rule from a pair of models that demonstrates the effect of a transformation.

# Generation of transformation rules

In particular, one of the two input models shows the original state, whereas the other model demonstrates the changes made during the transformation. By use of [EMF Compare](https://wiki.eclipse.org/EMF_Compare), those two models are first compared with each other in order to identify corresponding elements, i.e., those elements which are considered to be the same in both models. Thereupon, a transformation rule is basically generated as follows: The original model is converted to a Henshin graph and used as the left-hand side (LHS) of the resulting rule, while the Henshin graph obtained from the changed model serves as the right-hand side (RHS). Next, LHS-RHS mappings are created for all node based on pairs of corresponding elements. Eventually, those mappings are used to derive which elements are either preserved, deleted or created in the final Henshin rule.

# Applying the Henshin rule generator to models

In the package explorer, select the two models that will serve as an input for the rule generation. Next, right-click on the selection and choose Henshin > Henshin Rule Generation > Create Example-based Transformation Rule.

A window will pop up that asks you to specify the left-hand side of the rule, i.e., the model that demonstrates the original state. The other model will be considered as the changed model from which the right-hand side of the rule is derived.

Thereupon, you obtain a Henshin module including the necessary meta-model imports as well as the generated transformation rule. You can view/edit/use this automatically created rule like any other Henshin rule.

[[/images/Henshin_rule_generation_example_1.png]]

_Applying Henshin Rule Generation to a pair of models_

[[/images/Henshin_rule_generation_example_2.png]]

_Selecting the LHS_

[[/images/Henshin_rule_generation_example_3.png]]

_Automatically generated rule_