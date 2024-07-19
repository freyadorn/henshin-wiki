The **Movies example** showcases Henshin\'s [Code Generator for
Giraph](Code_Generator_for_Giraph "wikilink"), which enables
support for massive parallel rule execution. The example stems from the
paper *Implementing Graph Transformations in the Bulk Synchronous
Parallel Model* published in FASE 2014.

The original example consists of a transformation on a part of the data
found in the IMDB movie database. We use here the simplified metamodel
shown below.

[[/images/henshin-movies.png]]

The goal is to find all persons (actors or actresses) that starred
together in at least three movies. For every such pair we want to create
an instance of the Couple class and add links to the movies the couple
played in. Note that this is a simplified version with the following
limitations: 1) due to symmetry we actually create 2 couple object for
every pair of persons, and 2) we do not consider attributes, such as
actor / actress names or movie ratings. For this simplified task, we use
the rules and the sequential unit shown below.

[[/images/henshin-couples.png]]

To generate the Giraph code, right-click on the sequential unit and
select *Generate Giraph Code*. This will generate two classes: one
general utility class and one class for this particular transformation.
You can find the generated classes in the
[giraph/movies](https://github.com/eclipse-henshin/henshin/tree/master/plugins/org.eclipse.emf.henshin.examples/src/org/eclipse/emf/henshin/examples/movies)
folder in the *examples* plugin.

*contributed by Christian Krause and Matthias Tichy*


