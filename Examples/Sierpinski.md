![](henshin-sierpinski.gif "henshin-sierpinski.gif"){width="140"}

The **Sierpinski Triangle example** is a very simple example which we
use to do some benchmarking for [Henshin](Henshin "wikilink")\'s
[interpreter](Henshin/Interpreter "wikilink"). The [Sierpinski
triangle](http://en.wikipedia.org/wiki/Sierpinski_triangle) is a fractal
which is constructed by iteratively dividing triangles into
sub-triangles. The number of nodes in the triangle grows exponentially
with the number of iterations.

The transformation consists of a single rule, which divides and adds new
triangles. The screenshot below shows this rule in the graphical editor.

![](henshin-addtriangle2.png "henshin-addtriangle2.png"){width="400"}

The transformation files and the Java source code are available in the
examples plug-in in the
[sierpinski](http://git.eclipse.org/c/henshin/org.eclipse.emft.henshin.git/tree/plugins/org.eclipse.emf.henshin.examples/src/org/eclipse/emf/henshin/examples/sierpinski/)
package.

The following benchmark was conducted on a Intel(R) Xeon(R) CPU @
2.50GHz with 8GB of main memory using Henshin 0.9.2. The parameters for
the benchmark were automatically set using a script. All times are in
milliseconds.

    Level       Rule applications       Nodes        Matching Time       Application Time       Total Time  
  ----------- ----------------------- ------------ ------------------- ---------------------- ----------------
  1           1                       6            0ms                 1ms                    1ms
  2           3                       15           1ms                 1ms                    2ms
  3           9                       42           1ms                 3ms                    4ms
  4           27                      123          4ms                 8ms                    12ms
  5           81                      366          13ms                27ms                   40ms
  6           243                     1,095        37ms                67ms                   104ms
  7           729                     3,282        73ms                158ms                  231ms
  8           2,187                   9,843        125ms               195ms                  320ms
  9           6,561                   29,526       107ms               231ms                  338ms
  10          19,683                  88,575       185ms               219ms                  404ms
  11          59,049                  265,722      441ms               723ms                  1,164ms
  12          177,147                 797,163      1,309ms             2,500ms                3,809ms
  13          531,441                 2,391,486    3,627ms             9,944ms                13,571ms
  14          1,594,323               7,174,455    14,609ms            117,470ms              132,079ms

\
*Contributed by Enrico Biermann and Christian Krause*
[category:Henshin](category:Henshin "wikilink")
