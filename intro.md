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

# Introduction

:::{note}
This book is a work-in-progress! We'd love to learn how we can make it better, especially regarding fixing typos or sentences that are unclear to you. Please consider leaving any feedback, comments, or observations about typos in [this google doc](https://docs.google.com/document/d/1MvhNuap0QLMAfrMQLIAxbclBx0vjt6vyK8BhVhLwFoQ/edit).
:::

## Systems thinking, relational thinking, and software

Biological systems, computer systems, healthcare systems, the solar system, and on and on...

Systems come in different forms and flavors. Even though the word "system" is quite familiar, it is quite hard to say precisely what it means! Loosely stated, a system is a bunch of things interacting together, somehow. The idea of a system seems quite abstract, and more often, hard to think about.  What should we do when we are part of a system? How can we act in order to change things? What causes what, when everything is interlinked? How do we know what is part of our system, and what isn't?

Yet the notion of system persists. Despite its own lack of clarity, it clearly points to a real phenomenon, a phenomenon that is important for us to reference as we try to navigate the world. Thinking in systems promises a new way of looking at the world holistically.[^1] It's a way of thinking that emphasises relationships and context, and deemphasises analysis by reduction and deconstruction. 

In this book, we will focus on a specific aspect of systems thinking we term <mark>relational thinking</mark>. Relational thinking seeks to understand an object by taking it as a point from which to look outwards, asking how the object interacts, rather than inwards, asking what the object is made of. It's an excitingly and refreshingly different viewpoint from the ways we often are taught to think. And it's a powerful one.

### Mathematics for relational thinking

The power of a way of thinking can be amplified by mathematics. Mathematical tools for thinking, including thinking relationally, are useful and powerful because they provide assistance in thinking carefully, keeping us on the right path to getting conclusions we can trust and explain to others. Without such tools, it can be hard to know whether a method like relational thinking is providing a productive framing, or just new ways to be puzzled and to disagree. 

One challenge with relational thinking is that formal tools for thinking relationally are a relatively new topic of research. We are actively discovering the logic behind various systems, and discovering effective ways to rigorously reason about them.

Nonetheless, even though mathematics hasn't historically emphasised a systems perspective, there are some promising avenues in mathematics for relational thinking. A key mathematical tool for relational thinking, known as category theory, emerged in the 1940s.[^2] While this is almost a century ago, developing mathematics is a slow, patient, and inter-generational effort, and it's only in recent decades it is developing as a tool for systems thinking in the real world.

A second challenge presents itself, however: mathematics itself can be difficult to learn, especially when it is still an active topic of research and even the world-leading mathematicians do not always explain themselves well. In this regard, the purpose of this book is to give the reader the experience of mathematical relational thinking without requiring the mastery of any of the abstract tools of category theory. This is made available to us by leaning on the encoding of these category theoretic ideas into software, via the <mark>AlgebraicJulia project</mark>.

### AlgebraicJulia

[AlgebraicJulia](https://www.algebraicjulia.org) is a collection of libraries in the programming language Julia, that provide tools for model-driven science. A model is a simplified description of another thing, that highlights important features, and forgets the ones less relevant. For example, picture a collection of clay balls that represent the sun and planets in the solar system -- it might capture some aspects of the relative sizes of the planets and their distance from the sun, but forget what the planets are composed of. (Jupiter is not made of clay!) Models make it easier to reason, and do data science, around complicated things we encounter in our lives. AlgebraicJulia lets us use a systems perspective to create models in computers and to study them. Its design is heavily informed by the insights developed in category theory over the past 100 years.

AlgebraicJulia is still a software ecosystem under development. It's vision is to provide a relational thinking-based ecosystems for general purpose scientific modelling needs. Even though, AlgebraicJulia is a project in progress, we believe it is mature enough to be a significant aid in practicing relational thinking. 

## Key ideas

The goal of this book is to give you, the reader, an experience of relational thinking. This book differs from many mathematical texts, in that we do not seek to give abstract definitions, construct general theory, and prove mathematical theorems. Instead, we focus on a particular example called <mark> directed graphs</mark> that, while simple in nature, allows for a deep dive through relational thought. Although we focus on a particular, simple example, the relational patterns of thought we will experience are general in nature, and apply much more broadly.

The example we have chosen which is directed graphs is a simple language of dots and arrows between them. This is possibly the barest example we could have chosen. Yet we will see that even in this simple case, there are thorny questions, particularly around what we shall call 'dangling edges'. And we will also see that by thinking relationally, these questions can be easily and naturally resolved.

### Part I: Climbing the ladder of relational abstractions

Our first task, then, is to learn to think about directed graphs from a relational perspective. To do so, we slowly step up a four rung ladder of relational thinking. This forms the first part of the book.

```{image} assets/Ch1/Ladder.png
:alt: Whoopsy!
:width: 500px
:align: center
```
</br>

Beginning in the concrete world of directed graphs as pictures made up of dots and arrows, each rung takes us through a conceptual shift that is required to move us into a world of relational models. 

In Chapter 0, we meet a few examples of **directed graphs**, as well as the problem of dangling edges. Chapter 1 moves us into the world of **data**, showing how we can provide a relational representation of directed graphs that captures their core essence, no more or no less. We reflect on what makes a good abstraction. This allows us to represent graphs as data, and hence make them suitable to computer reasoning, through tools like AlgebraicJulia. After the more mathematical nature of Chapter 1, Chapter 2 then shows the immediate payoff: we learn a bit about AlgebraicJulia, and how to use directed graphs as data to program dynamical systems.

In Chapter 3 we move from the world of data to the world of **blueprints**. Also called schema, blueprints provide a framework for describing different sorts of data. For example, while we focus on the blueprint for directed graphs, we see directed graphs belongs to a family of different sorts of data, including undirected graphs, three-dimensional and higher-dimensional shapes, interacting processes, and more. The payoff of understanding the blueprint for graphs is that it immediately helps us understand the appropriate notion of relationship between graphs, and hence the universe of possible graphs. This is the focus of Chapter 4, which moves us from the world of blueprints to the world of universes, also known as **categories**. This takes us to the top of our ladder of relational thinking.

### Part II: Appreciating the relational view

Once at the top of our ladder, we can begin to appreciate the view. Now in the world of relational models, our second goal, is to showcase how to *think* relationally within this world. Chapter 5 uses relationships between graphs to help us think about when two graphs are the same, when a graph is a part of another graph, how to describe a collection of interrelated graphs, and ultimately how to construct new graphs by *gluing graphs* together. While these questions may sound abstract, imagine two graphs modeling two systems. Then, the value of answering the questions on graph relationships start shining forth. 

Chapter 6 builds on Chapter 5 to describe how graphs *evolve*, or change over time. Finally, Chapter 7 shows the payoff, extending this to more sophisticated blueprints, so that we may model changes in our physical world, or reason about processes like making a sandwich in your kitchen which has applications in robotics.

Ultimately, through this experience, we hope that you'll come away with a sense of the importance of finding good abstractions, identifying the ways objects relate to each other, situating them in the context of other objects they relate to, and contextualizing them models in the space of possibilities. We also hope that you'll see the concrete payoffs, especially by doing this formally via a programming language, so that we can automatically extract and benefit from the insights that come from a relational perspective. Indeed, while it may seem in the beginning like we're just finding different, more abstruse ways of describing directed graphs, we hope that in the problem of dangling edges you'll see how the payoff naturally emerges, with seemingly little effort. 

## How we've designed this book

### Style 

This book is an experimental project. This book attempts to reveal the thought-process behind creating mathematics (category theory) using story-telling, lots of illustrations, and visualization of computations. As such, this book is close to a creative non-fiction.

```{image} assets/Intro/machinery.png
:alt: Whoopsy!
:width: 400px
:align: center
```
</br>


The process of learning something new is like building a machine by collecting its many little parts, and by screwing all those parts tegether. When we got all the parts and when all those parts are in their right place screwed together, then the machine can take us forward in our further explorations! Most of the textbooks focus on providing the big screws and the parts of its machine to the reader. However, its the tiny nuts and bolts which decide if someone's understanding is tight and intact, or not. What this book does is to provide as much importance to the tiny nuts and bolts as much as to the big screws. Consequently, this book covers just enough material towards its goal in terms of breadth and depth. This has been made possible by the story-telling style of writing. 

### Reader background

We have chosen not to assume any particular prior mathematical or programming knowledge in the design of this book. That said, it is written from a mathematical viewpoint, and a reader with some familiarity with the style of thinking will find it easier going. We hope, however, that even if you do not have experience with mathematics or programming, that you might find it interesting to use this book as a way to get acquianted with the beauty and power of mathematical thought.  

In terms of difficulty, the book starts very concretely, but ramps up in difficulty as we get to the top of the ladder, peaking in Chapter 5. Again, however, once we're at the top of the ladder, we can begin to appreciate the view, and we focus more on unpacking the exciting implications of our ideas, rather than introducing new complexity.

```{image} assets/Intro/chapters-difficulty.png
:alt: Whoopsy!
:width: 400px
:align: center
```
</br>

The book is intended to be read somewhat linearly, with each chapter depending on the last. The exception is Chapter 2, which provides a bit of relief in our climb to introduce more of AlgebraicJulia by taking a detour into some fun ways to program and explore dynamical systems using directed graphs. 

Note that it is standard, however, to read mathematical texts in a circular pattern, often revisiting earlier sections, or even the book as a whole, to glean new insights and deeper understanding over time. In particular, don't be discouraged if things don't make sense on a first reading! This is completely normal, and happens to us too as we read books on mathematics. It takes a little while to get acquianted with new ways of thinking.

We hope that even if difficult at times, you will find this book fun. 

### Live, in-line code

Similarly, a reader with an introductory experience to programming will find such experience helpful. For example, to deeply engage with AlgebraicJulia, it is useful to have a local installation of the Julia language and AlgebraicJulia libraries. Nonetheless, we have endeavoured to ensure this is not necessary to understand the main messages of this book. We have done this by embedding editable, executable in-line code blocks in most chapters. 

For example, here is one.
```{code-cell}
print( "My in-line code is working, yay!" )
```

To run the code, click the "rocket icon" at the top right corner of this page, and select "Live code". Enabling live code will cause a button labelled "run" to appear in each executable code block. Clicking "run" will run the code in the block.

```{image} assets/Ch4/Binder_Instructions.png
:alt: Whoopsy!
:width: 400px
:align: center
```
</br>

The live code environment may take a little time[^3] to start up (status is shown at the top of the page), similar to the way a computer may take a while to boot up when first turned on. While it is starting up, a "Waiting for kernel..." message will be displayed. Once the environment is running, responses should be faster. Because of this delay, it may be worth enabling live code as soon as you open a new chapter, so it can start up in the background while you read.


### Puzzles

Each chapter contains a few puzzles. These puzzles provide an opportunity to take an active role in exploring and internalising the key lessons of the chapter. We encourage to you attempt them, spend some time with them, and play, even use them as a jumping off point for daydreaming about relational thinking. There is no rush.

:::{admonition} Puzzle
You can edit the code blocks, and run them again! See if you can edit the program below so it prints a sentence describing your motivation for reading this book, instead of just "My in-line code is working, yay!". You need to click the "rocket icon" at the top right corner of this page, and select "Live code" to start editing.

:::

+++
```{code-cell}
print( "My in-line code is working, yay!" )
```
+++

## A new medium for systems thinking
As Andy Matuschak and Michael Nielsen write in their essay "How can we develop transformative tools for thought?":[^4]

> Such a medium creates a powerful immersive context, a context in which the user can have new kinds of thought, thoughts that were formerly impossible for them. Speaking loosely, the range of expressive thoughts possible in such a medium is an emergent property of the elementary objects and actions in that medium. If those are well chosen, the medium expands the possible range of human thought.

We have not yet fully realised the potential of a new medium for systems thinking. But as Matuschak and Nielsen note, the range of possible thoughts is an emergent property of the elementary objects and actions. In systems thinking, these notions are ones of relationality; the same ones that category theorists have carefully studied for the last century. We believe the work we share here, grounded in the mathematics of category theory, thus gives a glimpse of a powerful start. The work of the AlgebraicJulia project is to expand this mathematical medium into an embodied, software technology, from mathematics to programming languages and eventually to computer applications. We invite you on this journey to expand the possible range of human thought to consider the world in a holistic, relational, and interconnected way.

Many of the great challenges of today are a result of our shared struggle to see the world in its beautiful, complex, interconnected glory. We hope that this book is a contribution to facing these challenges together.


## Footnotes and references

[^1]: For an excellent introduction to thinking in systems, see: Meadows, Donella. H. (2015). [Thinking in Systems](https://donellameadows.org/systems-thinking-book-sale/). Chelsea Green Publishing.

[^2]: Eilenberg, Samuel, and Saunders MacLane. “General Theory of Natural Equivalences.” Transactions of the American Mathematical Society 58, no. 2 (1945): 231–94. https://doi.org/10.2307/1990284.

[^3]: We gratefully use the free service provided by [Binder](https://mybinder.org), which runs our (Algebraic)Julia code on a server they host. Availability depends on a number of factors, and in our experience the kernel can take anywhere from a few minutes to half an hour to start up. If it's taking too long, consider running a version of the code on your own computer. All the code in this book is available in this github repo - https://github.com/ToposInstitute/RelationalThinking-code. 

[^4]: Andy Matuschak and Michael Nielsen. “How can we develop transformative tools for thought?” https://numinous.productions/ttft, San Francisco (2019).