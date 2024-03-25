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

## Systems thinking through software

We encounter the word system frequently, hearing talk of health care systems, computer systems, the solar system, and more. Yet the word itself is hard to define. A system is a bunch of things interacting together, somehow. It seems abstract, and often hard to think about. What does one do when part of a system? How does one act in order to change things? What causes what, when everything is interlinked? How do we know what is part of a system, and what isn't?

Yet the notion of system persists. Despite its own lack of clarity, it clearly points to some real phenomenon, a phenomenon that is important for us to reference as we try to navigate the world. Thinking in systems promises a new way of looking at the world holistically. It's a way of thinking that emphasises relationships and context, and deemphasises analysis by deconstruction. That is, to seeking understanding about an object it looks outwards, asking how it interacts, rather than inwards, asking what it is made of.

A challenge with systems thinking is that formal tools for thinking systemically are still a topic of active research -- we still are discovering the logic of systems, discovering effective ways to rigorously reason about them. Mathematics has not historically emphasised a systems perspective. Mathematical tools for thinking, including thinking systemically, are useful and powerful, because they provide assistance in thinking carefully, keeping you on the right path to getting conclusions you can trust and explain to others. Without such tools, it can be hard to know whether systems thinking is providing a productive framing, or just new ways to be puzzled and to disagree.

There are some promising avenues, however, for mathematical systems thinking. A key tool for mathematical systems thinking, known as category theory, emerged in the 1940s. While this is almost a century ago, developing mathematics is a slow, patient, and intergenerational effort, and it's only in recent decades it is developing as a tool for systems thinking in the real world.

A second challenge presents itself: mathematics itself can difficult to learn, especially while it is still an active topic of research by world-leading mathematicians, who do not always explain themselves well. In this book we hope to give the reader the experience of mathematical systems thinking without requiring the mastery of any of the abstract tools of category theory. This is made available to us by leaning on the encoding of these category theoretic ideas into software, via the AlgebraicJulia project.

## AlgebraicJulia

AlgebraicJulia is a collection of libraries in the programming language Julia that provides tools for model-driven science. A model is a simplified description of another thing, that highlights important features, and forgets the ones less relevant. For example, picture a collection of clay balls that represent the sun and planets in the solar system -- it might capture some aspects of the relative sizes of the planets and their distance from the sun, but forget what the planets are composed of. (Jupiter is not made of clay!) Models make it easier to reason, and do data science, around complicated things we encounter in our lives. AlgebraicJulia lets us use a systems perspective to create models for scientific computing. Its design is heavily informed by the insights developed in category theory over the past 100 years.

While AlgebraicJulia is still a software ecosystem under development, and is not yet ready to meet all general purpose scientific modelling needs, we believe it is mature enough to be a significant aid in practicing relational thinking.

## Key ideas

The arc of this book is as follows. We begin by considering directed graphs: a simple language of dots and arrows between them. Our goal is to be able to think about these from a relational perspective. To do so, we slowly step up a four rung ladder of relational thinking.

In learning how to think about directed graphs relationally, we see in a microcosm the broader, abstract lessons of relational thinking.

Once at the top of this ladder, we can begin to appreciate the view. 

Modelling (descriptive) vs thinking (operationalised). Modelling is about representating the world. Thinking is about cognitive switch in focus to relationships rather than objects. Models are the nouns of thought. Relational thinking works best with relational models. So first we need to construct relational models. Then we will learn to manipulate them to derive conclusions.

Getting the direction right is important — counting vs labelling

Situate things in the context of other objects they relate to

Explicitly identify the structure of composition
Identify the categorical world -- contextualises models in the space of possibilities, allows reasoning about variation
Identify similar schemas
explicitly describe maps/morphisms

efficient because it allows reuse of ideas and code across contexts

Doing this leads to insights. Doing this formally via a programming language meants we can automatically extract and benefit from those insights. Slogan: "Describe the world, and get something for free!"


## How we've designed this book


### Reader background

We have chosen not to assume any particular prior mathematical or programming knowledge in the design of this book. That said, it is written from a mathematical viewpoint, and a reader with some familiarity with the style of thinking will find it easier going. But we hope that some readers will also use this book as a way to get acquianted with mathematical thought.  

// Add difficulty graph of chapters

// Add dependency graph of chapters

Our book is example driven, with references to the interested reader on where to learn more about both the programming and the mathematics. There are embedded sections of code,

### Live, in-line code

Similarly, a reader with an introductory experience to programming will find such experience helpful. For example, to deeply engage with AlgebraicJulia, it is useful to have a local installation of the Julia language and AlgebraicJulia libraries. Nonetheless, we have endeavoured to ensure this is not necessary to understand the main messages of this book. We have done this by embedding editable, executable in-line code blocks in most chapters. 

For example, here is one.
```{code-cell}
print( "My in-line code is working, yay!" )
```

To run the code, click the "rocket icon" at the top right corner of this page, and select "Live code". Enabling live code will cause a button labelled "run" to appear in each executable code block. Clicking "run" will run the code in the block.

The live code environment may take a little time to start up, similar to the way a computer may take a while to boot up when first turned on. While it is starting up, a "Waiting for kernel..." message will be displayed. Once the environment is running, responses should be faster. Because of this delay, it may be worth enabling live code as soon as you open a new chapter, so it can start up in the background while you read.



### Puzzles

Each chapter contains a few puzzles. These puzzles provide an opportunity to take an active role in exploring and internalising the key lessons of the chapter. We encourage to you attempt them, spend some time with them, and play, even use them as a jumping off point for daydreaming about relational thinking. There is no rush.

```{admonition} Puzzle
You can edit the code blocks, and run them again! See if you can edit the program above so it prints a sentence describing your motivation for reading this book, instead of just "My in-line code is working, yay!".
```

## A new medium for systems thinking
As Andy Matuschak and Michael Nielsen write in their essay "How can we develop transformative tools for thought?"[^1]:

[^1]: Andy Matuschak and Michael Nielsen, “How can we develop transformative tools for thought?”, https://numinous.productions/ttft, San Francisco (2019).

> Such a medium creates a powerful immersive context, a context in which the user can have new kinds of thought, thoughts that were formerly impossible for them. Speaking loosely, the range of expressive thoughts possible in such a medium is an emergent property of the elementary objects and actions in that medium. If those are well chosen, the medium expands the possible range of human thought.

We have not yet fully realised the potential of a new medium for systems thinking. But as Matuschak and Nielsen note, the range of possible thoughts is an emergent property of the elementary objects and actions. In systems thinking, these notions are ones of relationality; the same ones that category theorists have carefully studied for the last century. We believe the work we share here, grounded in the mathematics of category theory, thus gives a glimpse of a powerful start. The work of the AlgebraicJulia project is to expand this mathematical medium into an embodied, software technology, from mathematics to programming languages and eventually to computer applications. We invite you on this journey to expand the possible range of human thought to consider the world in a holistic, relational, and interconnected way.

Many of the great challenges of today are a result of our shared struggle to see the world in its beautiful, complex, interconnected glory. We hope that this book is a contribution to facing these challenges together.