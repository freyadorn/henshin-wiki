**OCL2AC** is a tool for transforming OCL constraints into application
conditions of rules, leading to constraint-guaranteeing rules. Such
rules are only applicable to models that satisfy the constraint at hand,
and guarantee that it will still be satisfied after the rule has been
applied.

OCL2AC comprises two main components:

-   *OCL2GC* translates OCL constraints into a set of semantically
    equivalent (nested) graph constraints.
-   *GC2AC* integrates graph constraints as (guaranteeing) application
    conditions into Henshin transformation rules.

### Installation

**Prerequisites**: We assume an Eclipse with EMF and OCL SDK installed.
Note that OCL SDK, unlike EMF, is not part of standard distributions
like the Eclipse Modeling Tools. To install OCL SDK, use the default
update site of your Eclipse (e.g., in Eclipse Neon, the default update
site is <http://download.eclipse.org/releases/neon/> )

**How to install**: OCL2AC is available from Henshin releases from 1.8.0
onward and via the nightly build (see [Installation instructions](Installation_instructions "wikilink"); upon installation
from update site, search for the \"OCL2AC\" feature, and make sure to
also install the \"Henshin SDK\" feature if you don\'t have it).

### Usage

Please refer to the external usage instructions at
[ocl2ac.github.io](https://ocl2ac.github.io/home/#started).

### Credit

Lead developer of OC2AC: Nebras Nassar


