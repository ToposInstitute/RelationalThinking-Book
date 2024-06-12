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

# Chapter 3: Graph Morphisms

:::{note}
This book is a work-in-progress! We'd love to learn how we can make it better, especially regarding fixing typos or sentences that are unclear to you. Please consider leaving any feedback, comments, or observations about typos in [this google doc](https://docs.google.com/document/d/1MvhNuap0QLMAfrMQLIAxbclBx0vjt6vyK8BhVhLwFoQ/edit).
:::

## Introduction

In the previous section we took a look at directed graphs; what they are, what they can represent, and how we can describe them to a computer. In that chapter we investigated the relationships within a graph. In this chapter, we're going to investigate relationships *between* entire graphs.

Comparing graphs is useful. Suppose there's some situation that we've modeled using a directed graph. If someone else has created a related directed graph we may want to be able to systematically compare it to ours or even merge the two.

But an understanding of graph relationships are also an important thinking tool, and the next step on our journey to relational thinking. We are building up an understanding of graphs and, at each stage, we're paying special attention to the *relationships* that are relevant. Like any abstraction, this may seem a bit pedantic and unnecessary at first. Relational thinking takes time. By the end we will see that there are nice freebies you get when you build up your manner of thinking by focusing on relationships.

## 1. Graph Injections


In what way might two graphs be related? One of the most obvious possibilities is that there may be a copy of one graph inside of the other.

In Chapter 1 we saw an example of a directed graph which described the layout of a ski resort. In this account, you could ride the ski lift up and down the mountain or ski from the top of the mountain into a nearby village.

But suppose we visit this ski resort in the Summer when there's no snow. At this time of year the resort lets guests ride up and down the mountain in the lift (which provides a *lovely* view!), but there is no way to get down the mountain into the village without a snow pack to ski on.

```{image} assets/Ch3-4/SummerWinter.png
:alt: Whoopsy!
:width: 800px
:align: cinter
```

Note that the Summer graph is basically contained inside of the Winter graph. How should we characterize this relationship?

In the previous chapter we thought about relationships in terms of maps. We can do something similar here. For the Summer graph to be a "part" of the Winter graph means, in effect, that we can map the Summer graph *into* the Winter graph, taking vertices to vertices and arrows to arrows.

```{image} assets/Ch3-4/InjectSki.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

We call this a "graph injection". In order to visualize injections clearly let's introduce some new visuals.

*Figuratively:*

Let's step away from the ski lodge and look at graph injections more generally. We want to represent the injection of graph 1 into graph 2:

First, let's redesign graph 2 to be a "recipient" of the graph 1, moving the colors and shapes out of the way.

We now place the first graph inside of the second graph.

```{image} assets/Ch3-4/DefineInjection.gif
:alt: Whoopsy!
:width: 800px
:align: center
```



This gives us an image of "what went where" in our injection. We sometimes call this the "image" of the first graph in the second.

It's important to understand that a graph is not allowed to come apart when getting injected. An arrow must stay attached to its source and target vertices through this process.

```{image} assets/Ch3-4/DanglingEdge.png
:alt: Whoopsy!
:width: 800px
:align: center
```

For modeling purposes, it is the connectivity of the graph that matters. If an injection ruptures the graph this way it would also rupture any underlying meaning we may have ascribed to that graph. DEFINE Dangling edge condition (eg In the ski lodge example, ...?)


### Problems ###

1. How many ways can you inject this upper graph into the lower graph?

```{image} assets/Ch3-4/Problem1.png
:alt: Whoopsy!
:width: 500px
:align: center
```
Answer:

Three! The upper triangle can be injected in any orientation.
```{image} assets/Ch3-4/Problem1Solution.png
:alt: Whoopsy!
:width: 500px
:align: center
```

2. How many ways can you inject this upper graph into the lower graph?
```{image} assets//Ch3/Problem2.png
:alt: Whoopsy!
:width: 500px
:align: center
```

Answer:

Twelve?

*Formally:*

In chapter 1 we used source and target maps to describe a directed graph. Maps can also be used to describe an injection. In this case the a “vertex map” and an “arrow map” run vertically, telling where each vertex and arrow ends up in the injection.

```{image} assets//Ch3/A,V,Maps.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

These two maps capture all the relevant information about the injection.

Recall our dangling edge condition: For these maps to represent a proper injection the arrows must stay attached to their source and target vertices. Clearly the above maps must work together in some way to prevent breakage. What conditions must be true in order to ensure that a graph doesn't come apart on entry?

To answer this, let's arrange our maps into a square, with the source maps for graph 1 and graph 2 running horizontally and the “arrow map” and “vertex map” running vertically :


```{image} assets//Ch3/CommSquare.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

Note how the dashed lines seem to flow “out” from the arrows in the upper left and flow “in” to the vertices at the lower right. Starting from any given arrow there are two paths you can take around this square, an upper route and a lower route. Let's focus on a specific arrow and consider what these routes “mean”. (We'll label the endpoints as "A" and "V")

### Upper Route ###


```{image} assets//Ch3/UpperRoute.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

Reading across the top we have that the red arrow has the heart as its source. Reading down the right side we see that the heart gets sent to the square vertex in graph 2. So the square is "the vertex that receives A's source."

### Lower Route ###

```{image} assets//Ch3/LowerRoute.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

Reading down the left we have that the red arrow from graph 1 gets sent to the blue arrow from graph 2 (In other words, the blue arrow is its "image"). Reading across the bottom we see that the blue arrow has the square vertex as its source. So the square is "the source of the image of A."

Let's now revisit to our dangling edge condition. “Coming apart” means, literally, that a vertex and an arrow that were connected in graph 1 are not connected when they land in graph 2. Suppose an arrow gets separated from its source by an attempted injection. For that arrow, the two routes around the square look like this:


```{image} assets//Ch3/OpenSquare.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

That is, what it “means” for a graph to get broken is precisely that “the vertex that receives A's source” is *different* from “The vertex that is the source of A's image.”

Thus we can actually DEFINE what we mean by a graph injection as a square in which all arrows have closed loops going either way around.

```{image} assets//Ch3/InjectionFadethrough.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

Of course, the same maps must also have closed loops for the arrows and their targets. Together, all of the “data” describing this embedding is caputred in the following diagrams:


```{image} assets//Ch3/GraphInjection.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

We have defined one thing – graph connectivity in injections – in terms of another thing – closed loops. Imposing the rule that the certain loops must close is called a "commutativity condition".

It turns out there are a lot of ideas that can be captured by connecting maps together and then declaring some commutativity conditions. Commutative diagrams are the bread and butter of category theory. We will not explore the general notion of commutativity in much depth here. For a thorough yet elementary introduction to this topic we recommend Lawvere and Schanuel's *Conceptual Mathematics*.[^1]

[^1]: Lawvere, F. W.; Schanuel, S. H. (2009). Conceptual Mathematics: A First Introduction to Categories (2nd ed.). Cambridge: Cambridge University Press. https://doi.org/10.1017/CBO9780511804199


As we'll see, one of the benefits of describing injections and dangling edge conditions in terms of maps is that maps and commutativity conditions are easy for the computer to understand maps and inspect. This allows the computer a way to be of use to us without that computer needing to have any actual understanding ideas like vertices, arrows, attachments..the semantics of graphs.


## 2. Graph Morphisms


```{image} assets//Ch3/ChoreHanded.png
:alt: Whoopsy!
:width: 500px
:align: center
```

```{image} assets//Ch3/EmbeddingHanded.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

```{image} assets//Ch3/EMBED.gif
:alt: Whoopsy!
:width: 500px
:align: center
```



## Footnotes and References







