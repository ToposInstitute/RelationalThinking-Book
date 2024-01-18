# Introduction (Draft)
By Brendan Fong

## Systems thinking through software

We encounter the word system frequently, hearing talk of health care systems, computer systems, the solar system, and more. Yet the word itself is hard to define. A system is a bunch of things interacting together, somehow. It seems abstract, and often hard to think about. What does one do when part of a system? How does one act in order to change things? What causes what, when everything is interlinked? How do we know what is part of a system, and what isn't?

Yet the notion of system persists. Despite its own lack of clarity, it clearly points to some real phenomenon, a phenomenon that is important for us to reference as we try to navigate the world. Thinking in systems promises a new way of looking at the world holistically. It's a way of thinking that emphasises relationships and context, and deemphasises analysis by deconstruction. That is, to seeking understanding about an object it looks outwards, asking how it interacts, rather than inwards, asking what it is made of.

A challenge with systems thinking is that formal tools for thinking systemically are still a topic of active research -- we still are discovering the logic of systems, discovering effective ways to rigorously reason about them. Mathematics has not historically emphasised a systems perspective. Mathematical tools for thinking, including thinking systemically, are useful and powerful, because they provide assistance in thinking carefully, keeping you on the right path to getting conclusions you can trust and explain to others. Without such tools, it can be hard to know whether systems thinking is providing a productive framing, or just new ways to be puzzled and to disagree.

There are some promising avenues, however, for mathematical systems thinking. A key tool for mathematical systems thinking, known as category theory, emerged in the 1940s. While this is almost a century ago, developing mathematics is a slow, patient, and intergenerational effort, and it's only in recent decades it is developing as a tool for systems thinking in the real world.

A second challenge presents itself: mathematics itself can difficult to learn, especially while it is still an active topic of research by world-leading mathematicians, who do not always explain themselves well. In this book we hope to give the reader the experience of mathematical systems thinking without requiring the mastery of any of the abstract tools of category theory. This is made available to us by leaning on the encoding of these category theoretic ideas into software, via the AlgebraicJulia project.

AlgebraicJulia is a collection of libraries in the programming language Julia that provides tools for model-driven science. A model is a simplified description of another thing, that highlights important features, and forgets the ones less relevant. For example, picture a collection of clay balls that represent the sun and planets in the solar system -- it might capture some aspects of the relative sizes of the planets and their distance from the sun, but forget what the planets are composed of. (Jupiter is not made of clay!) Models make it easier to reason, and do data science, around complicated things we encounter in our lives. AlgebraicJulia lets us use a systems perspective to create models for scientific computing. Its design is heavily informed by the insights developed in category theory over the past 100 years.

This is a book written in two parts. In the first part, we

The goal of this book is to inform and inspire.

Our book is example driven, with references to the interested reader on where to learn more about both the programming and the mathematics. There are embedded sections of code,