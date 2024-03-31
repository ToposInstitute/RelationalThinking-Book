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

# Chapter 3: Blueprints


We have now seen how directed graphs can be useful for modeling the world. However, in some situations they're not actually the best choice.

Suppose we were making a directed graph to represent the social network on Tiktok:
* Vertices are people
* Arrows are "follows", going from a person to someone they follow.

A piece of our graph would look like this:

```{image} assets/Ch4/TikTok.png
:alt: Whoopsy!
:width: 500px
:align: center
```
<br>

In this case, directed graphs are the perfect choice for modeling the social network because TikTok follows are _directional_. Someone you follow may not follow you back.

Now let's imagine doing the same thing for connections on LinkedIn. In this social network, both parties must mutually agree to the connection. So a LinkedIn connection is symmetric, not directional.

To model this kind of social network we need a different kind of graph. We call these "undirected graphs" and, as the name surely suggests, these are like directed graphs but with non-directional edges connecting the vertices instead of arrows.

```{image} assets/Ch4/LinkedIn.png
:alt: Whoopsy!
:width: 500px
:align: center
```
<br>
PAUSE AND PONDER: How is the data of an undirected graph different from the data of a directed graph? How might you communicate the details of an undirected graph to a computer?

<br>
<br>

Undirected graphs are just one example from a whole zoo of different _kinds_ of graphs we might want to model with. In this chapter we'll look at a few specimens from this zoo. In the process, we'll develop a general and flexible approach for working with many different flavors of graphs in AlgebraicJulia.

## Introducing Schemas for directed graphs

We'll begin this chapter with a little tidying up. The basic building block we've been working with so far is a map, a bundle of connections in which every item on one side gets connected to some item on the other. For example:

```{image} assets/Ch4/SourceMap.gif
:alt: Whoopsy!
:width: 500px
:align: center
```
<br>

Source and target maps can be a little _busy_ looking so we're going to simplify our view! Let's introduce a new abstraction that hides all of this detail. We're going to:
* wrap these connections in one big tube 
* put an arrow point on this tube so we can remember which direction the connections were going
* wrap the items at either end in labelled spheres
* and forget all about those underlying details!

```{image} assets/Ch4/SchemaDef.gif
:alt: Whoopsy!
:width: 500px
:align: center
```
<br>

A big chunky arrow like this is easier to draw and easier to think about. Whenever we see one we can take for granted that there is some specific set of connections bundled up "under the hood." This is just like in an algebra equation where, when we see the variable 'x', we know it stands for some specific number. Chunky arrows are like variables for maps.

Figures built from these chunky arrows are known as schemas. In moving from an explicit map to its schema, we are moving from rung 2 ("data") to rung 3 ("blueprints") on our ladder of abstractions.

### Directed graphs

What is the schema that represents a directed graph? Recall that a directed graph is defined by two maps, both of which connect the same collections of arrows and vertices. 

```{image} assets/Ch4/Graph1ST.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

<br>

Instead of representing these maps side by side like this, let's combine them so that they run in parallel. Our chunky arrows will then be in this configuration:

```{image} assets/Ch4/DGInstance1.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

A pair of parallel source and target maps is the underlying pattern that is common to all directed graphs, their essential "blueprint". We generally draw this as two arrows marked `src` and `tgt`.

```{image} assets/Ch4/DirectedGraphSchema.jpg
:alt: Whoopsy!
:width: 500px
:align: center
```

Any _particular_ pair of maps between the same arrows and vertices is said to be an "instance" of this schema. By filling in the schema in different ways we create different instances, and every instance corresponds to some directed graph.


```{image} assets/Ch4/DGraphInstance.gif
:alt: Whoopsy!
:width: 800px
:align: center
```
<br>

Having given a general characterization of directed graphs as a schema, we will now show how we can _modify_ this schema to define other kinds of graphs.

## Reflexive Graphs

Suppose we were making a directed graph to represent the game of tic-tac-toe, where:

* Vertices are the states of the game
* Arrows are "moves" of the game, going from one state to another.

A piece of our graph would look like this:

```{image} assets/Ch4/TicTacToe.jpg
:alt: Whoopsy!
:width: 500px
:align: center
```
<br>

Now let's imagine doing the same thing for the ancient board game Go. An important thing to know about this game is that the player always has the option to "pass." That is, one of the available moves at any given turn is to stay in the current state. Thus, every vertex in this graph is going to have one special arrow that loops back on itself.

```{image} assets/Ch4/GO.jpg
:alt: Whoopsy!
:width: 500px
:align: center
```
<br>

This actually happens a lot when modeling with directed graphs: we find ourselves wanting every vertex to have a special "self-looping" arrow attached to it. In fact, this happens so frequently that we give these graphs a special name - they're called "reflexive graphs." They are widespread and useful. In applied settings they are excellent for geometric applications. Reflexive relationships are prevalent in mathematics (divisibility among the integers, subsets among sets, etc.). And in category theory all structures of interest (preorders, categories, etc.) are reflexive graphs.

PAUSE AND PONDER: How is the data of an reflexive graph different from the data of a directed graph? How might you communicate the details of an undirected graph to a computer?

We can define a reflexive graph as a _directed graph_ with the added condition that "every vertex has a special self-pointing arrow." This idea can be expressed with a map going from vertices to arrows, where each vertex is connected to its self-looping arrow.

```{image} assets/Ch3/ReflexiveMap.gif
:alt: Whoopsy!
:width: 500px
:align: center
```
<br>

Every reflexive graph has such a map, called its reflexive map or `ref`. We can combine this reflexive map with the graph's source and target maps. (Notice that the reflexive map points in the opposite direction, from vertices to arrows.)


```{image} assets/Ch4/ReflexiveGraphInstance.gif
:alt: Whoopsy!
:width: 800px
:align: center
```
<br>

PAUSE AND PONDER. Let your eyes follow the dashed lines around the figure. Do you see any "patterns" in this system of connections?

What can we notice about the above instance? Well, for one thing, in order for an arrow to be the self-loop of a given vertex, it must have _that vertex_ as its source. In other words, if we 

* pick any vertex and...
* ...follow the reflexive map from right to left, from a vertex to its self-looping arrow

...and then 

* continue along the source map from _that_ arrow, moving from left to right to some vertex 

...we should always end up back where we started. For example, starting from the heart we get the following loop:

IMAGE OF SINGLE SOURCE LOOP

"Source following ref" should always from a closed loop.

By the same reasoning, the target map following the reflexive map should always form a closed loop as well.

IMAGE OF SINGLE TARGET LOOP

If we look closely at the instance data for the source maps we indeed see that all such loops are closed:

IMAGE OF FADETHROUGH

This closed loop condition turns out to be equivalent to the defition of a relfexive graph: A loop fails to be closed in the schema instance if and only if an arrow is not self-pointing in the associated graph. 

PAUSE AND PONDER: Can you think through why this is?

In first defining reflexive graphs we had to establish what we meant using semantic ideas like "self-loops" and phrases like "for every vertex...". We have now found an equivalent way of saying the same thing in terms of a maps and whether or not certain paths form closed loops. And "maps and closed loops" are exactly the kind of thing AlgebraicJulia can understand!


## Into the computer

Let's encode a reflexive graph schema in AlgebraicJulia!

First off, you may have noticed that schemase _are_ a kind of directed graph. So we can use our familiar source and target map formalism to input the general schema shape.

Now we want to add the closed loop conditions. We can express these as text...

(Note how the order in which we write the arrows seems backwards from the order in which you would actually traverse those arrows along the route. Unfortunately this is the notational convention! One way to think of it is to read the symbol • as the word "after." So "V•S2" is understood to mean "V after S2.")

IN the chapter on dynamical systems we saw how to start with a directed graph and then add addtiional data - states and update rules - to turn it into a a psecific dunamical system. In AlgebraicJulia we do something similar. We define the underlingying grpah and then add the commutativity conditions we want the underlying maps to statisfy.

We express these conditions as an equation. We use • notation, which means "following". We use "id" to mean a amap from a vertex to itself. Our condition "source map following ref map is the same as doing nothing." becomes scr•ref = id. 










```{image} assets/Ch4/ReflexiveGraph.jpg
:alt: Whoopsy!
:width: 800px
:align: center
```
That is, a directed graph can be considered a reflexive graph if and only if there exists a map "ref" such that it forms a 
set of loops with the existing maps in the directed graph schema. Thus, to work with reflexive graphs in Algebraic Julia we think of them as special cases of direct graphs and specify to the program that we want it to restrict itself to directed graphs for which such a reflexive map exists.

//code snippet showing reflexive graph definition.

 But for our purposes, reflexive graphs are important because it's interesting to try and count the morphisms between two of them!

...where the way we define our graph is by starting with all directed graphs and then specialize to only those which can satisfy the commutativity condition.
If we were programming in terms of "things" we'd have to add to our codebase to 
We think of reflexive graphs as special cases of directed graphs. Therefore any operations that are defined for directed graphs specialize to reflexive graphs as a subset.



## Undirected Graphs


Now let's return to the question of undirected graphs. Can we design a schema for these? Surprisingly, it turns out that we can think of undirected graphs as _special cases_ of directed graphs. 

An arrow in a directed graph is like a one-way street, a unidirectional connection pointing from its source to its target. In an undirected graph, an edge is more like like a *two-way street* in which the connection goes mutually in both directions. If we take this “two-way street” analogy literally we can see that every undirected graph is *equivalent* to a directed graph in which we've substituted a pair of opposing arrows in place of each undirected edge.

```{image} assets/Ch4/TwoWayStreet.png
:alt: Whoopsy!
:width: 500px
:align: center
```
Consider this simple undirected graph and its associated 'directed-graph-with-paired-arrows':

//IMAGE OF UNIDRECTED/DIRECTED VERSIONS OF THE SAME GRAPH

What must be true about a directed graph in order for it to correspond to an undirected graph? The condition on the directed graph is that "every arrow is associated with a unique partner arrow." We can express this idea as a map, in which each arrow gets connected to its unique partner:

```{image} assets/Ch3/InversionMap.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

We call this the inversion map or `inv`. We can combine this inversion map with the graph's source and target maps, attaching it as a self-point loop on the arrows.


IMAGE: CHANGE THIS TO HAVE THOUGHT BUBBLES OF THOUGHT BUBBLES. ADD "arrows" and "vertices" labels to the spheres
```{image} assets/Ch4/UndirectedGraphInstance.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

Notice how the instance data represents a directed graph which in turn represents and undirected graph!

PAUSE AND PONDER. Let your eyes follow the dashed lines around the figure. Do you see any "patterns" in this system of connections?

What can we notice about the above instance? The paired arrows of the directed graph must point in opposite directions, meaning the source of one arrow must be the target of its partner and vice versa. We can express this a closed loop condition. In words, if we 

* pick any arrow and...
* ...follow the source map from left to right, from an arrow to it's source vertex

...we end up in the same place as if we had..

* followed the inverse map from that arrow to its partner arrow.
* and then followed the target map from _that_ arrow to it's target vertex




And similarly going the other way around for S/T maps.

Expressed in equations:


```{image} assets/Ch4/UndirectedGraph.jpg
:alt: Whoopsy!
:width: 500px
:align: center
```




PAUSE AND PONDER: Consider the difference between your understanding of a graph and Algebraic Julia's. For you, this schema might represent a directed or undirected graph. You can interpret it any way you like. For AlgebraicJulia there is no interepretaion. Every idea is phrased exclusively in terms of schemas, maps and commutativity conditions. It has no idea what any of this "means" to you.

We chose to use thought bubbles to indicate these interepretations, signalling that they are ideas that exist only the in mind of the progammer.

## Other kinds of schemas

In this chapter we have used schemas and commutativity conditions to look at a few different flavors of graphs. But the framework we have developed here can actually be extended beyond just graphs, to an extraordinary variety of elaborate and useful concepts. Indeed, one of the profound offerings of AlgebraicJulia is the sheer number of mathematical abstractions it can handle in terms of schemas. In this final section we offer a brief glimpse at some more powerful models and ideas that are also captured by this framework.

DISCLAIMER: It is out of scope to go into any detail on the following schemas. We mention them here, in passing, only to give some sense of the possibilities available with AlgebraicJulia. 

## Simplicial sets

We can generalize reflexive graphs to higher dimensions using schemas. The result is one of the algebraic topologist's favorite tools: simplicial sets. Ordinary graphs connect 0-dimensional vertices using 1-dimensional lines. With simplicial sets we can also attach 2-dimensional triangles, building up triangulated surfaces. Going up another dimension we can attach 3-dimensional tetrahedra to make solid figures. And so on.

For the mathematician, simplicial sets are useful because they turn geometry into algebra: a simplicial triangulation of a topological space is a combinatorial object that can be reasoned about. For the applied scientist, simplicial sets may be useful as a way of 3D modeling, as we'll see in Chapter 7. Finally, in AlgebraicJulia, simplicial sets are practical because all of the rules for how different parts must attach can be fully captured with a few commutativity conditions.

```{image} assets/Ch4/SimplicialSets.png
:alt: Whoopsy!
:width: 800px
:align: center
```

## Petri nets

On the more "applied" side, we have the example of Petri nets, a sophisticated modeling system for the analysis of concurrent systems. It was developed by German computer scientist Carl Adam Petri in the 1960's, whose goal was to provide a system that could model parallel processes, synchronization, resource sharing, and which had an intuitive graphical notation. Petri nets provide a modeling tool that is suitable for a wide variety of systems, from chemical reactions to business management logistics.

It's important to note that Petri nets were developed by practitioners, not mathematicians. The system was born from necessity, designed to fill a utility gap in other approaches to modeling. But because Carl Petri gave the system an exact mathematical definition for its execution semantics, we are able to represent Petri nets in terms of schemas and work with them in Algebraic Julia.

```{image} assets/Ch4/Petri_Net.jpg
:alt: Whoopsy!
:width: 800px
:align: center
```



Algebraic Julia's implementation of Petri nets is called AlgebraicPetri.js. Documentation can be found [here](https://algebraicjulia.github.io/AlgebraicPetri.jl/dev/) along with several examples of scientific models using Petri nets, including population dynamics, epidemiological models and enzyme reactions.

## Databases

The whole concept of a schema originally comes from database theory. We can think of the underlying connections in a schema as linked data. For example the grey circles may represent a database of 'people' and a given arrow may repesent a tabulated relationship between those people (Who loves whom? Who is the enemy of whom? etc.) Building a schema is then just "structuring a query" on that database by defining new relationships in terms of existing ones.


```{image} assets/Ch3/DatabaseLabeled.png
:alt: Whoopsy!
:width: 800px
:align: center
```
<br>

Data often gets corrupted when transferred between contexts, a major issue in database management. Data corruption is a kind of analog for our dangling edge condition on graphs. And in the same way that AlgebraicJulia will allow us to use high level abstractions to resolve our dangling edge problems, there are other category theoretic techniques for data migration that offer a canonical way of migrating data that automatically takes care of various annoying edge conditions. See [here](https://arxiv.org/abs/1009.1166) for details.

