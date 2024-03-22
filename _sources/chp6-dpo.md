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

# Chapter 6: Cut-Copy-Glue Graphs

[Not ready for review]

[Our goal is not teaching how to do double pushout rewriting! 

Our goal is to demonstrate how to use graph glueing for a real world problem -- graph rewriting. Or ask questions to guide the reader's thinking to solve the problem. ]


"Find-and-replace" feature of text editors is one of the most powerful innovations of the 20th century. Inspite of unavailability of statistics, the advantage of this feature is tangible and undeniable! Beyond text editors, the concept of "find and replace" has also caused much chaos in the world than being helpful! When the European conquerors "found" native Americans settlements in Canada, they decided to "replace" the native culture by sending an entire generation of native American children to special missionary schools. This has resulted in trauma and chaos that continues well into the current times. Or a country trying to replace the ruling government of another country for political reasons. (may be an ecology example).  

> We know in hindsight that these are very bad decisions for a society! But, what was missed in the decision making process? 

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

Making changes to a graph may involve adding new vertices/edges (hence adding new connections) or removing existing vertices/edges (hence removing existing connections) or both. And these changes happen over particular region(s) of the graph.

A lot of concepts in the world around us are modelled as graphs. Hence, building a way to changing an existing graph that is guaranteed to be good is much more than just a theoretical exercise.

Let us begin! 

Given a pattern (a graph), and its replacement, our mission is to build a way to "find" the pattern in any host graph and "replace" it in a good way. 

## 6.1. Specs for "find and replace"

Microsoft Word provides an interface as in the picture below, to find a text and replace it with another text. This interface applies to any big body of text content. 

```{image} assets/Ch6/find-and-replace.png
:alt: Whoopsy!
:width: 450px
:align: center
```

:::{admonition} Pause and ponder

How would the interface for find-and-replace in graphs look like? 

:::

To begin with, let us think what would be filled in the "Find what:" and "Replace with:" boxes. 

```Find what``` box must take in a graph. This is the search pattern that will be matched in a host graph. ```Replace with```: box should also take in a graph. This is the replace pattern that will replace the match. 

Great, we are already half-way through! Next, we must how ```Find what``` and ```Replace with``` relate! In text documents, there is only one way of replacing a text by another. But in graphs, there are multiple ways of replacing one graph by another unless specified. 

Consider the following example:


```{image} assets/Ch6/find-and-replace-example.png
:alt: Whoopsy!
:width: 375px
:align: center
```

</br>

Our goal is to find a match of `Find what` in the host graph and to replace it with `Replace with`. There is a match in the host graph which is the vertex with the self-loop. However, there are at least two ways to replace the match with `Replace with`, as shown below.  

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

The only requirement in this case will be that the arrows needs to be **injective morphisms** (each vertex / edge of search pattern overlaps with at most one vertex / edge of the replacement pattern). 

Let us specify the overlaps for Replacement-1: 

```{image} assets/Ch6/interface-1.png
:alt: Whoopsy!
:width: 575px
:align: center
```

 and Replacement-2:

```{image} assets/Ch6/interface-2.png
:alt: Whoopsy!
:width: 625px
:align: center
```


Note that, the vertices and the edges of the search pattern which do not overlap are to be removed (from the host after finding a match). So, the embedding from `overlap` to `Find what` is the **specification for deletion**.

The vertices and the edges of the search pattern which do not overlap are to be added (to the host after finding a match). So, the embedding from `overlap` to `Replace with` is the **specification for addition**.

The overlap will be unchanged. It is precisely what remains after removing specified edges/vertices from the search pattern.

```{image} assets/Ch6/specification.png
:alt: Whoopsy!
:width: 625px
:align: center
```

Neat, huh?!

:::{admonition} **Exercise 1** 

````{div} wrapper 

Find the overlap between search and replace for the following replacement:




:::{admonition} Solution 
:class: dropdown
````{div} wrapper 
IMAGE
````

:::

:::{admonition} **Exercise 2** 

````{div} wrapper 

Apply the following find and replace to the given host graph at the highlighted match:




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

- The embedding into `Find what` is the **specification for deletion**. 

- The embedding into `Replace With` is the **specification for addition**.

:::

## 6.2. Finding a match

The next step is to answer how to find a match of a search pattern inside a host graph, similar to finding some text in a document. When a text editor receives an input like this, it find (exact) matches of the string of characters "Happy Priyaa". 

```{image} assets/Ch4/find-and-replace-text.png
:alt: Whoopsy!
:width: 250px
:align: center
```

However, in graph, connectivity matters. So when finding a graph in a host graph, we do not look for 1-to-1 correspondence between vertices / edges. A match simply needs to have the same connectivity as the search pattern. Does it a ring bell? A match is a graph morphism from ```Find what``` to a host graph. 

:::{Note}

A match is a graph morphism from `Find what` to a host.

:::

A few examples below: 

The following is an exact match.

```{image} assets/Ch6/Find-what-1.png
:alt: Whoopsy!
:width: 250px
:align: center
```

The following is a coarse-grained match.

```{image} assets/Ch6/Find-what-2.png
:alt: Whoopsy!
:width: 250px
:align: center
```

The following is an exact match. 

```{image} assets/Ch6/Find-what-3.png
:alt: Whoopsy!
:width: 250px
:align: center
```

:::{admonition} **Exercise**

How many matches (graph morpshism) are from the ```Find what``` to the host graph? 

```{div} wrapper 
IMAGE
```


:::{admonition} Solution 
:class: dropdown
```{div} wrapper 
IMAGE
```

::: 

:::{admonition} Key points
:class: tip

A *match* in a host graph is given by a morphism from ```Find what``` to a host graph! 

:::

## 6.3. Removing vertices and edges from a graph

We now have the specification for "find-and-replace" and the specification for a match. What remains is to perform the find-and-replace! We will begin with the deletion specification!

$$ {\sf overlap} \hookrightarrow {\sf Find~what} $$


### 6.3.1. Finding a match to remove vertices/edges

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


### 6.3.2. Performing deletion

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

:::{admonition} Example 

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

:::{admonition} Exercise 1 

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

:::{admonition} Exercise 2 

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
```{image} assets/Ch6/Ex-2-sol.png
:alt: Whoopsy!
:width: 650px
:align: center
```
````

:::

ONE MORE EXERCISE - WITH EDGES or VERTICES MERGED IN THE MATCH?

### 6.2.3 Managing the requirements of a match

In programming, there is a concept called "edge case". These are special situations where a code will fail to produce appropriate results. For example, a bug that occurs in only iPhone. They are called "edge cases" because these situations lie outside the normal flow of a code / algorithm and custom extra code is added for their special handling. There is an anecdote, "Edge cases are impossible to avoid, so keep them simple". 

:::{admonition} Million dollar questions:

What will happen when a match that violates the "no-dangling edge" requirement is supplied for pushout-completement? 

What will happen when a match that violates the "right identification" requirement is supplied for pushout-completement? 

:::

If the pushout-completement produces bad results, then these violations will be edge cases which needs to be handled outside the completement procedure -- like an algorithm to check if the match is appropriate. 

The good news is that the *universal nature* of pushouts guarantee us that pushout-complement will exist ONLY for appropraite matches!! That is, replacement will be done only when the match is appropriate. Hence, there are no edge cases!!! We did all the heavy lifting early-on by relying on relational thinking, considering all the relevant relationships, and getting the *plumbing* right! The pay off is that things flow smoothly without the need for extra intervention! 

> The procedure is the guard rail! 

A final question we must ask is that are pushout complements unique? The pushout completement graph computed for a deletion rule is unique because the deletion rule is an injective morphism.

:::{admonition} Key point
:class: tip

Vertices and edges of a graph is removed by computing pushout complement.

::: 

## 6.4. Adding vertices and edges to a graph

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

**Example 1:** Adding a single edge

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

:::{admonition} Exercise 1 cont.. 

:::

:::{admonition} Exercise 2 cont..


:::


:::{admonition} Key point 1
:class: tip

Vertices and edges are added by computing pushout.

::: 

:::{admonition} Key point 2: Find-and-replace machinery for graphs
:class: tip

A thing of beauty!!

A search pattern (`Find what`) found in a graph is carved into another pattern (`Replace with`) by computing pushout complement and followed by pushout.

```{image} assets/Ch6/final-picture.png
:alt: Whoopsy!
:width: 650px
:align: center
```

::: 

Common literature call our "Find-and-replace machinery" for graphs as Double Pushout Rewriting (DPO).

## 6.6. Find-and-replace in chemical reactions

## 6.7. Find-and-replace in game design

## 6.8. Exporting the Find-and-replace machinery to computers via Algebraic Julia

## A note on vocabulary
