[Henshin](Home "wikilink")\'s **Gossiping Girls** example shows how
to use [nested
rules](Henshin_Transformation_Meta-Model#Advanced_concepts:_Application_conditions_and_rule_nesting "wikilink")
to match subpatterns and how to generate and analyze [state
spaces](Henshin_Statespace_Explorer "wikilink").

The example is based on the *Gossiping Girls Problem*. The metamodel is
very simple: it consists of a class *Girl* and a class *Secret*. Girls
know those secrets to which they have a reference to. Two girls can
exchange their secrets by making a phone call with each other. For
example, if girl A knows the secrets 1 and 2, and girl B knows the
secrets 3 and 4, then after the phone call both girls know the secrets
1,2,3,4.

The problem to solve is as follows: Given N girls each knowing a secret
that no other girl knows. How many phone calls are necessary so that
every girl knows all the secrets?

### Modeling

This problem can be modeled nicely in Henshin. We only need one rule for
modeling phone calls, and one rule for adding girls and secrets (for
creating initial configurations). The two rules are shown below.

[[/images/henshin-gossip-rules.png]]

The rule *addGirl* creates a new girl and a new secret that only she
knows. The annotation *\@Container* says that the two created objects
should be added to the contents of an existing object of type
*Container*. By invoking this rule N times, we can create the initial
configuration for N girls.

The rule *exchangeSecrets(g1,g2)* is used to model phone calls. The
parameters *g1,g2* identify the girls which make the phone calls. If
these parameters are not set, Henshin nondeterministically selects two
arbitrary girls. The rule has one nested rules to describe the *for-all*
semantics for exchanging secrets. That means, for girl *g1* all of her
secrets which girl *g2* does not know will be matched (ensured using the
forbid\* edge). Analogously for girl *g2*. The application of the rule
then adds between every pair of girl and secrets (that she doesn\'t
know) a new edge between them. Thus, after applying this rule, both
girls know all secrets (their owns and the ones from the other girl).

### Evaluation

To solve the gossiping girls problem, we generate the state space with
Henshin and search for the shortest path leading to a state where all
girls know all secrets. The state space generation can be done in the
[State Space
Explorer](http://wiki.eclipse.org/Henshin_Statespace_Explorer) or in
Java as done in the
[GossipingGirls](http://git.eclipse.org/c/henshin/org.eclipse.emft.henshin.git/tree/plugins/org.eclipse.emf.henshin.henshin-org/eclipse/emf/henshin/henshin-GossipingGirls.java)
class. The figure below shows the state space for 4 girls generated in
the state space explorer.

[[/images/henshin-gossip-statespace-4.png]]

The green state is the initial state. State 12 is the target state in
which all girls know all secrets. We use the following OCL constraint to
identify the target states:

    girls-&gt;forAll(g : Girl | g.secrets-&gt;size()=girls-&gt;size())

\
which basically says that for all girls it holds that the number of
secrets that she knows is equal to the number of girls there exists. To
determine the minimum number of phone calls required, select the
validation tool *OCL (shortest path)* and enter the above constraint in
the text field and hit the run button. You should see now the shortest
path to the goal state. Note that in the state space, in every state a
self loop transition is possible. This models the situation where two
girls call each other which already know all secrets of each other.

The table below shows the number of states and transitions and the
minimum number of phone calls as computed by Henshin. The benchmark was
conducted on a Intel(R) Xeon(R) CPU @ 2.50GHz with 8GB of main memory.
Note that it is important to set the state space property
*ignoreDuplicateTransitions* to true here to be able to get to level 8.

    Girls       States       Transitions       Calls       Generation Time       Checking Time  
  ----------- ------------ ----------------- ----------- --------------------- -------------------
  2           2            2                 1           59ms                  3ms
  3           4            6                 3           88ms                  4ms
  4           13           31                4           221ms                 5ms
  5           52           198               6           733ms                 12ms
  6           357          2,092             8           2,891ms               66ms
  7           3,689        33,392            10          12,545ms              721ms
  8           62,857       827,963           12          510s                  430s

\
\
This example requires Henshin 0.9.3 or higher.

*contributed by Christian Krause*


