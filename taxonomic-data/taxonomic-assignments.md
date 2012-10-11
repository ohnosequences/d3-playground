
# taxonomic assignments

We want to visualize functions out of a tree, representing a taxonomy. The standard example is:

## reads -> taxonomy assignments

We have a set of Reads R, and each read is assigned to a node in a taxonomy, usually the NCBI taxonomy. It is important to note that

> not all reads are assigned to leaves!

In fact, a lot of them are usually assigned to a non-leaf node, due to lack of "resolution" of the data (reads are short, have errors, protein families, etc).

What's the point of this, from the biological standpoint? It really depends on the particularities of the experiment/data, of course, but there are some general features which can be abstracted from this.

### counting and frequencies

we have a set of reads, and a map

$$
a\colon R \to T
$$

assigning reads to nodes in a tree. There are a lot of "frequencies" and read counts we can define here. It is good to first look at ways of counting, then generate frequencies.

#### some things that we can count

##### node count

Each node $t \in T$ has an associated fiber $a^{-1}(t)$, and we can look at the size of this $# a^{-1}(t)$.

##### descendants count

Each node $t \in T$ determines a subtree $D_t$ of $T$ (that of its descendants). We can take the union of the fibers across this tree, and count this $# a^{-1}(D_t)$. Let's call this $R(D_t)$ (the set of reads assigned to $D_t$).

##### complementary count

The dual of descendants: count reads assigned to those that are _not_ descendants of $t$.

#### ratios, frequencies

##### relative counts

For me, the most interesting thing we could do here is looking at the ratios of two related descendant counts: take $p \in T$, and $c \in D_p$. Then $R(D_p) \geq R(D_c)$, and we have a ratio

$$
R(D_{c/p}) = R(D_c) / R(D_p) \leq 1
$$

If this is $1$, then everything assigned to a descendant of $p$ is actually assigned to a descendant of $c$.

There are some particular choices of $c$ and $p$ here worth mentioning:

1. $p$ the root node. Then we're just looking at the ratio of reads assigned to a descendant of $c$ with respect to all _assigned_ reads
2. $c$ a leaf node. In this case, what we get is the ratio of reads assigned to this node with respect to all assigned reads

### data

Almost all of the representations we want for this will use hierarchical layouts. There's an _abstract_ Hierarchy Layout in D3, see [D3 API - Hierarchy Layout](https://github.com/mbostock/d3/wiki/Hierarchy-Layout), and a lot of the data wrangling and coding can be done at this level. For this, we need a JSON representation of our data, along the lines of

``` json
{
 "name": "Bacteria",
 "children": [
  {
   "name": "Cyanobacteria",
   "children": [
    {
     "name": "Chroococcales",
     "children": [
      {"name": "Acaryochloris", "count": 3938},
      {"name": "Aphanocapsa", "count": 3812},
      {"name": "Aphanotece", "count": 743}
     ]
    },
    {
     "name": "Gloeobacteria",
     "children": [
      {"name": "Gloeobacterales", "count": 3534},
      {"name": "enviromental samples", "count": 5731}
     ]
    }
   ]
  }
 ]
}
```

Note that each node could have as many attrs as we want, like descendant count, frequencies, etc.

#### leaf-based values!!

As currently implemented, the Hierarchy Layout does **not** allow for values at non-leaf nodes. The value of each node is just the sum? of its descendant leaves. See for example

- [D3 pull request #574 - d3.layout.hierarchy: Allow internal nodes to have value](https://github.com/mbostock/d3/pull/574)
- [stackoverflow :: d3.js - Treemap wehere parent's value is greater than sum of its children](http://stackoverflow.com/questions/11253740/d3-js-treemap-where-parents-value-is-greater-than-sum-of-its-children)
- working example! -> [Recursive Treemap With Parent Weighted Nodes Using d3.js](http://devforrest.com/examples/treemap/treemap.php)

All of this could be needed for other hierarchical layouts.

### visualizations

#### treemaps

Need to think more about this. 

#### sunbursts







