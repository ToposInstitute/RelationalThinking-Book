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

# Chapter 3: Schemas


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

In this case, directed graphs are the perfect choice for modeling because TikTok follows are _directional_. Someone you follow may not follow you back.

Now let's imagine doing the same thing for connections on LinkedIn. In this social network, both parties must mutually agree to the connection. So a LinkedIn connection is symmetric, not directional.

To model this kind of social network we need a different kind of graph. We call these "undirected graphs" and, as the name surely suggests, these are like directed graphs but with non-directional edges connecting the vertices instead of arrows.

```{image} assets/Ch4/LinkedIn.png
:alt: Whoopsy!
:width: 500px
:align: center
```
PAUSE AND PONDER: How is the data of an undirected graph different from the data of a directed graph? How might you communicate the details of an undirected graph to a computer?

In this chapter we're going to look at a few different flavors of graphs. In the process, we'll develop a general and flexible framwork for working with all kinds of graphs in AlgebraicJulia.

## Introducing Schemas for directed graphs

We'll begin this chapter with a little tidying up. The basic building block we've been working with so far is a map, a bundle of connections in which every item on one side gets connected to some item on the other. For example:

```{image} assets/Ch4/SourceMap.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

Source and target maps can be a little _*busy*_ to look at so we're going to simplify our view! Let's introduce a new abstraction that hides all of this detail. We're going to:
* wrap these connections in one big tube 
* put an arrow point on this tube so we can remember which direction the connections were going
* wrap the items at either end in labelled spheres
* and forget all about those underlying details!

```{image} assets/Ch4/SchemaDef.gif
:alt: Whoopsy!
:width: 500px
:align: center
```
A big chunky arrow like this is easier to draw and easier to think about. Whenever we see one we can take for granted that there is some specific set of connections bundled up "under the hood." This is just like in an algebra equation where, when we see the variable 'x', we know it stands for some specific number. Chunky arrows are like variables for maps.

Figures built from these chunky arrows are known as schemas. In moving from an explicit map to its schema, we are moving from rung 2 ("data") to rung 3 ("blueprints") on our ladder of abstractions.

What is the schema that represents a directed graph? Recall that a directed graph is defined by two maps, both of which connect the same collections of arrows and vertices. 

```{image} assets/Ch4/Graph1ST.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

Instead of representing these maps side by side like this, let's combine them so that they run in parallel. Our chunky arrows will then be in this configuration:

```{image} assets/Ch4/DGInstance1.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

A pair of parallel source and taget maps is the underlying pattern that is common to all directed graphs, their essential "blueprint". We generally draw this as two arrows marked `src` and `tgt`.

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
Having given a general characterization of directed graphs as a schema, we will now show how we can _modify_ this schema to define other kinds of graphs.

## Reflexive Graphs

Suppose we were making a directed graph to represent the game of tic tac toe, where:

* Vertices are the states of the game
* Arrows are "moves" of the game, going from one state to another.

A piece of our graph would look like this:

```{image} assets/Ch4/TicTacToe.jpg
:alt: Whoopsy!
:width: 500px
:align: center
```

Now let's imagine doing the same thing for the ancient board game Go. An important thing to know about this game is that the player always has the option to "pass." That is, one of the available moves at any given turn is to stay in the current state. Thus, every vertex in this graph is going to have one arrow that loops back on it.

```{image} assets/Ch4/GO.jpg
:alt: Whoopsy!
:width: 500px
:align: center
```
This happens a lot when modeling with directed graphs: we'll find ourselves in a situation where every vertex needs to have a special looped arrow attached to it. It happens so frequently that we give these graphs a special name - they're called "reflexive graphs." 

A reflexive graph is defined as a directed graph in which "every vertex has a special self-pointing arrow." We can express this idea with a map going from vertices to arrows, where each vertex is connected to its self-looping arrow.

```{image} assets/Ch4/ReflexiveMap.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

We call this the reflexive map or `ref`.



```{image} assets/Ch4/ReflexiveGraphInstance.gif
:alt: Whoopsy!
:width: 800px
:align: center
```
Now think about the following sentence: 

"Every vertex is the source and target of its self-looping arrow." 

On the one hand, this is effectively the definition of a reflexive graph. On the other hand, we can easily translate this sentence into a closed loop condition on the above maps. 

In words:

"If we start from any vertex and follow the reflexive map and then either the source or target maps from there, we should always wind up at the back where we started." 

As an equation:

```{image} assets/Ch4/ReflexiveGraph.jpg
:alt: Whoopsy!
:width: 800px
:align: center
```
That is, a directed graph can be considered a reflexive graph if and only if there exists a map "ref" such that it forms a 
set of loops with the existing maps in the directed graph schema. Thus, to work with reflexive graphs in Algebraic Julia we think of them as special cases of direct graphs and specify to the program that we want it to restrict itself to directed graphs for which such a reflexive map exists.

//code snippet showing reflexive graph definition.

Reflexive graphs are widespread and useful. In applied settings they are excellent for geometric applications. Reflexive relationships are prevalent in mathematics (divisibility among the integers, subsets among sets, etc.). And in category theory all structures of interest (preorders, categories, etc.) are reflexive graphs. But for our purposes, reflexive graphs are important because it's interesting to try and count the morphisms between two of them!

...where the way we define our graph is by starting with all directed graphs and then specialize to only those which can satisfy the commutativity condition.
If we were programming in terms of "things" we'd have to add to our codebase to 
We think of reflexive graphs as special cases of directed graphs. Therefore any operations that are defined for directed graphs specialize to reflexive graphs as a subset.



## Undirected Graphs


Surprisingly, we can think of undirected graphs as _special cases_ of directed graphs. 

An arrow in a directed graph is like a one-way street, a unidirectional pointer from its source to its target. An edge in an undirected graph is more like like a *two-way street*, which the connection goes mutually in both directions. If we take this “two-way street” idea literally we can see that every undirected graph is *equivalent* to a directed graph with pairs of arrows in place of each edge.

```{image} assets/Ch4/TwoWayStreet.png
:alt: Whoopsy!
:width: 500px
:align: center
```



```{image} assets/Ch4/InversionMap.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

```{image} assets/Ch4/UndirectedGraph.jpg
:alt: Whoopsy!
:width: 500px
:align: center
```

```{image} assets/Ch4/UndirectedGraphInstance.gif
:alt: Whoopsy!
:width: 500px
:align: center
```






And in the next chapter we will focus on examples using undirected graphs instead of directed graphs. They're a little simpler to draw and think about, and are perfectly useful for telling the rest of our story. 

## Other kinds of schemas

In this chapter we have used schema constraints to look at a few different flavors of graphs. Graphs are relatively simple and that's why we chose to focus on them. It is easier to grasp the concept of a schema when the thing the schema represents is straightforward to understand, which graphs are.But the framework we have developed here can actually be extended beyond just graphs, to an extraordinary variety of elaborate and useful concepts. Indeed, one of the profound offerings of Algebraic Julia is the sheer number of mathematical abstractions it can handle in terms of schemas and constraints. In this final section we offer a brief glimpse at some more powerful models and ideas that are also captured by this framework.

Disclaimer:It is out of scope to go into any detail on the following schemas. We mention them here, in passing, only to give some sense of the possibilities with Algebraic Julia. 

## Simplicial sets

We can generalize reflexive graphs to higher dimensions using schemas. The result is one of the algebraic topologist's favorite tools: simplicial sets. Ordinary graphs connect 0-dimensional vertices using 1-dimensional lines. With simplicial sets we can also attach 2-dimensional triangles, building up triangulated surfaces. Going up another dimension we can attach 3-dimensional tetrahedra to make solid figures. And so on.

For the mathematician, simplicial sets are useful because they turn geometry into algebra: a simplicial triangulation of a topological space is a combinatorial object that can be reasoned about. For the applied scientist, simplicial sets may be useful as a way of 3D modeling, as we'll see in Chapter 7. Finally, in Algebraic Julia, simplicial sets are practical because all of the rules for how different parts must attach can be fully captured with a few compositionality constraints.

```{image} assets/Ch4/SimplicialSets.jpg
:alt: Whoopsy!
:width: 500px
:align: center
```

## Petri nets

On the more "applied" side, we have the example of Petri nets, a sophisticated modeling system for the analysis of concurrent systems. It was developed by German computer scientist Carl Adam Petri in the 1960's, whose goal was to provide a system that could model parallel processes, synchronization, resource sharing, and which had an intuitive graphical notation. Petri nets provide a modeling tool that is suitable for a wide variety of systems, from chemical reactions to business management logistics.It's important to note that Petri nets were developed by practitioners, not mathematicians. It was born from necessity, designed to fill a utility gap in existing systems. But because Carl Petri gave the system an exact mathematical definition for its execution semantics, we are able to represent petri nets in terms of schemas and work with them in Algebraic Julia.

```{image} assets/Ch4/Petri_Net.jpg
:alt: Whoopsy!
:width: 500px
:align: center
```

## Databases

Algebraic Julia's implementation of Petri nets is called AlgebraicPetri.js. Documentation can be found here along with several examples of scientific models using Petri nets, including population dynamics, epidemiological models and enzyme reactions.

Databases?:

Conclusion*** That “embedding” might seem a little useless/artificial/formal. In the next section we develop tools that*** That the tool we develop in the final section: “Double Pushout Rewriting” can be applied to all of the “special” examples above.