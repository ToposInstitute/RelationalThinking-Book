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

For any given a schema there will be many possible instances. Below we show a few of the instances for each of our three graph schemas.

```{image} assets/Ch3/ThreeSchemas.png
:alt: Whoopsy!
:width: 800px
:align: center
```
<br>

Of course, each of these schemas actually have an *infinite* number of possible instances (graphs can be arbitrarily big!). It isn't practical to show _every_ instance, but it is possible to just imagine them all floating in space, one endless swarm of things the schema might refer to. 

```{image} assets/Ch4/InstanceDrift.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

<br>

In our ongoing quest to think about things in terms of relationships we should look at these instances, each floating in isolation from the others, and ask ourselves, "How are these instance related to one another?"

In this chapter we will develop a powerful answer to this question. 

Let's set this swarm of thought bubbles aside for now and return to it at the end, armed with a new understanding of how its instances are related. At that point we will be ready to move up the fourth rung or in our ladder of abstactions (our last, mercifully!), from schemas to categories. 

:::{attention}
Hang in there! You're almost at the top of the ladder.
:::


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

This is what we might call a "procedural" way of thinking about the Summer and Winter maps; you imagine step-by-step procedures that could be applied to transform one map into the other. As we make our way towards "relational" thinking, we are going to recommend another way of looking at these two graphs.

Note how the Summer graph is basically contained inside of the Winter graph. For the Summer graph to be a "part" of the Winter graph means, in effect, that we can map the Summer graph *into* the Winter graph, taking vertices to vertices and arrows to arrows.

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


///SAY SOMETHING ABOUT THE 1-1 NATURE OF THE MAPS!!!

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



IMAGES DEALING WTIH MORPHISM WITH DOUBLED ARROWS




It may seem like "crushing" a directed graph this way would be an undesirable outcome. But this actually turns out to be a common thing to do when working with graphs. Crushing is different from breaking. The dangling edge condition is a problem for modeling because it renders any underlying model meaningless. But collapsing or merging the parts of a graph doesn't actually pose any danger to the integrity underlying model, as the following example will illustrate.


In Chapter 1 we saw an example of a directed graph which described who's turn it was to do the dishes. In this account, Paul and Toni originally took turns. Then, eventually, their new roommate Tuco moved in and took over dishes duty.

As it turns out, Paul and Toni are both right handed while Tuco is left handed. Thus, there is another directed graph which also accurately describes the chore progression, but now in terms of the handedness of dish-washer.


```{image} assets//Ch3/ChoreHanded.png
:alt: Whoopsy!
:width: 500px
:align: center
```


The second model is a "coarse grained" version of the first. It is consistent with the first model but contains fewer details (*ie* it does not tell us which right handed person is doing the dishes). This course-graining is exactly what is captured in a graph morphism.  


```{image} assets//Ch4/RightiesVSLefties.gif
:alt: Whoopsy!
:width: 800px
:align: center
```




////GRAPHIC SHOWING RULES ABOUT WHAT CAN AND CANNOT BE MAPPED




### Puzzles


:::: {admonition} Puzzle 1

We have seen that Graph 1 can me mapped into Graph 2 with the following injection:

```{image} assets/Ch3/DefineInjection.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

How many other morphisms are there from Graph 1 to Graph 2? 

We've programmed this problem into the executable code below. When you think you know the answer, execute code and see if you and AlgebraicJulia agree.

::::

+++

```{code-cell}
using Catlab.CategoricalAlgebra, Catlab.Graphs, Catlab.Graphics

Graph1 = Graph()
add_vertices!(Graph1, 2)
add_parts!(Graph1, :E, 2, src=[1,2], tgt=[2,1])


Graph2 = Graph()
add_vertices!(Graph2, 3)
add_parts!(Graph2, :E, 4, src=[1,2,2,3], tgt=[2,3,1,3])

countTheMorphisms = length(homomorphisms(Graph1, Graph2))
```

+++

:::{admonition} Answer
:class: dropdown
There are three distinct morphisms from Graph 1 to Graph 2; two injections and one way of collapsing the whole graph down to one vertex.

As a human, you look for the answer to this puzzle by reasoning about the shape of the directed graph. AlgebraicJulia looks for its answer by trying to count all of the pairs of vertex maps and arrow maps which complete the commutative squares in the graph morphism schema. They are very different approaches but they arrive at the same answer.

:::


:::: {admonition} Puzzle 2

How many ways can this map into that?

::::

+++

```{code-cell}

Graph3 = Graph()
add_vertices!(Graph3, 2)
add_parts!(Graph3, :E, 2, src= [1,1],tgt= [1,2])

Graph4 = Graph()
add_vertices!(Graph4, 4)
add_parts!(Graph4, :E, 5, src=[1,1,1,1,4], tgt=[1,2,3,4,4])


countTheMorphisms = length(homomorphisms(Graph 3, Graph 4))
```

+++




:::: {admonition} Puzzle 3

How many ways can this triangle be mapped into this hexagon?

::::




+++

```{code-cell}

hexagon = Graph()
add_vertices!(hexagon, 6)
add_parts!(hexagon, :E, 6, src=[1,2,3,4,5,6], tgt=[2,3,4,5,6,1])

triangle = Graph()
add_vertices!(triangle, 3)
add_parts!(triangle, :E, 3, src=[1,2,3], tgt=[2,3,1])

countTheMorphisms = length(homomorphisms(triangle, hexagon))
```

+++


:::{admonition} Answer
:class: dropdown
Trick question! There are now ways of mapping the triangle into the hexagon.

:::

:::: {admonition} Puzzle 4

What about the other way around? How many ways can this hexagon be mapped into this triangle?

::::

+++

```{code-cell}

countTheMorphisms = length(homomorphisms(hexagon, triangle))
```

+++



:::{admonition} Answer
:class: dropdown
Three!

:::

:::: {admonition} Puzzle 4

This puzzle is the same as puzzle 2, except we now think of these as reflexive graphs. This changes the answer!

::::


+++

```{code-cell}

Graph5 = ReflexiveGraph()
add_vertices!(Graph5, 2)
add_parts!(Graph5, :E, 2, src= [1,1],tgt= [1,2])

Graph6 = ReflexiveGraph()
add_vertices!(Graph6, 4)
add_parts!(Graph6, :E, 5, src=[1,1,1,1,4], tgt=[1,2,3,4,4])


countTheMorphisms = length(homomorphisms(Graph 5, Graph 6))
```

+++


:::{admonition} Answer
:class: dropdown
Five.

Note how Algebraic Julia is able to specialize to the case of Reflexive graphs....something something.

:::





## 4.4 The category of instances

We're now ready to move up the last rung in our ladder of abstractions, from "blueprints" to "categories!"

Look closely at this instance of our graph morphism schema:

```{image} assets/Ch4/MorphismInstance.gif
:alt: Whoopsy!
:width: 800px
:align: center
```

Note how the top of this square contains the data for graph 1 and the bottom is the data for graph 2. The _overall_ blueprint represents a morphism of graph 1 into graph 2. As long as they satisfy all the closed loop conditions, any collection of maps in this pattern describe _some_ graph morphism.

Our final abstraction will be a directed graph in which the vertices are instances and the arrows are morphisms between the corresponding graphs.

// IMAGE OF THOUGHT BUBBLE WITH ARROW SAYING "MORPHISM OF GRAPHS"


:::{attention}
We know what you're thinking! "_Another_ directed graph?! At _another_ level of abstraction?!" 

This is the last one. 

Promise.
:::





* Injections are hooked arrows
* Isomorphisms are paired unidirectional bars









```{image} assets/Ch4/MorphismArrow.gif
:alt: Whoopsy!
:width: 800px
:align: center
```





```{image} assets/Ch4/CategoryDrift.gif
:alt: Whoopsy!
:width: 800px
:align: center
```









## Footnotes and References









