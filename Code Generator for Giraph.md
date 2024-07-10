In addition to its [interpreter engine](Interpreter "wikilink"),
[Henshin](Home "wikilink") also supports generating Java code for
[Apache Giraph](http://giraph.apache.org/).

## Getting Started

You can find here some first steps to get started with the Giraph code
generator.

### Generating Giraph Code

To generate Giraph code, right-click in the graphical or the tree-based
editor on a rule or a unit and select *Generate Giraph Code*. Choose the
target package and check out the options. When you click on *Finish* two
classes are being generated in the same folder as the Henshin file. One
is a generic helper class that is common for all Giraph-based
transformations (it is called *HenshinUtil*). The other class contains
the code for your transformation. It contains the code for used rules
and units.

### Setting Up Giraph

The generated Giraph code has been tested with the [release
branch](https://github.com/apache/giraph) of Apache Giraph and [Hadoop
0.20.203.0](https://archive.apache.org/dist/hadoop/core/hadoop-0.20.203.0).
The easiest is to use *git pull* to get the Giraph source code, and to
download the Hadoop binaries using the above link. Then un-tar the
Hadoop archive somewhere.

Next you should build Giraph. Change into the Giraph source folder and
use *mvn clean package -DskipTests*. For this you need Maven installed.
Make sure that the build was successful.

#### Hadoop Configuration

Set the Hadoop home and Java home directories in *conf/hadoop-env.sh* in
the Hadoop directory.



