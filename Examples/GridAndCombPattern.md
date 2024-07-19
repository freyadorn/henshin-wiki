[Henshin](Home "wikilink")\'s **Grid & Comb Pattern** addresses a
graph transformation benchmark from the technical report [Benchmarking
for Graph
Transformation](http://www.cs.bme.hu/~gervarro/publication/TUB-TR-05-EE17.pdf)
by Varró et al. This example consists of the following parts:
construction of a sparse and a full grid, construction of a comb pattern
using a higher-order (HO) transformation, and matching the comb pattern
in the generated sparse and full grids. The involved structures are
shown below (images taken from the TR).

[[/images/henshin-structures.png]]

The transformation and the source code can be found
[here](https://github.com/eclipse-henshin/henshin/tree/master/plugins/org.eclipse.emf.henshin.examples/src/org/eclipse/emf/henshin/examples/combpattern).
All benchmarks were run on a Intel(R) Xeon(R) CPU @ 2.50GHz with 8GB of
main memory using Henshin 0.9.2.

### Generation of a sparse grid

In this part, a sparse grid structure is constructed that consists of a
number of columns which are not interconnected. The transformation rules
and units are shown below. The main unit unit for constructing the grid
is the sequential *unitbuildGrid* which takes the two parameters *width*
and *height* as input (both integers), and returns the constructed
sparse grid using the parameter *grid*. The construction works as
follows: create width/2 many columns (each consisting of two vertical
lines of nodes), where each column is built up step-wise by invoking
startColumn once, and extendColumn height-2 times. many columns (each
consisting of two vertical lines of nodes), where each column is built
up step-wise by invoking *startColumn* once, and *extendColumn* height-2
times.

[[/images/henshin-grid-sparse-rules.png]]

The following table shows the run time of the Henshin interpreter for
different sizes of the sparse grid, where we used the same values for
the width and the height.

   Size     Nodes     Generation time 
  -------- --------- -------------------
  20       400       163ms
  40       1.600     328ms
  60       3.600     382ms
  80       6.400     532ms
  100      10.000    935ms
  120      14.400    1.723ms
  140      19.600    2.838ms
  160      25.600    4.882ms
  180      32.400    7.113ms
  200      40.000    12.118ms

### Generation of a full grid

Now our goal is to construct a full grid. The transformation rules and
units are shown below. The main unit unit for constructing the grid is
again the sequential unit*buildGrid*. The construction of the full grid
is different to the construction of the sparse grid. First, we create an
initial grid with two nodes that are vertically connected. Second, we
build up the first vertical line of nodes. Then, we step-wise add new
columns to the grid, each starting from the top and working all the way
down.

[[/images/henshin-grid-full-rules.png]]

The following table shows the run time of the Henshin interpreter for
different sizes of the full grid. The construction time is longer than
for the sparse grid (note the different sizes). The reason for this is
that in the construction of the sparse grid, the next nodes are passed
to the rules which already provide a partial match. For the full grid,
no node parameters are passed and negative application conditions are
used to ensure that the correct position in the grid is used. This more
involved matching process takes more time.

   Size     Nodes     Generation time 
  -------- --------- -------------------
  10       100       54ms
  20       400       208ms
  30       900       217ms
  40       1.600     437ms
  50       2.500     877ms
  60       3.600     1.561ms
  70       4.900     2.615ms
  80       6.400     4.162ms
  90       8.100     6.278ms
  100      10.000    9.238ms

### Generating and matching the comb pattern

Now we want to construct the comb pattern with varying width. For this
we use the transformation units and rules shown below. The comb pattern
of width 1 is directly modeled using the rule *combPattern*. The
iterated unit *buildCombPattern* extends this basic rule by invoking the
HO-rule *extendCombPattern* width-1 times. The rule *extendCombPattern*
matches and transforms the rule *combPattern* (this is the higher-order
part of the transformation). Note that the rule *extendCombPattern* is
therefore defined over the Henshin and the Ecore meta-model. If you want
to define a high-order transformation, make sure that you import the
runtime versions of both models.

[[/images/henshin-comb-rules.png]]

The following table shows the matching time for the comb pattern for the
sparse and the full grid.

**Matching in the sparse grid (no matches)**

   Grid size     Pattern width     Matching time 
  ------------- ----------------- -----------------
  200           20                49ms
  200           40                49ms
  200           60                52ms
  200           80                53ms
  200           100               55ms
  200           120               58ms
  200           140               60ms
  200           160               65ms
  200           180               68ms
  200           200               71ms

**Matching in the full grid**

   Grid size     Pattern width     Matches     Matching time 
  ------------- ----------------- ----------- -----------------
  100           10                9.009       620ms
  100           20                8.019       759ms
  100           30                7.029       538ms
  100           40                6.039       673ms
  100           50                5.049       777ms
  100           60                4.059       837ms
  100           70                3.069       863ms
  100           80                2.079       909ms
  100           90                1.089       855ms
  100           100               99          806ms

*contributed by Dmitry Zakharov and Christian Krause*


