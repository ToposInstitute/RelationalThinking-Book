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

```{image} assets/Ch4/quit-sugar-1.png
:alt: Whoopsy!
:width: 550px
:align: center
```

But the reality is: 

```{image} assets/Ch4/quit-sugar-2.png
:alt: Whoopsy!
:width: 350px
:align: center
```

because, the connections are broken causing the system to experience instability: 

```{image} assets/Ch4/quit-sugar-3.png
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

## 6.1 Finding a pattern 

Microsoft Word provides an interface as in the picture below, to find a text and replace it with another text. This interface applies to any big body of text content. 

```{image} assets/Ch4/find-and-replace.png
:alt: Whoopsy!
:width: 450px
:align: center
```

:::{admonition} Pause and ponder

How would the interface for find-and-replace in graphs look like? 

:::

To begin with, let us think what would be filled in the "Find what:" and "Replace with:" boxes. 

```Find what``` box must take in a graph. This is the graph that will be "found" in a host graph. ```Replace with```: box should also take in a graph. This is the graph that will replace the graph provided in ```Find what```. 

Great, we are half-way through! We are only half-way through beacuse, we also need to specify the relationship between ```Find what``` and ```Replace with```! This is because the ```Replace with``` graph may retain some or all of the vertices/edges of ```Find what``` graph. The replacement procedure must leave those vertices and edges intact in a host graph, and orient the replacement graph accordingly. There is only one way of replacing a text by another. But there are multiple ways to replace one graph by another depending how the replacement graph unless specified. 

In the following example, the single vertex in the ```Find what``` can be replaced any of the two vertices in the "```Replace with```" graph, or none (the vertex and the self loop is erased and a new graph is put in its place)!

```{image} assets/Ch4/find-and-replace-2.png
:alt: Whoopsy!
:width: 375px
:align: center
```

Hence, the specification for "find and replace" needs to include information on what vertices and edges are the same between the ```Find what``` and the ```Replace with``` graphs. The best way to do this would be to use an injective morphism from ```Find what``` to ```Replace with```. 

In the above example, there would be an injective morphism from ```Find-what``` into ``````Replace with`````` if the self-loop of the ```Find-what``` had remained in the ```Replace-with``` (as shown below). The graph on the right can be viewed as the ```Find-what``` plus a new vertex and an edge. Thus, an injection from a ```Find-what``` specifies what are the new vertices and edges added to the graph. 

```{image} assets/Ch4/injection-1.png
:alt: Whoopsy!
:width: 425px
:align: center
```

Similarly, there would be an injective morphism from ```Replace-with``` into ```Find-what```, if ```Replace-with``` has only vertices and edges which are  from ```Find-what```, as shown below. The graph on the left can be viewed as the ```Find-what``` minus the self-loop. Thus, an injection into a ```Find-what``` graph specifies what existing vertices and edges is to be removed from the graph.  


```{image} assets/Ch4/injection-2.png
:alt: Whoopsy!
:width: 350px
:align: center
```

The two injective morphisms (one into ```Find-what``` and one out ```Find-what```) together specify the ```Replace-with``` graph.


```{image} assets/Ch4/interface-2.png
:alt: Whoopsy!
:width: 500px
:align: center
```


 Thus, an interface for "find-and-replace" in graph includes the following information: 

```{image} assets/Ch4/interface-1.png
:alt: Whoopsy!
:width: 600px
:align: center
```


A Microsoft Word version of the above interface would probably look like: 

[Picture]

:::{admonition} **Exercise** 

````{div} wrapper 

Write the following find-and-replace using plus-minus interface:

Find and replace:

```{image} assets/Ch4/Ex-1-graphs.png
:alt: Whoopsy!
:width: 550px
:align: center
```

Interface:
```{image} assets/Ch4/Ex-1-interface.png
:alt: Whoopsy!
:width: 550px
:align: center
```
````


:::{admonition} Solution 
:class: dropdown
```{div} wrapper 
IMAGE
```

:::


The final step is to answer how to find the ```Find what``` graph inside a host graph, similar to finding some text in a document. When a text editor receives an input like this, it find (exact) matches of the string of characters "Happy Priyaa". 

```{image} assets/Ch4/find-and-replace-text.png
:alt: Whoopsy!
:width: 250px
:align: center
```

However, in graph, connectivity matters. So when finding a graph in a host graph, it is not necessarily an exact match, but a match which preserves connectivity. Does it ring bells? A match is a graph morphism from ```Find what``` to a host graph. A few examples below: 

The following is an exact match.

```{image} assets/Ch4/Find-what-1.png
:alt: Whoopsy!
:width: 450px
:align: center
```

The following is not an exact match but connectivity is preserved.

```{image} assets/Ch4/Find-what-2.png
:alt: Whoopsy!
:width: 450px
:align: center
```

The following is an exact match. 

```{image} assets/Ch4/Find-what-3.png
:alt: Whoopsy!
:width: 450px
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

The interface to find-and-replace for graphs includes 2 components: 
1. An injective morphism from ```Find what``` to add edges and vertices 
2. An injective morphism into ```Find what``` to erase edges and vertices
  

A *match* in a host graph is given by a morphism from ```Find what``` to a host graph! 

:::


## 6.2. Adding vertices and edges to host graph

Now that we have constructed the interface for graphs find-and-replace and know what a match (in a host graph) is, we shall move to performing the "find-and-replace". Let us consider the first component of "find-and-replace" interface: an injective morphism from ```Find what```. This morphism says what new vertices and edges to be added to the `Find what` graph. We shall use the idea of pushouts from the last chapter to say how to compute a new graph by adding these vertices and edges to a host graph once a match has been found. 

As always, let us begin by drawing diagrams. The advantage of drawing digrams is that it arranges information in an intuitive way that it makes it easier to "see" the solution! 

```{image} assets/Ch4/add-1.png
:alt: Whoopsy!
:width: 350px
:align: center
```

We now want to compute a new graph which has the additional edges and vertices of `Additional only` added to the host graph where the match has been found. Do you see how? 

::: {admonition} The answer is
:class: dropdown

Pushouts!!!

:::

Computing the pushout of the above diagram, gives exactly what we want! Note that the Graph-1 is the overlap between the host graph and `Additional only`. 

Let us try out some examples to make the idea absolutely clear! 

**Example 1:** Adding  one vertex and one edge

In the following example, the `Find what` is a vertex with a self-loop. The `Additional only` adds a new vertex and an edge. The host and the `Additional only` graphs look the same in this example. An exact match of `Find what` is spotted in the host graph. The graphs have been color-coded for easy identitification of the mappings. 

```{image} assets/Ch4/example-1.png
:alt: Whoopsy!
:width: 550px
:align: center
```

**Example 2:** Adding a new edge (exact match)

In the following example, the `Find what` has two vertices connected by an edge. The `Additional only` adds a new edge in between the vertices. An exact match of `Find what` is spotted in the host graph. The graphs have been color-coded for easy identitification of the mappings. Computing the pushout adds the new edge to the host graph. 

```{image} assets/Ch4/example-2.png
:alt: Whoopsy!
:width: 550px
:align: center
```
**Example 3:** Adding a new edge (coarse-grained match)

In the following example, the `Find what` has two vertices connected by an edge. The `Additional only` has a new edge in between the vertices. The match merges the two vertices of `Find what` into one vertex in the host graph. The graphs have been color-coded for easy identitification of the mappings. Computing the pushout adds the new edge (self-loop) to the host graph. 

```{image} assets/Ch4/example-3.png
:alt: Whoopsy!
:width: 550px
:align: center
```

Note that, in Example 3, the new edge (grey) to be added to the host graph is straight in `Additional only`. The edge when added to the host graph becomes a self-loop. Thus, when a match is found, the desired changes are **integrated** into the host graph rather than the match being replaced as such! 

## 6.3. Removing vertices and edges from host graph

In the last section, we saw that adding vertices and edges to an existing graph is as simple as a computing pushout! What about removing edges and vertices from an existing graph? What role will pushouts play in this procedure? 

As before, let us begin by making a diagram of the information we have: 

```{image} assets/Ch4/remove-1.png
:alt: Whoopsy!
:width: 550px
:align: center
```

We know that we cannot compute a pushout because computing a pushout requires two radiating arrows from a single graph. However, we know that the new graph which results after performing the changes is same as the host graph except for the vertices and edges that have been removed. Hence, there must exist a relationship (injection) between them, which we add to our first diagram:

```{image} assets/Ch4/remove-2.png
:alt: Whoopsy!
:width: 750px
:align: center
```

We also know that Graph-2 must relate to the new graph! (Why? Graph-1 has a match in the host graph Graph-2 injects into Graph-1. Since the new graph is simply the host graph with edges and vertices exclusive to Graph-1 removed, Graph-2 must have a match in the new graph.) We add this relationship too to the diagram. 

```{image} assets/Ch4/remove-3.png
:alt: Whoopsy!
:width: 750px
:align: center
```

Wait a minute!! That does like a pushout square where the host graph is in the pushout corner! The `Replace with [D]` is the overlap between the `Find what` and the New graph. `Find what` includes edges and vertices that has been removed. Glueing together `Find what` and the New graph gives the host graph!

Since the new graph completes a pushout square we shall call it as the **Pushout complement**. 

:::{Note} 

Complement means *the thing that completes or brings to perfection*. 

> A pushout complement is a pushout completement! 


Let us practice a few exercises on computing "pushout **complement**" 

:::{admonition} Exercise 1 

What is the pushout complement?

```{image} assets/Ch4/remove-5.png
:alt: Whoopsy!
:width: 750px
:align: center
```

The nodes exclusive to Graph-1 must be removed from the host graph. Thus, the pushout out complement must include: 
1.  all the parts of the host graph not under the match, and 
2.  only those parts of the match which is included in the `Replace with [D]`. 

:::{admonition} Solution
:class: dropdown

Only those parts of the match which is included in the `Replace with [D]` are vertices "1", and "2".

````{div} wrapper 
```{image} assets/Ch4/remove-6.png
:alt: Whoopsy!
:width: 650px
:align: center
```
````

The pushout complement includes all the edges and vertices in the host graph that is not under the match (all unlabelled vertices and edges),  and includes those vertices and edges in the match which are in Graph-2.

:::

:::{admonition} Exercise 2 

What is the pushout complement?


This problem has a coarse-grain match. Vertices "1" and "2" of `Find what` are sent to the same vertex in the host graph. Simiarly vertices "3" and "4" are sent to the same vertex in the host graph.

```{image} assets/Ch4/remove-8.png
:alt: Whoopsy!
:width: 750px
:align: center
```


:::{admonition} Solution
:class: dropdown

````{div} wrapper 
```{image} assets/Ch4/remove-9.png
:alt: Whoopsy!
:width: 650px
:align: center
```
````

:::

:::{admonition} Exercise 3 

What is the pushout complement?


```{image} assets/Ch4/remove-10.png
:alt: Whoopsy!
:width: 750px
:align: center
```


:::{admonition} Solution
:class: dropdown

The pushout complement includes all the edges and vertices in the host graph that is not under the match (all unlabelled vertices and edges),and includes those vertices and edges in the match which are in Graph-2.

````{div} wrapper 
```{image} assets/Ch4/remove-11.png
:alt: Whoopsy!
:width: 650px
:align: center
```
````

:::

ONE MORE EXERCISE - WITH EDGESor VERTICES MERGED IN THE MATCH?

### 6.4.1 Requirements on matching

Erasing edges and vertices require care more than adding them. For example, suppose a vertex is erased in the host graph, then all the edges 
connected to the vertex must also be erased. Otherwise, we will end up in a zombie-graph as shown in the introduction. Hence, care is needed when finding a match. 

Suppose, we have the following specification for deletion: vertex 2 and edge '2-3' needs to be deleted! 
```{image} assets/Ch4/remove-7.png
:alt: Whoopsy!
:width: 650px
:align: center
```

Can you see why the following match is incorrect even though it is a graph morphism? 

```{image} assets/Ch4/dangling-2.png
:alt: Whoopsy!
:width: 650px
:align: center
```

Removing vertex 2 from the host graph will result in a dangling edge, see the diagram picture below!

```{image} assets/Ch4/dangling-3.png
:alt: Whoopsy!
:width: 250px
:align: center
```

An appropriate match will be:

```{image} assets/Ch4/dangling-1.png
:alt: Whoopsy!
:width: 550px
:align: center
```

We know that a match need not be injective. This may lead to problems. For example, do you see why the following match doees not work?


```{image} assets/Ch4/identification-1.png
:alt: Whoopsy!
:width: 550px
:align: center
```

The above match is not appropriate because vertex 2 needs to be deleted and vertex 1 needs to retained. However, the match identifies both 1 and 2 to the same vertex in the host graph. So what should we do now -- keep "1,2" or delete "1,2" from the host graph? A good match, in the first place, MUST avoid identifying 1 and 2 into the same bin since either choice is incorrect. 

Thus, an appropriate match will leave all the edges connected after the replacement and will identify vertices / edges to be deleted seperate from the good vertices / edges in the host graph. In a nutshell, it will satsify the "no-dangling-edge" and the "right identification" conditions.

In programming, there is a concept called "edge case". These are special situations where a code will fail to produce appropriate results. For example, a bug that occurs in only iPhone. They are called "edge cases" because these situations lie outside the normal flow of a code / algorithm and custom extra code is added for their special handling. There is an anecdote, "Edge cases are impossible to avoid, so keep them simple". 

:::{admonition} Million dollar questions:

What will happen when a match that violates the "no-dangling edge" requirement is supplied to pushout-completement procedure? 

What will happen when a match that violates the "right identification" requirement is supplied to the pushout-completement procedure? 

:::

If the pushout-completement produces bad results, then these violations will be edge cases which needs to be handled outside the completement procedure -- like an algorithm to check if the match is appropriate. 

The good news is that the *universal nature* of pushouts guarantee us that pushout-complement will exist ONLY for appropraite matches!! That is, replacement will be done only when the match is appropriate. Hence, there are no edge cases!!! We did all the heavy lifting early-on by relying on relational thinking, considering all the relevant relationships, and getting the *plumbing* right! The pay off is that things flow smoothly without the need for extra intervention! 

> The procedure is the guard rail! 

A final question we must ask is that are pushout complements unique? That is, is the new graph computed by removing the vertices and edges specified by the deletion rule unique. We want it to be unique!

::: {admonition} Pause and ponder

Why is the pushout complement computed for a deletion rule unique?

:::

In general pushout complements are not unique. We have shown an example below. However, because the deletion rule is an injection, guarantees that its pushout complement is unique. 

:::{admonition} A general example where pushout complement is not unique. 
:class: dropdown

Two different pushout complement graphs for the same pushout graph.

````{div} wrapper 
IMAGE1, IMAGE2

````
:::


## 6.4. Add and erase in one go!

Here is a thing of beauty!!

## 6.5. Graph rewriting in organic chemistry

## 6.6. Graph rewriting in game design

## 6.7. Making computer do the "find-and-integrate" changes!
