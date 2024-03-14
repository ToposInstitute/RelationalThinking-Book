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

# Chapter 5: Graph Gluing

:::{attention}
This chapter will engage you in deep relational thinking!
:::

In the previous chapters, we have seen that graphs are a quite simple and powerful tool to model relationships between various entities. We also learnt the idea of identifying one graph inside another via a graph embedding. A graph embedding may coarse-grain the information in the domain graph by sending two different vertices/edge of the domain to the same vertex/edge in the codomain but it always **preserves the connectivity** of the domain.

In this chapter, we are interested in combining two graphs, like adding two numbers (if this analogy sounds odd to you, towards the end of this chapter we will see that adding numbers is same as combining two discrete graphs). The goal of combining graphs is to bridge islands of connections allowing for information to flow between the graphs (via common channels) and for expression of new meanings.

As a step towards making sense of the idea of combining graphs, let us a revisit a graph that we met in the first chapter — “Whose turn is it to do dishes?”.

![whoops!](./assets/Ch3/DGdishes.jpg)


In this graph, Paul claimed to be friends with Tuco (to our unaware readers, Tuco is Paul’s neighbor’s cat). Paul is also a friend of Brendan, Angeline and myself (co-authors of this book). Encoding these friendships as graphs, we got:

```{image} assets/Ch3/Tuco.jpeg
:alt: Whoopsy!
:width: 560px
:align: center
```

Read each edge in the above graphs as “is a friend of”. Combining the above two friendship graphs along the common vertex Paul into a single friendship graph, we get:

```{image} assets/Ch3/Tuco-glued.jpeg
:alt: Whoopsy!
:width: 560px
:align: center
```

We see new information emerging in the combined graph. Tuco “is a friend of a friend” for Brendan, Priyaa, and Angeline. If we assume that a friend of a friend is a friend, then one can infer that Tuco “is a friend of” Brendan, Angeline and Priyaa. The connections in the combined graph supports such assumptions and inferences.

The above example is deceivingly simple. By eyeballing the two friendship graphs, it is easy to see by common experience how the graphs can be combined. But, graphs generated in the real world are quite complex. In practice, combining such graphs using pen and paper is out question! However, it is the same common sense in play combining even the most complex graphs.

The goal of this chapter is to make our inherent sense of combining graphs more explicit and fun! Once explicit, we all can agree unambiguously what does it mean to combine two graphs together. We will achieve this goal by playing a game of dumb charades of graphs using memes from relational thinking, also called as category theory by some! By playing this game, we focus is on “what” (a combined graph is) rather than “how” (to combine graphs).

The challenge of this game is to communicate “the connectivity of any combined graph” without explicitly saying what the graph is, so that the other person will know exactly what the graph is! We said that we are going to use memes from relational thinking to play this game. Sections 2 and 3 will introduce two such memes from relational thinking.

## 5.1 Commuting diagrams

You have met this meme in Chapter 3 when Paul introduced you to graph embeddings. Do you recollect this animation from Chapter 3?

![whoops!](./assets/Ch3/FullHomomorphism.gif)

This animation is an instantiation of two commuting diagrams which define a graph embedding.

We know that a graph is a purposeful (constrained) relationship between edges and vertices. This relationship is a pair of maps: a source and a target map from the set of edges to the set of vertices. A graph embedding is a purposeful relationship between two graphs. This relationship is also a pair of maps: a vertex map between vertices of the graphs and an arrow map between the edges of the graphs. What makes a graph embedding purposeful is its harmonious behavior with the existing source and the target maps in the domain and the codomain. This harmony is precisely encoded as a pair of commuting diagrams.

Let us suppose we have two graphs and a graph embedding between them. So we have the following system maps:

```{image} assets/Ch3/2-commuting-diagram-1.png
:alt: Whoopsy!
:width: 450px
:align: center
```

The following two commuting diagrams of graph embedding tells us how these system of maps play with each other. The first diagram tells that the source of each edge in Graph-1 is preserved by the embedding. 

```{image} assets/Ch3/2-commuting-diagram-2.png
:alt: Whoopsy!
:width: 400px
:align: center
```

The second diagram tells that the target of each edge in Graph-1 is preserved by the embedding.

```{image} assets/Ch3/2-commuting-diagram-3.png
:alt: Whoopsy!
:width: 400px
:align: center
```

Hey, did you notice that commuting diagrams look like directed graphs?! But they are very specific sort of graphs - they have an *origin* vertex which has only outgoing arrows (Edges of Graph-1), and a *destination* vertex which has only incoming arrows (Vertices of Graph-2). Hence, these graphs are non-cyclic. One follows any arrow from the origin to reach the destination. There are multiple choices of paths to take from the origin (two paths in each of the above diagrams). The word commuting refers to the fact that, for an edge in “Edges of Graph-1”, following any of the path, will lead to the same vertex in “Vertices of Graph-2”. In a commuting diagram, all the paths are practically the same. Commuting diagrams signify diversity of paths but unity of purpose.

Let us see have a closer look at the first commuting diagram.

```{image} assets/Ch3/2-commuting-diagram-4.png
:alt: Whoopsy!
:width: 450px
:align: center
```

For any edge in Graph-1, following ‘Path 1’ will lead to the image of its source vertex in Graph-2. For the same edge, following ‘Path 2’ leads to the source vertex of its image in Graph-2. The commuting diagram ensures us that both these vertices are the same. Interpret the second diagram similarly. The commuting diagrams convey that the arrow and the vertices maps are *aware* of the source and the target maps of Graph-1 and Graph-2.

Note that there can be any number of graph embeddings between Graph-1 and Graph-2. But, all of them follow the commuting diagrams.

Commuting diagrams are more general than for just graph embeddings. Commuting diagrams are of any closed shape like a triangle or a square and are non-cyclic. The arrows of a commuting diagram can belong to any rung: do you remember this picture from Chapter 1?

```{image} assets/Ch3/Ladder.png
:alt: Whoopsy!
:width: 400px
:align: center
```

The above commuting diagrams (sort of belongs to rung 3). The vertices are sets (sets of vertices and sets of edges) and the arrows are set functions.

A commuting diagram in rung 4 may look like this:

```{image} assets/Ch3/2-commuting-diagram-5.png
:alt: Whoopsy!
:width: 400px
:align: center
```
Graph 1 is the origin and Graph 3 is the destination. The above diagram says that embedding the choices of paths to embed Graph-1 inside Graph-3 are exactly the same. But what does this sameness mean? To make this answer straightforward, let us color the embeddings:

```{image} assets/Ch3/2-commuting-diagram-6.png
:alt: Whoopsy!
:width: 400px
:align: center
```
We know that each embedding has an arrows map and vertices map. The commuting diagram tells us that:

1.  For any vertex in Graph-1 (drawn as the little blue ball), applying the green embedding to it produces a vertex in Graph-3 (green circle surrounding the black circle). Or applying the red embedding produces a vertex in Graph-2 (red circle surrounding the black circle); applying the yellow embedding to this vertex produces a vertex in Graph-3 (yellow circle surrounding the red circle). Because the diagram commutes, both the vertices in Graph-3 are the same.

```{image} assets/Ch3/2-same-vertex-2.png
:alt: Whoopsy!
:width: 350px
:align: center
```

2.  For any edge in Graph-1 (drawn as a black line), applying the green embedding, or applying red embedding and then a yellow embedding (on the edge produced by the red embedding) will produce the same edge in Graph-3.

```{image} assets/Ch3/2-same-edge.png
:alt: Whoopsy!
:width: 350px
:align: center
```

## 5.2 When are two graphs the same?

We already met the idea of “sameness” in the previous section. In our every day conversations, we use the word “same” quite often — 

Brendan eats the same breakfast everyday (probably)! 

Steve Jobs wore the same outfit everyday! 

The word “same” has origin in the Sanskrit word “*sama*” which (sort of) translates to “equal”. The word “sama” invokes an image of a balanced weighing scale in my mind. Growing up in India, it was a common sight for me seeing shopkeepers weighing their produce using such scales.

```{image} assets/Ch3/3-balance.png
:alt: Whoopsy!
:width: 350px
:align: center
```

The idea of “sameness” or “sama” is that in certain context, two distinguishable objects are considered to be equal or identical — their meaning is considered to be the same. Under the context of color, all the outfits of Steve Jobs were identical / equal (can't be told apart). Under the context of recipe, the breakfast Brendan eats are identical / equal (can't be told apart). 

The ability to blur the lines between the meaning of objects and group them into one, by placing them in a certain context is quite natural to human thinking and is also useful. Imagine a world without weighing scales which is a practical demonstration of “sameness” (under the context of weight)! The concept of “sameness” is an instance of relational thinking in everyday life. 

When someone speaks of sameness of two things, there exists an (unspoken) context hidden in it. The idea of sameness is manifestation of this unspoken context. Relational thinking (in mathematical sense) supports making this context explicit thereby eliminating the room for arguments if two things are same or not. (Eugenia Cheng has explained this very beautifully. I think a reference to her explanation or a video embedding will be very helpful.)

In this section, we want to explore the “sameness of graphs” through the lens of relational thinking. The question of finding out if two graphs are the same is an area of study by itself and beyond the scope of this book. We are more interesting in making explicit the relationship that makes two graphs the same.

Let us play a simple game. Among these three graphs, circle the two graphs which you “think” are the same:

```{image} assets/Ch3/3-same-graphs-1.jpeg
:alt: Whoopsy!
:width: 350px
:align: center
```

Did you circle (b) and (d)? Can you say out loud — 

1. Why did you not circle (a) and (c)? 
    
    [ They are not the same because (c) has more arrows than (a). ]
    
2. Why did you not circle (a) and (b)? 
    
    [ Even though these two graphs have the same number of edges, there seems to be a mismatch in how the vertices are connected! ]
    
3. Why did you circle (b) and (d)? (reshape one into another)
    
    [ They both contain precisely the same connectivity. Because of this, (b) can be embedded inside (d) covering the whole graph. (d) can be embedded inside (b) covering the whole graph! No loss of information occurs in either direction of embedding.]
    
    Let us make the “no loss of information” more precise. Saying *two graphs are the same* amounts to providing two embeddings between them as shown below. Take any vertex in (b). Follow it through the red followed by the green embedding. You will reach the same vertex you started at. Take any vertex in (d). Follow it through the green followed by the red embedding. You will reach the same vertex you started with. Similarly for the edges.

```{image} assets/Ch3/3-same-graphs-2.jpeg
:alt: Whoopsy!
:width: 350px
:align: center
```

Before I end this section, I would like to entertain you with a personal story tangentially related to graphs and the idea of sameness! When we were young, my brother and I were very fond of “Maggi noodles”.  My mom considered Maggi noodles to be both unhealthy and expensive. But, because we were fond of it, she would occasionally bring home two packets of these noodles. It was brother’s task to cook these noodles. After cooking, he would spilt the noodles into two portions and I would be given a chance to pick one I wanted. I always had great trouble in making up my mind, since whichever portion I picked, mine looked lesser to me. Often, I would ask to change since my brother’s portion looked bigger. My mom with great annoyance would reply, “Both the portions are the same! It's your eyes which are big!”. 

```{image} assets/Ch3/maggi.png
:alt: Whoopsy!
:width: 150px
:align: center
```

## 5.3. Combining graphs using memes

Having explored the idea of commuting diagrams and sameness, we are now ready to play our game of dumb charades of graphs! The goal of the game is to describe the combined graph of any two (overlapping) graphs without saying what the vertices, edges, source and target maps of the combined graph are. Let’s name this graph as the “***colimit graph***”. 

In this game, all we know is that we are looking for a specific graph.  We will narrow down our search step by step by expressing requirements of the colimit graph using the memes we have learnt, until we end up with the colimit we are looking for. Let us begin!

### A. Specifying the shape of the overlap

To combine two graphs, we first need to know which vertices and edges are common to both the graphs. Recollect that the “Paul” vertex was common to both the friendship graphs we met early in the chapter. Let’s call the common vertices and edges as the ***overlap*** of the two graphs. So, to combine two graphs, we need to know their overlap. 

In chapter 3, we saw that a graph embedding identifies one graph inside another.  Let us suppose we have three graphs, and two morphisms as shown in the diagram below. Let us not worry what exactly Graph-1, Graph-2, Graph-3, because we are concerned more concerned with how they are related to each other. We are concerned with the shape of the diagram  created by these relationships rather than what are the actual graphs inside these colored rectangles. Think of the rectangles as placeholders that can receive any graph in them.

```{image} assets/Ch3/4.3-Diagram.png
:alt: Whoopsy!
:width: 350px
:align: center
```

The relationships between the graphs in the above diagram are: 

- Graph-1 embeds inside Graph-2.
- Graph-1 embeds inside Graph-3.
- Thus, Graph-2 and Graph-3 overlap in the parts where Graph-1 is embedded.

The “overlap” acts as a bridge between two graphs allowing one to navigate from one graph to the other via common vertices and edges. 

Theoretically, this means that we can combine Graph-2 and Graph-3 into a single graph. In this graph, we move from the vertices of the Graph-2 to the vertices of Graph-3 via the overlap. This idea of a combining graphs can be loosely illustrated as sticking together two sheets of paper using glue.

```{image} assets/Ch3/sheets.jpeg
:alt: Whoopsy!
:width: 250px
:align: center
```

Another analogy that comes to mind for combining graphs is glueing a broken handle to a tea cup using two pieces of glue. 

```{image} assets/Ch3/cup-handle.jpeg
:alt: Whoopsy!
:width: 450px
:align: center
```

With this analogy in mind, let us think of the colimit graph of the diagram showed in Figure 1. The diagram in Figure 1 has all the information about the graphs to be combined and their overlap. Hence, we say “colimit graph of a diagram” similar to how we say “sum of two numbers”.  

We now know what it means for two graphs to overlap — it is a diagram of shape in Figure 1. We are also sort of convinced that two graphs can be combined along their overlap. 

```{admonition} Points to ponder
:class: dropdown

What would it mean to have Graph-1 to be empty (no vertices and no edges) in Figure 1? What will the “glued object” be if there is no glue?
```

### B. Combining Graphs along overlap

Now that, we know the overlap, the next step in this game is to narrow down the candidates for the colimit graph (for the diagram in Figure 1). Since we cannot talk about the colimit graph (individually) in terms of its source and target maps, we will look at the colimit graph from the perspective of other graphs and gather all that knowledge. This perspective is encoded as a relationship between a graph and the colimit graph. The immediate candidates to gather perspective are Graphs 1, 2, and 3. 

How is the colimit graph is expected to be related to Graph-1, Graph-2, and Graph-3? We invite the reader to take a moment to ponder over this question using the picture below.

```{image} assets/Ch3/4.2-commute-1.png
:alt: Whoopsy!
:width: 450px
:align: center
```

Here are a few possible relationships. Since the colimit graph is given by glueing Graph-2 and Graph-3 along the shape of Graph-1, at the least, 

1. Graph-2 must embed in the colimit graph (the cup is in the glued object). 
2. Graph-3 must embed in the colimit graph (the handle is in the glued object). 

Let us add these relationships in the above diagram. Each arrow is a graph embedding:

```{image} assets/Ch3/4.2-commute-2.png
:alt: Whoopsy!
:width: 450px
:align: center
```

(Does this shape look familiar? Does it remind you of commuting diagrams we met in section 2?)

Let us now shift our attention to the relationship between Graph-1 and the colimit graph.

First of all, does Graph-1 embed inside the colimit graph? 

It does! Since Graph-1 embeds in Graph-2, and Graph-2 embeds in the colimit graph, Graph-1 embeds in the colimit graph via Graph-2. Similarly, Graph-1 embeds in Graph-3, and Graph-3 embeds inside the colimit graph. Through these relationships too, Graph-1 embeds in the colimit graph. Thereby, we have two choices of embeddings of Graph-1 in Graph-3: 

So, Graph-1 can be embeds the combined graph in two ways: 

- either through Graph-2,
- or through Graph-3.

Which of these should we pick to identify Graph-1 inside the combined graph? What is the guiding to principle to pick one relationship over the other? (Time to ponder!) 

The answer is that the choice of relationships MUST NOT matter!! Irrespective of the route of embedding, Graph-1 must embed precisely in the same location in the colimit graph because it's the overlap. In the cup-handle analogy, this amounts to saying that the points in the cup and the points in the handle which are glued together are indistinguishable in the glued object. The rest of the points can be distinguished as belonging to the cup or to the handle. 

Saying that “ the choice of relationships MUST NOT matter” is same as saying that “the above diagram commutes”! Irrespective of the two routes chosen  from the Graph-1 (origin), one will always end up in the same location (subset of vertices and edges) in the colimit graph (destination). 

That completes our perspectives of the colimit graph from Graphs 1, 2 and 3. 

The first set of requirements of the colimit graph are that: 

1. Graph-2 must embed in the colimit graph. 
2. Graph-3 must embed in the colimit graph. 
3. Graph-1 embeds in colimit graph, however, the choice of path MUST NOT matter!

:::{admonition} Exercise
:class: dropdown

Find at the least two graphs and the two embeddings which will make a commuting diagram. 

(a diagram)

:::{admonition} Solution 
:class: dropdown

Possible solutions 

:::

### B. Combining Graphs along overlap

Now, there can be many graphs satisfying the above requirements. Let us make this concrete by stepping outside the game for a moment and instantiating Graph-1, Graph-2 and Graph-3.

```{image} assets/Ch3/4.3-Graph1-3.png
:alt: Whoopsy!
:width: 450px
:align: center
```

Graph-1 embeds in Graph-2 and Graph-3 as follows:

```{image} assets/Ch3/4.3-Diagram.png
:alt: Whoopsy!
:width: 450px
:align: center
```

A few possible choices of colimit graphs of the above diagram satisfying the requirements of Graph 1-3 are: 

```{image} assets/Ch3/4.3-a.png
:alt: Whoopsy!
:width: 450px
:align: center
```

</br>

```{image} assets/Ch3/4.3-b.png
:alt: Whoopsy!
:width: 450px
:align: center
```

</br>

```{image} assets/Ch3/4.3-c.png
:alt: Whoopsy!
:width: 450px
:align: center
```

</br>

```{image} assets/Ch3/4.3-d.png
:alt: Whoopsy!
:width: 450px
:align: center
```

We invite the reader to to have a careful look at each choice and convince oneself that each of these diagrams commute. 

Which one the graphs inside the yellow boxes will be your choice of the colimit graph?

We want the colimit graph to be “the most natural choice” among all the choices of graphs. By most natural, we mean a choice which will follow “the principle of least effort (to integrate)” or the “path of least resistance” (as nature does). 

**In the above graphs what is your most natural choice for the colimit graph and why?** 

While I will reveal my choice in a moment, let us have a look at each of the possible choices of colimit graphs. 

- In (a), except for the overlaps, the vertices and edges from Graph-2, and the vertices and the edges from Graph-3 are clearly distinguishable. The information of Graph-2 and Graph-3 remain intact (except at the overlap).
- In (b) and (c), the boundaries of Graph-2 and Graph-3 fade beyond the overlap region.
- Choice (d) represents maximal collapse of information - all the vertices and edges from Graph-2 and Graph-3 are squeezed into a single vertex and edge in the possible graph.

 Choice (a) has the feeling of the most lazy choice since Graph-2 and Graph-3 are kept apart as they are except in the overlap region. It does nothing extra like coarse-graining information like (b), and (d) or add extra information like (c). Indeed (a) is our choice! 

We shall now make our intuition of “least effort” precise by asking how choice (a) relates to all the other possible candidates! In other words, the criteria to narrow down a choice of “least effort” from all possible potential candidates.

**Let us compare choice (a) with choice (b)**

```{image} assets/Ch3/4.3-compare-a-b-1.png
:alt: Whoopsy!
:width: 450px
:align: center
```

We have two commuting diagrams with Embedding A and Embedding B as common arrows. Next we would like to see how (a) and (b) are related. 

```{image} assets/Ch3/4.3-compare-a-b-2.png
:alt: Whoopsy!
:width: 450px
:align: center
```

There are many graph morphisms from graph (a) to graph (b). But there is exactly one choice that will make the triangle formed by the two blue arrows and the dotted arrow commute. This choice is again the most natural one, mapping blue to blue, green to green, orange to orange. The arrow from (a) to (b) is dotted to signify that there is only one such embedding. 

As I mentioned earlier in this chapter, commuting diagrams are like ecosystems in balance. The above diagram has 4 ecosystems pasted together (including the 2 squares starting at Graph-1). Any change in one ecosystem will create a change in the other. When the “most natural” information flow along the morphisms, all the ecosystems are in balance.

```{image} assets/Ch3/4.3-compare-a-b-3.png
:alt: Whoopsy!
:width: 450px
:align: center
```

Can we embed (b) in (a) such that the diagrams (1), (2), (3) will commute simultaneously? Yes, there is no such embedding — the orange-green vertex of (b) can embed in either the orange or the green vertex of (a). It embeds in the orange vertex, then diagram (2) does not commute. If it embeds in the green vertex, then diagram (3) does not commute. Since the choice (b) has coarse-grained the information of Graph-2 and Graph-3, the information cannot be fine-grained again!

**Score** Choice (a): 1,  Choice (b): 0

**Let us compare choice (a) with choice (c)**

Again, there is exactly one (obvious) choice of morphism from (a) to (c) that will make the  yellow and the blue triangles commute. 

```{image} assets/Ch3/4.3-compare-a-c-1.png
:alt: Whoopsy!
:width: 450px
:align: center
```

However, there are at least two ways in which (c) embeds in (a) to make the shapes (1), (2), and (3) commute. Both these choices are equally good! The grey vertex in (c) can be mapped to two blue vertices in (a). Because, (c) has extra information, that is vertex and edge not from Graphs 1-3, there are many ways of embedding this extra information in the most lazy choice (a).

```{image} assets/Ch3/4.3-compare-a-c-2.png
:alt: Whoopsy!
:width: 450px
:align: center
```

**Score** Choice (a): 1,  Choice (c): 0

**Let us compare (a) with (d)**

There is exactly one (obvious) choice of morphism from (a) to (d) that will make the  yellow and the blue triangles commute. 

```{image} assets/Ch3/4.3-compare-a-d-1.png
:alt: Whoopsy!
:width: 450px
:align: center
```

However, (d) does not embed in (a) because (d) has  a self-loop and can embed only in graphs with at least one self-loop. 

```{image} assets/Ch3/4.3-compare-a-d-2.png
:alt: Whoopsy!
:width: 450px
:align: center
```

**Score** Choice (a): 1,  Choice (d): 0

Our choice of colimit graph having such ‘universal’ nature, meaning there must be exactly one way to embed it into any other choice, is an indication that we got our plumbing right and now things will flow smoothly! In category theory, this is called a universal property. 

Whewww!! That is some hard core relational thinking!

### D. Uniqueness of the colimit

Let us review what we have done so far! In this game, to begin with, a guess of a colimit graph could be any graph in the space of all possible graphs. Since we do not know what this graph is, we will name this as “Graph X”. 

By asking the first question, we narrowed down the possible set of guesses from the space of all graphs to those graphs which makes  commute.

```{image} assets/Ch3/4.1-req1.png
:alt: Whoopsy!
:width: 450px
:align: center
```
By asking the second question, we narrowed down the guesses further to those graphs which has exactly one embedding for any other choice that makes the previous diagram commute. The embedding makes diagrams (1) and (2) commute simultaneously. 

```{image} assets/Ch3/4.1-req2.png
:alt: Whoopsy!
:width: 450px
:align: center
```

The final question to find “THE colimit” is that ***if there is only one choice of graph which satisfies requirement 1 and requirement 2***. 

Time to answer the final question: 

Suppose there are two choices that satisfies the requirements 1 and 2.  Lets called these call these graphs “X1” and “X2”

Because X1 is universal, X1 has a unique embedding into X2.

```{image} assets/Ch3/4.1-req3.png
:alt: Whoopsy!
:width: 450px
:align: center
```

Because X2 is universal, X2 has a unique embedding into X1. 

```{image} assets/Ch3/4.1-req4.png
:alt: Whoopsy!
:width: 450px
:align: center
```

However, requirements 1 and 2 tells us that these two unique embeddings are inverse of each of other!! That is, graphs X1 and X2 are “practically the same” (This is not a requirement but a consequence of the existing requirements on relationships). So, for practical purposes, any graph that satisfies requirements 1 and 2 is the colimit graph we are looking for!!

Given two graphs with overlap, we now know how to precisely narrow down to their colimit  from the set of all graphs, purely based on the relationships between the colimit and all the other relevant graphs!


## 5.4. Collecting it all together

The goal of our dumb charades game is to say “what is colimit graph” without talking about its connectivity (vertices and edges). We used the relationships between graphs and commuting diagrams to describe what the colimit graph of a diagram must look like! Let us summarize our description of colimit of a diagram. 

We start with a graph which embeds into two graphs:

```{image} assets/Ch3/colimit-diagram.png
:alt: Whoopsy!
:width: 450px
:align: center
```

What we compute is a graph called “colimit” which satisfies the following requirements:

**Requirement 1**: The square commutes

```{image} assets/Ch3/5-req1.png
:alt: Whoopsy!
:width: 450px
:align: center
```

**Requirement 2**: For any choice of graph which satisfies requirement 1, there is exactly one way (a unique) embedding of the colimit into that choice. The triangles made by the embedding commute. 

```{image} assets/Ch3/5-req2.png
:alt: Whoopsy!
:width: 450px
:align: center
```

As a consequence of these requirements, all graphs which satisfy both these requirements **are “practically the same”**. 

:::{note}
Relational thinking narrows down a solution from the space of all sensible structures by methodically reflecting on “WHAT” is that we are looking for rather focusing on “HOW” to construct a solution and verify that the construction will always produce a sensible structure.  

In this sense, relational thinking is non-invasive!

:::

## 5.5. Time for pen and paper! 

Let us compute colimit for the following diagrams.

**Ex 1. Colimit using a single vertex**
```{image} assets/Ch3/Ex-1.png
:alt: Whoopsy!
:width: 450px
:align: center
```

:::{admonition} Solution
:class: dropdown
```{image} assets/Ch3/Ex-1-sol.png
:alt: Whoopsy!
:width: 450px
:align: center
```
:::


**Ex 2. Colimit using a single edge**

This one is a little tricky, because the two vertices (”A” and “B”) in Graph-1 embeds into the same vertex of Graph-3 (”A,B”), thereby coarse-graining the information in Graph-1. However, vertices “A” and “B” in Graph-1 embeds in separate vertices in Graph-2. So, how would the overlap region look in the colimit graph? 

```{image} assets/Ch3/Ex-2.png
:alt: Whoopsy!
:width: 450px
:align: center
```

:::{admonition} Solution
:class: dropdown

Since Graph-2 and Graph-3 has to agree in the overlap region, and Graph-3 has only coarse-grained embedding of Graph-1, the embedding of Graph-2 in the colimit, coarse-grains the overlap region of Graph-2 to match the overlap region of Graph-3. 

```{image} assets/Ch3/Ex-2-sol.png
:alt: Whoopsy!
:width: 450px
:align: center
```
:::


**Ex 3. Colimit using a empty graph**

In the beginning of this chapter, we said that combining graphs is analogous to adding numbers. Can you see how this problem demonstrates this analogy?

```{image} assets/Ch3/Ex-3.png
:alt: Whoopsy!
:width: 450px
:align: center
```
(An empty graph is the graph with its set of vertices and the set of edges to be empty set. The sources and the target maps sends “no vertex” to “no edge”.)

:::{admonition} Solution
:class: dropdown

```{image} assets/Ch3/Ex-3-sol.png
:alt: Whoopsy!
:width: 450px
:align: center
```
When there is no glue (Graph 1 is empty), the colimit just has Graph-2 and Graph-3 side by side with no bridge in between them. Graph-2 has three vertices and no edges. Graph-3 has two vertices and no edges. The colimit graph has 5 vertices which is the sum of vertices in Graph-2 and vertices in Graph-3. Sum of two numbers is just a colimit . Isn't that cool ?!
:::

**Ex 3. Finite colimits**

Let us suppose, we want to glue more than two graphs together! That seems to be a reasonable ask! So we got a diagram like the one below. 

```{image} assets/Ch3/Ex-4.png
:alt: Whoopsy!
:width: 350px
:align: center
```

What are the requirements for a graph to be colimit of the above diagram?

Clue: Extend the requirements in Section 5.4 from 2 to n graphs! 

## 5.6. Operationalizing computing colimits

In the previous section, we hand-computed the colimits of the diagrams. This section show cases how this computation can be performed on a computer using Algebraic Julia. 

(Kris, is there anything interesting to tell about computing pushouts in AJ?)

Solving Ex 1. 
+++

```{code-cell}
# AJ code goes here

```

+++



Solving Ex 2. 
+++

```{code-cell}
# AJ code goes here

```

+++



Solving Ex 3. 
+++

```{code-cell}
# AJ code goes here

```

+++


## 5.7. Conclusion 
Congratulations!! You have crossed Chapter 5 successfully!

Things get quite complex and rich quickly as we add relationships and ask the relationships to satisfy more and more constraints. However, once set up right, the tools make life better because all the complex thinking is handled early on eliminating the necessity to think cleverly about the edge cases when the tools is used.  We will demonstrate this idea in the next chapter when we look into the concept of “find and replace” inside graphs! You would be accustomed to using the “find-and-replace” operation in text editors. In the next chapter, we shall apply the idea of “find-and-replace” to graphs by the means of graph colimits. 

It only gets easier from here!!