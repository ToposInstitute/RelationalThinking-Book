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
header-includes:
  - \usepackage{amsmath}
---



# Chapter 7: Looking beyond

:::{attention}
This chapter will extend the previously learned concepts from graphs to the world!
:::

So far, we have uncovered many powerful concepts for modeling graphs and changes to them. Using _schemas_, we can cleverly package up data of a graph in a way that refers to its fundamental concepts, nodes and edges, and how they relate to one another. Using _double-pushout (DPO) rewriting_, we can make a whole host of edits to the data of the graph that can be used to model a number of graph-based scenarios, such as the Greek mythological romance, chemical reactions, and game design. 

Schemas provide us with the language to talk about the concepts we wish to shed light on. The graph schema highlights edges and how they relate to nodes. As remarkabley applicable these models are, they beg the question, how do they work with something more complex? For example, what if we were interested in talking about parts of a car or items in your household. We would have to do quite a bit of mental book-keeping to talk in the language of graphs-- "Remember, this node is about a bottle of Coca-Cola soda and this node is about my refridgerator... and the edge between them means that the Coca-Cola soda **is in** the refridgerator,... but this other edge between the soda and a node representing a box of Mentos means that the Mentos should **never** be put in the soda..."

:::{admonition}
If you put Mentos in a soda, it will explode.
:::

Lucky for us, what we have learned so far can have more general applications than graphs. The rest of this chapter will show some examples for this.

## Example 1: Cube Configuration
Let's start by simply extending the graph schema with another concept, like faces to model a cube. Consider a scenario where the index category, $\mathsf{C}$, represents the schema for a three-dimensional (3D) shape, comprising three fundamental objects: `Face`, `Edge`, `Vertex`. This schema is enriched with six non-identity morphisms: `top`, `bottom`, `left`, `right`, `src`, `tgt`, which define the relationships between these objects. DPO rewriting in this context can model various transformations of a 3D object including cube,or box. 

This can be expressed as a schema in AlgebraicJulia as follows:

```{code-cell}
@present Sch3DShape(FreeSchema) begin
  Face::Ob
  Edge::Ob
  Vertex::Ob

  top::Hom(Face, Edge)
  right::Hom(Face, Edge)
  bottom::Hom(Face, Edge)
  left::Hom(Face, Edge)

  src::Hom(Edge, Vertex)
  tgt::Hom(Edge, Vertex)
end
to_graphviz(Sch3DShape)
@acset_type Typ3DShape(Sch3DShape)
```

![Sch3dShape](assets/Chp7/Sch3DShape.svg)

We can see that the schema, `Sch3DShape`, closely resembles the schema for a graph, `SchGraph`. More pointedly, `Sch3DShape` extends `SchGraph` with the idea of a `Face` and edges of a face. 


:::{admonition} Extending schemas
In AlgebraicJulia, inheritence of schemas can be programmed using `<:`. In the case of `Sch3DShape` and `SchGraph`, this can be written as:

```
@present Sch3DShape(FreeSchema) <: SchGraph begin
  Face::Ob

  top::Hom(Face, Edge)
  right::Hom(Face, Edge)
  bottom::Hom(Face, Edge)
  left::Hom(Face, Edge)
end
```
:::

We can model various configurations of a cube, akin to unfolding a cardboard box. For instance, using DPO rewriting, we could model the action of opening or closing the top of the box. This operation would involve redefining the relationships between the Faces and Edges objects to "remove" the connections that form the top face. Similarly, unfolding the cube into a flat layout would radically alter the connections between Faces, Edges, and Vertices to represent the cube in an unfolded state. Such transformations are powerful for visualizing and reasoning about the structural possibilities of objects in three-dimensional space.

![](assets/Ch7/TopOpening.gif)

Let us say we want to have a cube represent a box with a birthday cake in it. When we deliver the cake to a friend, it starts off as closed. We can say that it's closed by explaining how the faces, edges, and vertices are defined relative to one another. In a cube, there are exactly six faces, which we can call `f1, f2, f3, f4, f5, f6`. Faces have exactly four edges, which we call the top, bottom, left, and right edges. Edges can also have names, such as `e1, e2, e3, e4, ...`. If we want to say the top of edge of `f1` is `e1`, then we can refer to the schema and express the the following part:

```
top(f1) == e1
```

Similarly, we can say the left edge of `f1` is `e2` by saying:

```
bottom(f1) == e2
```

Edges, on the other hand, have exactly two vertices, the source and target. This is reminiscent of how we came to understand edges in a graph. The schema for both the Cube and the Graph have two morphisms, `src` and `tgt`, that map `Edge` to `Vertex`. So, if we would like to state what vertices bound the edge, `e1`, we can say  

```
src(e1) == v1
tgt(e1) == v2
```

### A closed box

We can create an instance of a cube, by defining such a $\mathsf{C}$-Set, where $\mathsf{C}$ is `Sch3DShape`.

![](assets/Ch7/BoxAssembly.gif)

```{code-cell}
closedCube = @acset_colim ySch3DShape begin
  (f1, f2, f3, f4, f5, f6)::Face

  # top face (clockwise)
  top(f1) == top(f5)        # e1
  right(f1) == top(f2)      # e2
  bottom(f1) == top(f3)     # e3
  left(f1) == top(f4)       # e4

  # wall faces (clockwise)
  left(f2) == right(f3)     # e5
  left(f3) == right(f4)     # e6
  left(f4) == right(f5)     # e7
  left(f5) == right(f2)     # e8

  # bottom face (clockwise)
  top(f6) == bottom(f5)     # e9
  right(f6) == bottom(f2)   # e10
  bottom(f6) == bottom(f3)  # e11
  left(f6) == bottom(f4)    # e12

  # top vertices
  tgt(top(f1)) == src(right(f1))     # v1
  tgt(top(f1)) == src(right(f2))
  tgt(right(f1)) == src(bottom(f1))  # v2 
  tgt(right(f1)) == src(right(f3))
  tgt(bottom(f1)) == src(left(f1))   # v3 
  tgt(bottom(f1)) == src(right(f4))
  tgt(left(f1)) == src(top(f1))      # v4
  tgt(left(f1)) == src(right(f5))

  # bottom vertices
  tgt(top(f6)) == src(right(f6))     # v5
  tgt(top(f6)) == tgt(right(f2))
  tgt(right(f6)) == src(bottom(f6))  # v6
  tgt(right(f6)) == tgt(right(f3))
  tgt(bottom(f6)) == src(left(f6))   # v7 
  tgt(bottom(f6)) == tgt(right(f4))
  tgt(left(f6)) == src(top(f6))      # v8
  tgt(left(f6)) == tgt(right(f5))
end
```

This instance defines a box that looks like Figure ??. 

### Opening a closed box

Now, once we deliver the birthday cake, we want be able to open the box so the birthday celebrant can enjoy their sweet treat. We can model this by designing a DPO rule that opens a face of the box.

**[INSERT GIF OF DPO RULE]**

::: {admonition} Opening a box rule
:class: dropdown

```{code-cell}
open_box = @migration(SchRule, Sch3DShape, begin
  L => @join begin
    (face, face1, face2, face3, face4)::Face

    # top edges
    top(face) == top(face1)
    right(face) == top(face2)
    bottom(face) == top(face3)
    left(face) == top(face4)

    # wall faces (clockwise)
    left(face1) == right(face2)
    left(face2) == right(face3)
    left(face3) == right(face4)
    left(face4) == right(face1)

    # top vertices
    src(right(face1)) == src(top(face1))
    src(right(face1)) == tgt(top(face4))
    src(right(face2)) == src(top(face2))
    src(right(face2)) == tgt(top(face1))
    src(right(face3)) == src(top(face3))
    src(right(face3)) == tgt(top(face2))
    src(right(face4)) == src(top(face4))
    src(right(face4)) == tgt(top(face3))

    # bottom vertices
    tgt(right(face1)) == src(bottom(face1))
    tgt(right(face1)) == tgt(bottom(face4))
    tgt(right(face2)) == src(bottom(face2))
    tgt(right(face2)) == tgt(bottom(face1))
    tgt(right(face3)) == src(bottom(face3))
    tgt(right(face3)) == tgt(bottom(face2))
    tgt(right(face4)) == src(bottom(face4))
    tgt(right(face4)) == tgt(bottom(face3))
  end
  K => @join begin
    (face1, face2, face3, face4)::Face
  end
  R => @join begin
    (face1, face2, face3, face4, faceNew)::Face
    (edge1, edge2, edge3)::Edge

    # top edges
    top(faceNew) == top(face4)
    right(faceNew) == edge1
    bottom(faceNew) == edge2
    left(faceNew) == edge3

    # connect vertices of new edges
    src(edge1) == tgt(top(face4))
    tgt(edge1) == src(edge2)
    tgt(edge2) == src(edge3)
    tgt(edge3) == src(top(face4))

    # wall faces (clockwise
    left(face1) == right(face2)     # e5
    left(face2) == right(face3)     # e6
    left(face3) == right(face4)     # e7
    left(face4) == right(face1)     # e8

    # top vertices
    src(right(face1)) == src(top(face1))
    src(right(face1)) == tgt(top(face4))
    src(right(face2)) == src(top(face2))
    src(right(face2)) == tgt(top(face1))
    src(right(face3)) == src(top(face3))
    src(right(face3)) == tgt(top(face2))
    src(right(face4)) == src(top(face4))
    src(right(face4)) == tgt(top(face3))

    # bottom vertices
    tgt(right(face1)) == src(bottom(face1))
    tgt(right(face1)) == tgt(bottom(face4))
    tgt(right(face2)) == src(bottom(face2))
    tgt(right(face2)) == tgt(bottom(face1))
    tgt(right(face3)) == src(bottom(face3))
    tgt(right(face3)) == tgt(bottom(face2))
    tgt(right(face4)) == src(bottom(face4))
    tgt(right(face4)) == tgt(bottom(face3))
  end
  l => begin
    face1 => face1
    face2 => face2
    face3 => face3
    face4 => face4
  end
  r => begin
    face1 => face1
    face2 => face2
    face3 => face3
    face4 => face4
  end
end)
```

Note: `@migration` is formatting the parts of the rule so that `L`, `K`, `R`, `l`, and `r` are clearly identified.
:::

The `L` part of the rule should represent the condition that must be satisfied in order for the rule to be applied. In this case, we want to be able to open a face that is currently shut. A face is considered shut if it is connected to four other faces by four common edges. We can model this by defining a cube that has these qualities. Note: that the edges connecting the walls of the cube are not specified because they are not relevant to the rule.

The `K` part tells me that all the faces the hinging edge are the same between `L` and `R`. Notice that the hindging edge is the edge underlying both sides of this equality, `top(face) == top(face1)`. This is because `top` is a morphism from `Face` to `Edge`, so `top(face)` is some element in `Edge`.

The `R` part of the rule should represent the state in which the open face is connected to only one other face by a common edge that acts as the hinge to the open box. New edges and vertices are added to represent the disconnected edges.

```{code-cell}
open_box_rule = make_rule(openBox, ySch3DShape)
```

<!-- This rule is also well-specified because it will only match on the top face of the box because it considers the orientation of the edges.  -->

<!-- ![](assets/Ch7/BottomOpening.gif) -->

<!-- We can constrain this by defining a specific match for our DPO rule. In AlgebraicJulia, this can be expressed by saying the specific face we would like to match. -->

<!-- ##### TODO: Need to figure out how to get the rule to match such that face = face1
```
match = homomorphisms(L, closedCube)[1]
``` -->

In summary, DPO rewriting can help us model various configurations of a box by manipulating the data in the `Sch3DShape`-Set.

## Example 2: Kitchen World Schema
This machinery can be used to not only represent geometric objects, but it can also be used to capture more abstract semantics, such as the presence of items in a kitchen.

Consider the index category, $\mathsf{C}$, representing the schema for a "kitchen world." In this model, objects include {Food, Egg, Bread, Cheese, BreadSlice, Counter, Kitchenware, Entity}, with morphisms designed to reflect subtype relationships (e.g., Egg, Bread, Cheese, BreadSlice are Food) and spatial relationships (e.g., Counter is on an Entity, Kitchenware is on an Entity, and Food on Entity).

If you recall from [Chapter 4](), objects and morphisms in a schema are eventually mapped to sets and functions, respectively, via $\mathsf{C}$-Set functors. This means that a morphism from Bread to Food says that all elements of Bread are a Food, and likewise, a morphism Food is on an Entity says that all elements of Food are on an Entity. This schema enforces a universal expectation about the types of objects and their arrangements.

```{code-cell}
@present SchKitchen(FreeSchema) begin
  Entity::Ob

  Food::Ob
  food_in_on::Hom(Food, Entity)
  food_is_entity::Hom(Food, Entity)

  Kitchenware::Ob
  ware_in_on::Hom(Kitchenware, Entity)
  ware_is_entity::Hom(Kitchenware, Entity)

  Counter::Ob
  counter_is_entity::Hom(Counter, Entity)

  BreadLoaf::Ob
  bread_loaf_is_food::Hom(BreadLoaf, Food)
  BreadSlice::Ob
  bread_slice_is_food::Hom(BreadSlice, Food)
  Egg::Ob
  egg_is_food::Hom(Egg, Food)
  Cheese::Ob
  cheese_is_food::Hom(Cheese, Food)

  Knife::Ob
  knife_is_ware::Hom(Knife, Kitchenware)
  Plate::Ob
  plate_is_ware::Hom(Plate, Kitchenware)
end
to_graphviz(SchKitchen)

@acset_type Kitchen(SchKitchen)

yKitchen = yoneda(Kitchen, SchKitchen; cache=make_cache(Kitchen, SchKitchen, "Kitchen"))
```

DPO rewriting here can model transformations in the kitchen's state, such as changing the arrangement of items. For example, applying a DPO rewrite rule could simulate the action of combining cheese on a bread slice, altering the morphisms to reflect this new arrangement. Another rule might model the process of putting a slice of bread on a place. This example demonstrates the potential of DPO rewriting for simulating and reasoning about the complex interactions and transformations of objects in a defined space.

### Put Cheese On Bread
Let us take as an example the action of putting cheese on bread. Following the same approach as the previous example, we can define `L`, `R`, and `K` components of this rewrite rule.

```{code-cell}
put_cheese_on_bread = @migration(SchKitchen, begin
  L => @join begin
    cheese::Cheese
    slice::BreadSlice
  end
  K => @join begin
    cheese::Cheese
    slice::BreadSlice
  end
  R => @join begin
    cheese::Cheese
    slice::BreadSlice
    cheese_is_food(cheese) == bread_slice_is_food(slice)  # become one
  end
end)
put_cheese_on_bread_rule = make_rule(put_cheese_on_bread, yKitchen)
```

Note: Relative to our other examples, this schema has substantially more object and morphisms which would require a burdensome amount of syntax to define an `ACSetTransformation` for `l` and `r`. Instead, we can compute the colimit of representables and infer the homomorphism maps, `l` and `r`. This functionality is subsumed in `make_rule()`.

As we can see from this rule, we can model the concept of the bread slice and cheese becoming one by sending `cheese` to the same food element as `bread_slice`. This is a knowledge engineering choice which demonstrates the flexibility of DPO-rewriting rules.

### Plate Slice

We can use the same idea to model a slice of bread being on a plate. 

```{code-cell}
plate_slice = @migration(SchRule, SchKitchen, begin
  L => @join begin
    slice::BreadSlice
    plate::Plate
  end
  K => @join begin
    slice::BreadSlice
    plate::Plate
  end
  R => @join begin
    slice::BreadSlice
    plate::Plate
    food_in_on(bread_slice_is_food(slice)) == ware_is_entity(plate_is_ware(plate))
  end
end)
plate_slice_rule = make_rule(plate_slice, yKitchen)
```

In this case, the bread slice and plate are mapped to the same entity. In the case of the bread slice, the function that does this mapping is tied to the `food_in_on` morphism and, in the case of plate, the function that maps it to entity is tied to the `ware_is_entity` morphism. This is effectively saying that the entity that the food is on is the same entity as the plate. 

#### TODO: Add make_rule to the environment

Both examples illustrate the versatility of double-pushout rewriting in modeling transformations across different contexts. From the reconfiguration of physical structures like cubes to the dynamic arrangement of items in a kitchen, DPO rewriting provides a powerful tool for modeling and simulating changes in systems defined using $\mathsf{C}$-Sets.

