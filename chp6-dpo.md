---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Julia 1.10
  language: julia
  name: julia-1.10
---

# Chapter 6: Double pushout graphs

## 6.1. Introduction

"Find-and-replace" feature of text editors is one of the most powerful innovations of the 20th century. Inspite of unavailability of statistics, the advantage of this feature is tangible and undeniable! Beyond text editors, the concept of "find and replace" has also caused much chaos in the world! When the European conquerors "found" native Americans settlements in Canada, they decided to "replace" the native culture by sending an entire generation of native American children to special missionary schools. This has resulted in trauma and chaos that continues well into the current times. Or a country waging war over another country to replace the exisiting government for political reasons! Or Large-scale replacement of forests by industries and settlements has adversely impacting global climate.

> We know in hindsight this approach has lead humanity to face issues bigger than they the ones it intended to solve! So, what was missed in the decision making process? 

Let us consider a fun and non-political example -- the struggle of qutting sugar. Here is a simple explanation of a why quitting sugar is so hard! A usual thought-process behind attempting to quit sugar is: 

```{image} assets/Ch6/quit-sugar-1.png
:alt: Whoopsy!
:width: 550px
:align: center
```

But the reality is: 

```{image} assets/Ch6/quit-sugar-2.png
:alt: Whoopsy!
:width: 350px
:align: center
```

because, the connections are broken causing the system to experience instability: 

```{image} assets/Ch6/quit-sugar-3.png
:alt: Whoopsy!
:width: 350px
:align: center
```

While being healthy makes a person happy, eating sugar makes a person happy in a different way. Hence, removing sugar results in a broken connection and an unstable structure! The result is usually to restore stability by brining (more) sugar back! Even though this example is oversimplistic, it drives home the message connections playing an important role in driving our lives and why careful considerations of connections is necessary to make a positive change. 

If we want to make a change, it is recommended to start with something simple. The simple thing we shall do in this chapter is to build a way to make changes to an existing graph in a "good" way. By good, we mean that we do not end up with broken connections, and make precisely the changes that are intended. 

Making changes to a graph means adding new vertices/edges or removing existing vertices/edges or both. And these changes happen over particular region(s) of the graph (like a textual find and replace).

A lot of concepts in the world around us are modelled as graphs. Hence, building a way to changing an existing graph that is guaranteed to be good is much more than just a theoretical exercise.

Let us begin! 

Given a pattern (a graph), and its replacement, our mission is to "find" the pattern in any host graph and "replace" it in a good way -- 
first by hand and then in computers! [^1]. 

## 6.2. Specs for "find and replace"

Suppose we want to find a text and replace it by some other text in a document. Microsoft Word provides an interface as in the picture belowfor this purpose.  

```{image} assets/Ch6/find-and-replace.png
:alt: Whoopsy!
:width: 450px
:align: center
```

:::{admonition} Pause and ponder

How would the interface for find-and-replace in graphs look like? 

:::

To begin with, let us think what would be filled in the "Find what:" and "Replace with:" boxes. 

```Find what``` box must take in a graph. This is the search pattern that will be matched in a host graph. ```Replace with``` box should also take in a graph. This is the replace pattern that will replace the match. 

Great, we are already half-way through! Next, we must how ```Find what``` and ```Replace with``` relate! In text documents, there is only one way of replacing a text by another. But in graphs, there are multiple ways of replacing one graph by another unless specified. Our goal is to find a match of `Find what` in the host graph and to replace it with `Replace with`. 

Consider the following example:


```{image} assets/Ch6/find-and-replace-example.png
:alt: Whoopsy!
:width: 375px
:align: center
```

</br>

There is a match in the host graph which is the vertex with the self-loop. However, there are at least two ways to replace the match with `Replace with`, as shown below.  

```{image} assets/Ch6/replacements.png
:alt: Whoopsy!
:width: 475px
:align: center
```

Replacement-1 replaces the search pattern (`Find what`) by matching it with the middle vertices of the replace pattern (`Replace with`). 

```{image} assets/Ch6/replacement-1.png
:alt: Whoopsy!
:width: 475px
:align: center
```
</br>


Replacement-2 replaces the search pattern by deleting the self loop and by matching the remaining vertex with one of the end nodes of the replace pattern.

```{image} assets/Ch6/replacement-2.png
:alt: Whoopsy!
:width: 475px
:align: center
```

</br>

To pick one of these replacements, the find-and-replace must specify where the search pattern (`Find what`) and the replace patterns (`Replace with`) meet or overlap. 

We know from Chapter 5, how to specify overlap between two graphs :) Do you remember the diagram with three graphs and two radiating arrows?

```{image} assets/Ch6/overlap.png
:alt: Whoopsy!
:width: 300px
:align: center
```

The only requirement in this case will be that the arrows needs to be **injective morphisms** (each vertex of the search pattern must overlap with a unique vertex of the replacement pattern. Similary for the edges). 

:::{admonition} Pause and ponder

How does the search and the replace patterns overlap in the two different cases of replacement shown above?

:::

The solutions are as follows!

For replacement-1, 

```{image} assets/Ch6/interface-1.png
:alt: Whoopsy!
:width: 575px
:align: center
```

 and replacement-2:

```{image} assets/Ch6/interface-2.png
:alt: Whoopsy!
:width: 625px
:align: center
```

</br>


Note that, the vertices and the edges of the search pattern outside the overlap region are removed from the host. For example, in case 2, the self-loop of the search pattern is outside the overlap. So it has been removed during the replacement procedure. What overlaps in the search pattern is alone retained. So, the embedding from `overlap` to `Find what` is the **specification for deletion**. 


```{image} assets/Ch6/interfaceExample.png
:alt: Whoopsy!
:width: 625px
:align: center
```

The vertices and the edges of the replacement pattern outside the overlap region are added to the host grpah. In case 2, these are unlabelled yellow color nodes in `Replace with` are outside the overlap region. They have been added to the host graph during the replacement. So, the embedding from `overlap` to `Replace with` is the **specification for addition**.

The overlap will be unchanged. It is precisely what remains after removing specified edges/vertices from the search pattern.

```{image} assets/Ch6/specification.png
:alt: Whoopsy!
:width: 625px
:align: center
```

Neat, huh?!

:::{admonition} Puzzle 1

````{div} wrapper 

Find the overlap between search and replace for the following replacement:


... 

:::{admonition} Solution 
:class: dropdown
````{div} wrapper 
IMAGE
````

:::

:::{admonition} Puzzle 2

````{div} wrapper 

Apply the following find and replace to the given host graph at the highlighted match:

...



:::{admonition} Solution 
:class: dropdown
````{div} wrapper 
IMAGE
````

:::

:::{admonition} Key points
:class: tip

The interface to find-and-replace needs to have:

```{image} assets/Ch6/interface.png
:alt: Whoopsy!
:width: 450px
:align: center
```

- The injective morphism into `Find what` is the **specification for deletion**. 

- The injective morphism into `Replace With` is the **specification for addition**.

:::

## 6.3. Finding a match

The next step is to answer how to find a match of a search pattern inside a host graph, similar to finding some text in a document. When a text editor receives an input like this, it find (exact) matches of the string of characters "Happy Priyaa". 

```{image} assets/Ch6/find-and-replace-text.png
:alt: Whoopsy!
:width: 250px
:align: center
```

However, in graph, connectivity matters than finding exact match of the shape of the search pattern. So when searching for pattern in a host graph, we do not look for 1-to-1 correspondence between vertices / edges of the `Find what`. Rather, we need to look for matches which have similar connectivity as the search pattern. Do the words "similar connectivity" a ring bell? Similar connectivity implies  graph morphism.

<mark>A match is a graph morphism from `Find what` to a host.</mark>

Let us suppose we want to find a match of this search pattern in the host graph:

```{image} assets/Ch6/match.png
:alt: Whoopsy!
:width: 350px
:align: center
```

</br>


Here are two possible matches of the search pattern. The mapping (match) is shown using the labels on the vertices and the edges. 


The below match is an injective morphism. 

```{image} assets/Ch6/match-example-1.png
:alt: Whoopsy!
:width: 350px
:align: center
```

</br>

The below match, has vertices "1" and "2" in the search pattern, mapped to the same vertex in the host graph.

```{image} assets/Ch6/match-example-2.png
:alt: Whoopsy!
:width: 350px
:align: center
```

</br>



:::::{admonition} Let us strengthen our understanding!
:class: note

::::{tab-set}

:::{tab-item} Puzzle 3
Find at least two matches of the search pattern in the host graph. 

````{div} wrapper 
```{image} assets/Ch6/match-ex1.png
:alt: Whoopsy!
:width: 350px
:align: center
```
````


:::{admonition} Solution 
:class: dropdown

````{div} wrapper 

An exact match: 

```{image} assets/Ch6/match-ex-sol1.png
:alt: Whoopsy!
:width: 350px
:align: center
```

Non-injective match:

```{image} assets/Ch6/match-ex-sol2.png
:alt: Whoopsy!
:width: 350px
:align: center
```

````

:::

:::{tab-item} Puzzle 4

(Same as Paul's Puzzle from chapter 1)

How many matches (graph morphism) are from the ```Find what``` to the host graph? 

```{div} wrapper 
IMAGE
```


:::{admonition} Solution 
:class: dropdown
```{div} wrapper 
IMAGE
```

:::

::::

:::::

:::{admonition} Key points
:class: tip

A *match* in a host graph is given by a morphism from ```Find what``` to a host graph! 

:::

## 6.4. Removing vertices and edges from a graph

We now have the specification for "find-and-replace" and the specification for a match. What remains is to perform the find-and-replace! We will begin with the deletion specification!

$$ {\sf overlap} \hookrightarrow {\sf Find~what} $$


### 6.4.1. Finding a match to remove vertices/edges

[Message: Tell what a good match is!]

Erasing edges and vertices require care. Suppose a vertex in a match is specifed to be removed. There may be other vertices in the host graph which are connected to the vertex to be deleted. In that case, deleting the vertex will lead to zombie graphs which has edges not connected to any vertex. 

The following example illustrates this situation. 

Suppose, we have the following specification for deletion: vertex 2 and edge '2-3' will be deleted since they are not under the overlap region! 
```{image} assets/Ch6/remove-7.png
:alt: Whoopsy!
:width: 650px
:align: center
```
</br>

Can you see why the following match is bad even though it is a graph morphism? 

```{image} assets/Ch6/bad-match-1.png
:alt: Whoopsy!
:width: 650px
:align: center
```
</br>

Removing vertex 2 from the host graph will result in a dangling edge, see the diagram picture below!

```{image} assets/Ch6/dangling-3.png
:alt: Whoopsy!
:width: 250px
:align: center
```

An appropriate match will be:

```{image} assets/Ch6/remove-8.png
:alt: Whoopsy!
:width: 550px
:align: center
```

</br>

Another situation that needs care is that a match must not identify a vertex to be deleted and a vertex that needs to be retained (not to be deleted) in the search pattern to the same vertex in the host graph. By the deletion specification, vertex 1 must remain and vertex 2 needs to be deleted. However, the following match identifies vertices 1 and 2 of the search pattern to the same vertex ("1,2") in the host graph.

```{image} assets/Ch6/identification-1.png
:alt: Whoopsy!
:width: 550px
:align: center
```
</br>

So what should be done now -- keep "1,2" or delete "1,2" from the host graph? A good match, in the first place, MUST avoid identifying 1 and 2 into the same bin since either choice is incorrect. 

:::{admonition}  Key points
:class: tip

 A **good match** of a search pattern (for deletion of vertices and edges) will: 
-  satisfy the *no-dangling edge condition*.
-  identify vertices / edges to be deleted and vertices / edges to remain seperately, called the *right identification condition*.

:::

We will talk more about "finding a good match" once the algorithm to perform the deletion specification is set up.


### 6.4.2. Performing deletion

[Message: Tell deletion is computing pushout compelements]

We are given a deletion specification, a host graph and a match for the search pattern in the host graph. As before, let us begin by making a diagram of the information we have.

```{image} assets/Ch6/remove-1.png
:alt: Whoopsy!
:width: 650px
:align: center
```

</br>

The new graph which results after performing the replacement will have: 
1. All the vertices and edges of the host graph which are outside the match, and
2. Only those vertices and edges of the match which is also included in the overlap.
   
This means that `overlap` will have a match in the new graph **determined by the match found in the host graph**. The new graph will have an injective morphism into the host graph.

</br>

```{image} assets/Ch6/remove-2.png
:alt: Whoopsy!
:width: 750px
:align: center
```
</br>

The above square commutes! Does this shape remind you of pushouts from chapter 5? It turns out that, the host graph is the pushout of `Find what` and the new graph!!

This indeed makes sense, because, the search pattern includes vertices and edges which has been removed. Glueing together `Find what` and the and the new graph along the overlap msut give the original host graph!

Since the new graph completes a pushout square we shall call it as the **Pushout complement**. 

:::{Note} 

Complement means *the thing that completes or brings to perfection*. 

:::

> A pushout complement is a pushout completement! 

Let us look at an example of computing "pushout **complement**" 

:::{admonition} Example 1

```{image} assets/Ch6/example-1.png
:alt: Whoopsy!
:width: 750px
:align: center
```

The nodes exclusive to `Find what` must be removed from the host graph. Thus, the pushout out complement must include: 
1.  all the parts of the host graph not under the match, and 
2.  only those parts of the match which is included in the overlap. 

In this example, the pushout complement includes all the edges and vertices in the host graph that is not under the match (all unlabelled vertices and edges),  and includes those vertices and edges in the match which are under the image of `overlap` (vertices 1 and 2).

:::

:::{admonition} Puzzle 5

What is the pushout complement?


This problem has a coarse-grain match. Vertices "1" and "2" of `Find what` are sent to the same vertex in the host graph. Simiarly vertices "3" and "4" are sent to the same vertex in the host graph.

```{image} assets/Ch6/Ex-1.png
:alt: Whoopsy!
:width: 750px
:align: center
```


:::{admonition} Solution
:class: dropdown

````{div} wrapper 
```{image} assets/Ch6/Ex1-sol.png
:alt: Whoopsy!
:width: 650px
:align: center
```
````

:::

:::{admonition} Puzzle 6

What is the pushout complement?


```{image} assets/Ch6/Ex-2.png
:alt: Whoopsy!
:width: 750px
:align: center
```


:::{admonition} Solution
:class: dropdown

The pushout complement includes all the edges and vertices in the host graph that is not under the match (all unlabelled vertices and edges),and includes those vertices and edges in the match which are in Graph-2.

````{div} wrapper 
```{image} assets/Ch6/Ex2-sol.png
:alt: Whoopsy!
:width: 650px
:align: center
```
````

:::

### 6.4.3. Managing the requirements of a match

In programming, there is a concept called "edge case". These are special situations where a code will fail to produce appropriate results. For example, a bug that occurs in only iPhone. They are called "edge cases" because these situations lie outside the normal flow of a code / algorithm and custom extra code is added for their special handling. There is an anecdote, "Edge cases are impossible to avoid, so keep them simple". 

:::{admonition} Million dollar questions:

1. What will happen when a match that violates the "no-dangling edge" requirement is supplied for pushout-completement? 

2. What will happen when a match that violates the "right identification" requirement is supplied for pushout-completement? 

:::

If the pushout-completement produces bad results, then these violations will be edge cases which needs to be handled outside the completement procedure -- like an algorithm to check if the match is appropriate. 

The good news is that the *universal nature* of pushouts guarantee us that pushout-complement will exist ONLY for appropraite matches!! That is, replacement will be done only when the match is appropriate. Hence, there are no edge cases!!! To drive home home this message, here are two examples. We leave it to the reader to check if a pushout complement exists in these cases. 

:::{admonition} Example 2

(Will be redrawn)

Is there a pushout complement? If not, why?


````{div} wrapper 
```{image} assets/Ch6/dangling.png
:alt: Whoopsy!
:width: 350px
:align: center
```
````


:::



:::{admonition} Example 3

Is there a pushout complement? If not, why?


````{div} wrapper 
```{image} assets/Ch6/right-identity.png
:alt: Whoopsy!
:width: 350px
:align: center
```
````

:::



We did all the heavy lifting early-on by relying on relational thinking, considering all the relevant relationships, and getting the *plumbing* right! The pay off is that things flow smoothly without the need for extra intervention! 

> The procedure is the guard rail! 

A final question we must ask is that are pushout complements unique? The pushout completement graph computed for a deletion rule is unique because the deletion rule is an injective morphism.

:::{admonition} Key point
:class: tip

Vertices and edges of a graph is removed by computing pushout complement.

::: 

## 6.5. Adding vertices and edges to a graph

We applied the deletion rule to remove the vertices and edges in the host graph as specified by the rule. We called the resulting graph a pushout complement.  We are now ready to add vertices and edges  specified by the addition rule to the pushout complement. That will complete our replacement procedure! Hurray! 

As always, let us begin by drawing a diagram of the gre relationships we got! The advantage of drawing digrams is that it arranges information in an intuitive way that it makes it easier to "see" the solution! 

```{image} assets/Ch6/add-1.png
:alt: Whoopsy!
:width: 550px
:align: center
```
</br>

We shall focus on the lower half of the diagram now where addition of vertices and edges shall proceed!

```{image} assets/Ch6/add-2.png
:alt: Whoopsy!
:width: 550px
:align: center
```

</br>

We now want to compute a new graph by adding the vertices and edges exclusive to `Replace with` to the host graph at the match! Do you see how?! Does the shape of the diagram ringbells?

::: {admonition} Yes, the answer is
:class: dropdown

Pushouts!!

:::

By computing the pushout of the above diagram, we glue vertices / edges to be added to the pushout complement along the overlap. Our completed diagram looks as follows now:

```{image} assets/Ch6/pushout.png
:alt: Whoopsy!
:width: 550px
:align: center
```

Let us try out some examples to make sure we are right! We start from the example we saw in the previous section! 

**Example:** Adding a single edge

Suppose, we have an addition rule like this, and the match given by the pushout complement step:

```{image} assets/Ch6/add-example-rule.png
:alt: Whoopsy!
:width: 550px
:align: center
```

</br>

The `Replace with` has an edge between vertices 1 and 2. Since this edge is exclusive to `Replace with` (`overlap` does not have this edge between 1 and 2), this edge will be added to the host graph. Computing the pushout, precisely does this to the pushout complement!

```{image} assets/Ch6/add-example-1.png
:alt: Whoopsy!
:width: 550px
:align: center
```

The complete picture of the replacement procedure (deletion and addition) for this example is as follows: 

We have the following replacement rule and a match in the host graph. 

```{image} assets/Ch6/complete-example-rules.png
:alt: Whoopsy!
:width: 550px
:align: center
```
</br>

Computing the pushout complement followed by the pushout completes the replacement procedure!

```{image} assets/Ch6/complete-example.png
:alt: Whoopsy!
:width: 550px
:align: center
```

<!----------------------------->

:::{admonition} Puzzle 7 (continued from Puzzle 5 in Section --- )

Compute the pushout:

```{image} assets/Ch6/DPO-ex-1.png
:alt: Whoopsy!
:width: 550px
:align: center
```

:::{admonition} Solution
:class: dropdown

[Explanation]

````{div} wrapper 
```{image} assets/Ch6/DPO-ex1-sol.png
:alt: Whoopsy!
:width: 650px
:align: center
```
````

:::

:::{admonition} Puzzle 8 (continued from Puzzle 6 in Section --- )

```{image} assets/Ch6/DPO-ex-2.png
:alt: Whoopsy!
:width: 550px
:align: center
```

:::{admonition} Solution
:class: dropdown

[Explanation]

````{div} wrapper 
```{image} assets/Ch6/DPO-ex2-sol.png
:alt: Whoopsy!
:width: 650px
:align: center
```

:::

:::{admonition} Key point 1
:class: tip

Vertices and edges are added by computing pushout.

::: 

:::{admonition} Key point 2: Find-and-replace machinery for graphs
:class: tip
:name: Find-and-replace-machinery

A thing of beauty!!

A search pattern (`Find what`) found in a graph is carved into another pattern (`Replace with`) by computing pushout complement and followed by pushout.

```{image} assets/Ch6/final-picture.png
:alt: Whoopsy!
:width: 650px
:align: center
```

::: 

Common literature calls our "Find-and-replace machinery" for graphs as Double Pushout Rewriting (DPO). 

## 6.6. Find-and-replace in chemistry

In chemistry, **click reactions** give high yield with low requirements and form only harmless by-products. A specific click reaction called the Diels-Alder's reaction is one of the most useful ones in material design. Discovered in 1928, Otto Diels and Kurt Alder were awareded Nobel prize for their fascinating discovery. 

</br>

```{image} assets/Ch6/reaction.png
:alt: Whoopsy!
:width: 250px
:align: center
```

</br>

In this section, we will see that Diel-Alder's reaction is a double pushout rewrite - the  "Find and replace" machinery which uses pushouts. In this example,  we will view molecules as undirected graphs where atoms are vertices and the chemical bonds are edges. Diel-Alder's reaction is the following, "Find-and-replace" rule for a specific molecule (we do not worry about the names of these molecules because that is not the point).

```{image} assets/Ch6/Adler-1.png
:alt: Whoopsy!
:width: 350px
:align: center
```

</br>

Standard chemistry books write this reaction as follows:

```{image} assets/Ch6/Diel-Alder.png
:alt: Whoopsy!
:width: 350px
:align: center
```

Let us now use this rule on a substrate molecule which will act as the host graph.

```{image} assets/Ch6/Adler-substrate.png
:alt: Whoopsy!
:width: 650px
:align: center
```

</br>

Applying the double-pushout find-and-replace, we precisely get the final product of this chemical reaction!!

```{image} assets/Ch6/Adler.png
:alt: Whoopsy!
:width: 650px
:align: center
```

## 6.7. Find-and-replace in game design

Bob is an expert in designing game layout. He designs environment for games. He uses a layout editor for designing cool layouts. The layout keeps track of Bob's design as an directed graph components of the environment are vertices. Two vertices are connected by an arrow if the player can navigate between these two components in the game in the direction of the arrow. 

Bob is currently designing a game of Dungeons and Dragons, and have added two rooms to his dungeon -- Room 1 and Room 2, and made Room2 directly accessible from Room 1. 

However, Bob thought for a moment, and told to himself, "Hey, how about a corridor in-between?". 

```{image} assets/Ch6/corridor.png
:alt: Whoopsy!
:width: 350px
:align: center
```

</br>

Now a corridor needs to be added between Room 1 and Room 2, and arrows need to be redrawn as follows!

```{image} assets/Ch6/game-design-2.png
:alt: Whoopsy!
:width: 650px
:align: center
```

</br>

Reshaping directed graph follows the same idea as undirected graphs. The find-and-replace rule shall be applied to reshape Bob's game layoout!

## 6.8. Algebraic Julia for find-and-replace in graphs

### 6.8.1. Finding matches of a search pattern

+++

```{code-cell}
# Puzzle 3
#-----------

using Catlab

pattern = path_graph(SymmetricGraph, 3)
host = cycle_graph(SymmetricGraph, 3)

# There are 12 matches because the path can start 
# at any of the three vertices of the cycle. 
# There are two directions each can go. And for each we decide
# for both edges whether they go clockwise or not, 
# so that is 3 x 2 x 2 independent choices.

matches = homomorphisms(pattern, host)

```

+++

### 6.8.2. Is this a pushout complement?

```{code-cell}
# Example 2
#-----------

using Catlab
using AlgebraicRewriting.CSets
K = SymmetricGraph(1)
L = path_graph(SymmetricGraph, 2)
G = path_graph(SymmetricGraph, 3)

# There is only one homomorphism (up to symmetry)
# So we can pick an arbitrary one
p = homomorphism(K, L)
m = homomorphism(L, G)

# We can check whether or not the pushout complement exists
can_pushout_complement(p, m)

# We can get a list of the specific violations
gluing_conditions(ComposablePair(p, m))

```

```{code-cell}

# Example 3
#----------------------------------------
# (K, L, p: K->L are all the same)
#----------------------------------------

using Catlab
using AlgebraicRewriting.CSets

G = @acset SymmetricGraph begin V=1; E=2; src=[1,1]; tgt=[1,1]; inv=[2,1] end
m = homomorphism(L, G)

# We can check whether or not the pushout complement exists
can_pushout_complement(p, m)

# We can get a list of the specific violations
gluing_conditions(ComposablePair(p, m))

```

### 6.8.3. Computing Pushout complements

```{code-cell}
# Puzzle 5
# --------

using Catlab
using AlgebraicRewriting.CSets

Overlap, Pattern₅, Host₅ = SymmetricGraph.([2, 4, 6])
O_P₅ = ACSetTransformation(Overlap, Pattern₅; V=[1,2])
P_H₅ = ACSetTransformation(Pattern₅, Host₅; V=[1,1,2,2])
O_PC₅, PC_H₅ = pushout_complement(O_P₅, P_H₅)

to_graphviz(dom(PC_H₅))


```

```{code-cell}
# Puzzle 6
# --------

using Catlab

Pattern₆ = SymmetricGraph(3)
add_edge!(Pattern₆, 2, 3)

Host₆ = path_graph(SymmetricGraph, 6)

O_P₆ = ACSetTransformation(Overlap, Pattern₆; V=[1,3])
P_H₆ = homomorphism(Pattern₆, Host₆; initial=(V=[5,1,2],))

O_PC₆, PC_H₆ = pushout_complement(O_P₆, P_H₆)

to_graphviz(dom(PC_H₆))

```

### 6.8.4. Computing double-pushouts


```{code-cell}

# Puzzle 7
#---------

using Catlab

# Monic=true enforces that the two vertices in Overlap are not mapped to a
# single vertex in the single-edge graph.
add = homomorphism(Overlap, path_graph(SymmetricGraph, 2); monic=true)

fromR, fromPC = pushout(O_PC₅, add)
to_graphviz(codom(fromR))

```

```{code-cell}

# Puzzle 8
#---------

using Catlab

fromR, fromPC = pushout(O_PC₆, add)
to_graphviz(codom(fromR))


```

## 6.9 Goodness of relational thinking 

### 6.9.1. Relational thinking shifts vocabularly

We would like to bring reader's attention to the some vocabulary common in practice since language influences thinking and vice versa.

The idea of "Find and replace" is a crude one in the sense it is unaware of the context of replacement. A text editor will allow me to replace "truth" by "lie" anywhere in the document. Context-unaware find and replace leads to chaos.

The mathematics we used to "find-and-replace" in graphs tells us that it is not a replacement, rather it is integration of a new pattern in the place of an exisiting pattern. To integrate is organic and harmonoius by nature! While to replace has an air of superiority about it! Replace is useful -- replacing inflated tyres, replacing broken chain, etc.! However, in situations involving multiple interconnected entities, relational thinking recommends a shift in vocabulary from "Find-and-replace" to "Find-and-integrate". 

:::{admonition} Slogan
:class: tip

 Replace "replace" by "integrate" 

::: 

This slogan probably is the answer to all the political issues mentioned in the beginning of the chapter. 

### 6.9.2 The order of processes

Let us take a moment reflect on the process of modifying a graph guided by relational thinking! 

As we saw in this chapter, we modify by searching for a pattern in a graph. Once a match is found, we remove and then add vertices and edges as indicated by the pattern to be integrated. Loosely speaking, in a free world, nothing prevents one from switching the order of these operations -- first add new vertices and edges and then remove, even though remove-first and add-next is more economic. A add-first and remove-next procedure must handle accidental errors like which are otherwise absent in the other order: 
- Removing what has been added
- Adding over the removal region. 

Hence, to remove first and to add next is elegant and as well as cleaner!  

As we saw in double pushouts, relational thinking precisely follows this order! Somehow, the mathematics 'knew' that it must first remove by computing pushout complement and then add by computing pushout over that complement! Switching the order is not possible. It is a wonder how did the math get the ordering right?! Even though it is a mystery, there is a message that is being conveyed. 


:::{admonition} Slogan
:class: tip

 Think relationally to get it right rightaway!!

::: 


[^1]: A few examples in this chapter have been inspired by [the handout](https://steemit.com/mathematics/@markgritter/double-pushouts-on-graphs) titled "Double Pushout Rewriting in Chemistry" authored by Georg Fischer, and Hannah Gschwentner, and [the blog post](https://steemit.com/mathematics/@markgritter/double-pushouts-on-graphs) "Double pushouts on Graphs" by Mark Gritter! 


