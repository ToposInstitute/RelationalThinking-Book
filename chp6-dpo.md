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


"Find-and-replace" feature of text editors is one of the most powerful innovations of the 20th century. Inspite of unavailability of statistics, the advantage of this feature is tangible and undeniable! Beyond text editors, the concept of "find and replace" has also caused much chaos in the world than being helpful! When the European conquerors "found" native Americans settlements in Canada, they decided to "replace" the native culture by sending an entire generation of native American children to special missionary schools. This has resulted in trauma and chaos that continues well into the current times. Or forcefully "replacing" an exisiting government of a country by another country for political reasons. (may be an ecology example).  

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

:::{admonition} Write the following find-and-replace using plus-minus interface:

````{div} wrapper 

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

:::{admonition} How many matches? 

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

## 6.2. Adding vertices and edges to a graph



## 6.3. Removing vertices and edges from graph


## 6.4. Adding and removing in one go

## 6.5. Graph rewriting in organic chemistry

## 6.6. Graph rewriting in game design


----------
IGNORE!!

We are familiar to performing 'cut-copy-paste' in text documents. Now, imagine, instead of a body of text we have a graph with numerous vertices and edges. We now want a "cut-copy-paste" analogue of text documents for graphs -- 'Cut' feature to remove vertices and edges from a graph and 'copy-paste' feature to add new vertices and edges to the graph. 

You can already imagine the complications of performing "cut-copy-paste" on graphs. A text editor allows one to cut text from any position, and paste text into any position. What a disaster it would cause if this idea is imported directly to graphs!

Cut vertex "A" from the graph. What remains is not even a graph anymore! 




What is meant by copy-paste of a graph into another graph? 

The challenge is that we want to modify a graph b 