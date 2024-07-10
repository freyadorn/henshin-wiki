The **Henshin Trace model** is an EMF model which provides generic and
flexible support for traceability in Henshin. The most important use of
the Trace model are exogenous model transformations. Exogenous
transformations are used to translate one or more source models into one
or more target models. The trace model is used to keep track of the
translated elements during the transformation. This is necessary because
in general source and target models need not be related in any way.

The trace model consists of a single class *Trace* which has two
non-containment n-ary references called *source* and *target*. These two
references are of type *EObject* and therefore can be used to refer to
any EMF object. This is also the reason why we say that the trace model
is generic. In addition to the source and target references, traces can
be named and can contain subtraces.

![Henshin Trace Model](Henshin-trace-model.png "Henshin Trace Model")

## Importing the Trace Model

To use the trace model in an exogenous transformation, all you need to
do is to import the trace package into your transformation and to use it
in the transformation rules. To import the trace model, use the context
menu entry *Import Packages\... -\> From Registry* in one of the Henshin
editors and choose this URI:
<http://www.eclipse.org/emf/2011/Henshin/Trace>.

![Henshin Import Menu](Henshin-import-menu.png "Henshin Import Menu")

## Using the Trace Model

Suppose you want to translate EClasses to GenClasses and all their
EAttributes to GenFeatures. Then you could use the following to
transformation rules:

![Henshin Trace Example
Rules](Henshin-trace-example.png "Henshin Trace Example Rules")

Here, the trace element between EClass and GenClass is used to find out
in which GenClass the new GenFeature should be contained in.

Remark: for the above translation the Trace model is actually not needed
because the elements in GenModel keep references to the Ecore elements
themselves. In general, the source and target models in exogenous
transformations may not be related and therefore the Trace model is
necessary.



