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

# Chapter 4: Categories
## 4.1 Introduction

Given a schema, it will have many possible instances. We can imagine this set of all theses instances as a swarm of thought bubbles - all the graphs this schema might refer to. 

IMAGE OF DIFFERENT SCHEMAS, EACH WITH ITS OWN SWARM OF THOUGHT BUBBLES.


In our ongoing quest to think about things in terms of relationships we can ask, "How are these instance related to one another?"

In this chapter we will define a useful way of relating instances of a schema. Once we do, we will be ready to move up the last rung or in our ladder of abstactions, from schemas to categories. Yet another kind of directed graph at yet another level of abstraction (our last, mercifully!).

```{image} assets/Ch4/InstanceDrift.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

## 4.2 Graph Injections

### Graph injections, concretely

In Chapter 1 we saw an example of a directed graph which described the layout of a ski resort. In this account, you could ride the ski lift up and down the mountain or ski from the top of the mountain into a nearby village.

But suppose we visit this ski resort in the Summer when there's no snow. At this time of year the resort lets guests ride up and down the mountain in the lift (which provides a *lovely* view!), but there is no way to get down the mountain into the village without a snow pack to ski on.

Let's compare the summer and winter maps for the ski resort.

```{image} assets/Ch3/SummerWinter.png
:alt: Whoopsy!
:width: 800px
:align: center
```

One way to think about the relationship between these two graphs is that we can turn one into the other. Given the Summer map I can _turn it into_ the winter map by adding the additional vertices and arrows associated with travelling to the village. On the other hand, given the Winter map, I can create the Summer map by _deleting_ the arrows and vertices associated with the village.

This is a procedural way of looking at the Summer and Winter maps, a procedure that could be applied to transform one into the other.

In our ongoing quest to think about things in terms of their relationships instead of in terms of the things themselves, we are going to recommedn another way of looking at these two graphs. Note how the Summer graph is basically contained inside of the Winter graph. How should we characterize this relationship? For the Summer graph to be a "part" of the Winter graph means, in effect, that we can map the Summer graph *into* the Winter graph, taking vertices to vertices and arrows to arrows.

```{image} assets/Ch3/InjectSki.gif
:alt: Whoopsy!
:width: 800px
:align: center
```
<br>

We call this a "graph injection". In order to visualize injections clearly let's introduce some new visuals.

Let's step away from the ski lodge and look at graph injections more generally. We want to represent the injection of graph 1 into graph 2:

First, let's redesign graph 2 to be a "recipient" of the graph 1, moving the colors and shapes out of the way.

IMAGE DETAILING THIS FOR VERTICES AND ARROWS

The process of placing the first graph inside of the second graph now looks like this:

```{image} assets/Ch3/DefineInjection.gif
:alt: Whoopsy!
:width: 800px
:align: center
```
<br>
The final result gives us an complete picture of "what went where" in our injection. We sometimes call this the "image" of the first graph in the second.

<br>
<br>

An important detail about graph injections is that a graph is not allowed to come apart when getting injected. An arrow must stay attached to its source and target vertices through this process in order to avoid the dangling edge condition and preserve the integrity of any underlying model.


```{image} assets/Ch3/DanglingEdge.png
:alt: Whoopsy!
:width: 800px
:align: center
```




### Puzzles

1. How many ways can you inject this upper graph into the lower graph?

```{image} assets/Ch3/Problem1.png
:alt: Whoopsy!
:width: 500px
:align: center
```
Answer:

Three! The upper triangle can be injected in any orientation.
```{image} assets/Ch3/Problem1Solution.png
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


:::{admonition} Pause and Ponder! 
What strategies did you use to count the injections? Were you systematic or did you do it through trial and error? Could you design an algorithm that a computer could use to solve these problems for you?
:::



### Graph injections as data


What is the data that defines a graph injection? We can actually capture all of the important details with a pair of maps. The first is a "vertex map", connecting each vertex in graph 1 with its destination vertex in graph 2. The second is an "arrow map" which identifies where the arrows from graph 1 land inside the arrows of graph 2.

```{image} assets//Ch3/A,V,Maps.gif
:alt: Whoopsy!
:width: 800px
:align: center
```
<br>
These two maps capture all the relevant information about the injection.
<br>
<br>

Recall our dangling edge condition: For these maps to represent a proper injection the arrows of the injected graph have to stay attached to their source and target vertices. Clearly these maps must be coordinated in some way to prevent breakage. What conditions must be true in order to ensure that a graph doesn't come apart on entry?

To answer this, let's arrange our maps into a square, with the source maps for graph 1 and graph 2 running horizontally and the “arrow map” and “vertex map” running vertically:


```{image} assets//Ch3/CommSquare.gif
:alt: Whoopsy!
:width: 800px
:align: center
```
<br>
Note how the dashed lines seem to flow “out” from the arrows in the upper left and flow “in” to the vertices at the lower right. Starting from any arrow in the upper right, there are two paths you can take around this square: an upper route and a lower route. Let's focus on a specific arrow and consider what these routes “mean”. (We'll label the endpoints as "A" and "V")






:::{admonition} Upper route


```{image} assets//Ch3/UpperRoute.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

Reading across the top we have that the red arrow has the heart as its source. Reading down the right side we see that the heart gets sent to the square vertex in graph 2. So the square is "the vertex that receives A's source."

:::


:::{admonition} Lower route

```{image} assets//Ch3/LowerRoute.gif
:alt: Whoopsy!
:width: 500px
:align: center
```

Reading down the left we have that the red arrow from graph 1 gets sent to the blue arrow from graph 2 (In other words, the blue arrow is its "image"). Reading across the bottom we see that the blue arrow has the square vertex as its source. So the square is "the source of the image of A."

:::





To complete the picture, let's now revisit to our dangling edge condition. “Coming apart” means, literally, that a vertex and an arrow that were connected in graph 1 are not connected when they land in graph 2. Suppose an arrow gets separated from its source by an attempted injection. For that arrow, the two routes around the square will look something like this:


```{image} assets//Ch3/OpenSquare.gif
:alt: Whoopsy!
:width: 500px
:align: center
```
<br>

That is, what it “means” for a graph to get broken is precisely that “the vertex that receives A's source” is *different* from “the vertex that is the source of A's image.”

So we can actually DEFINE a graph injection with a closed loop condition: starting from any arrown in the upper left, the paths going either way around the square will always form a closed loop.

```{image} assets//Ch3/InjectionFadethrough.gif
:alt: Whoopsy!
:width: 500px
:align: center
```
<br>

Of course, these same arrow and vertex maps must also have closed loops for the *target maps*. All together, this is the complete set of “data” describing the injection:


```{image} assets//Ch3/GraphInjection.gif
:alt: Whoopsy!
:width: 800px
:align: center
```
<br>
<br>


### Graph injection schema


```{image} assets/Ch4/MorphismInstance.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

Note how the top of this square contains the schema for Graph 1, the bottom is the schema for graph 2, and the overall square schema represents a morphism of graph 1 into graph 2 _if and only if_ these maps satisfy the closed loop condition.

The upper part of the diagram describes a graph. The lower part of the diagram describes another. And any pair of vertical arrows that satisfy the commutativity constraint describe a way of morphing the first graph into the second.

For our schema, we will impose this closed loop condition by writing it as an equation. We can describe the two paths around the schema in writing by listing the sequence of chunky arrows along each path, `src 2`•`arr` for the lower route and `vert`•`src 1` for the upper route. And similarly for the targets.

```{image} assets/Ch4/GraphMorphismSchema.jpg
:alt: Whoopsy!
:width: 800px
:align: center
```



In this chapter and the last we have seen that lot of ideas that can be captured by connecting maps together and then declaring some commutativity conditions. Commutative diagrams are the bread and butter of category theory. We will not explore the general notion of commutativity in much depth here. For a thorough yet elementary introduction to this topic we recommend Lawvere and Schanuel's *Conceptual Mathematics*.[^1]

[^1]: Lawvere, F. W.; Schanuel, S. H. (2009). Conceptual Mathematics: A First Introduction to Categories (2nd ed.). Cambridge: Cambridge University Press. https://doi.org/10.1017/CBO9780511804199


In this section we have looked at one kind of relationship between graphs - the ways one graph can be injected into another - and we have characterized that relationship in terms of schemas and commutivity conditions. However, there is more to see. It turns out the this schema doesn't just capture injections!


## 4.3 Graph Morphisms

### General morphisms

The vertex and arrow maps for our graph injections were all one-to-one. Each vertext in graph 1 was sent to a unique vertex in graph 2, and similarly each arrow was sent to a unique destination.

Consider the following source map. First of all, we can see that it satisfies out commutativity condition, in that all squares from the upper left are closed.


```{image} assets//Ch3/EMBED.gif
:alt: Whoopsy!
:width: 500px
:align: center
```
But notice that the vertical maps _merge_ vertices and arrows in the image. If we show the result of this map we see that arrows and vertices get doubled up in the image:

IMAGE OF GRAPH MORPHISM WITH DOUBLED ARROWS

In general, any collection of maps satisfying our schema are called "graph morphisms." Graph injections are a special case of graph morphisms in which teh arrow and vertex maps are one-to-one.


In Chapter 1 we saw an example of a directed graph which described who's turn it was to do the dishes. In this account, Alice and Bob originally took turns. Then, eventually, their new roommate Tuco moved in and took over dishes duty.

As it turns out, Alice and Bob are both right handed while Tuco is left handed. Thus, there is another directed graph which also accurately describes the chore progression, but now in terms of the handedness of dish-washer.


```{image} assets//Ch3/ChoreHanded.png
:alt: Whoopsy!
:width: 500px
:align: center
```


The second model is a "course grained" version of the first. It is consistent with the first model but contains fewer details (ie it does not tell us which right handed person is doing the dishes). We can describe this course grained relationship in terms of closed loops as well.

Let's construct a map that connects the Alice, Bob and Tuco (the vertices of the top directed graph) with their handedness (the vertices of the lower graph).

Let's also construct a map of arrows --- WORK THIS OUT: each arrow in the top means "day after Alice" "day after Bob" ; each arrow on the bottoms means "day after righty" "day after lefty"

Show that these are vertex and arrow maps that satisfy our same loop condition from injections

//GIF of fadethroughs on source map.

The difference is that these maps collapse vertices and arrows to the same target. We would represent this like so:

//Image 

In injections, the maps are one-to-one. They are a special case of these more general graph morphisms.

It's important to note that when we collapse the graph like this it remains intact "inside" of the other. We have not broken the dangling edge condition.



```{image} assets//Ch3/EmbeddingHanded.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

### Puzzles

## 4.4 The category of instances





```{image} assets/Ch4/CategoryDrift.gif
:alt: Whoopsy!
:width: 800px
:align: center
```









## Footnotes and References









