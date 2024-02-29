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

# Chapter 6: Double-Pushout Rewriting Applications

As we have seen, Double-pushout (DPO) rewriting is a concept that gives us access to the feature of adding and deleting elements in our data. At its core, DPO rewriting involves the use of two pushout diagrams in the $\mathsf{C}$-Set category that transforms one $\mathsf{C}$-Set object to another. This process can model a wide range of changes in schema-backed data, from the reconfiguration of physical objects to transformations in world knowledge.

## Example 1: Cube Configuration
Consider a scenario where the index category, $\mathsf{C}$, represents the schema for a cube, comprising three fundamental objects: `Face`, `Edge`, `Vertex`. This schema is enriched with six non-identity morphisms: `top`, `bottom`, `left`, `right`, `src`, `tgt`, which define the relationships between these objects. DPO rewriting in this context can model various transformations in the cube's configuration, akin to unfolding a cardboard box. 

This can expressed as a schema in AlgebraicJulia as follows:

```{code-cell}
@present SchCube(FreeSchema) begin
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
to_graphviz(SchCube)
@acset_type Cube(SchCube)
```

For instance, using DPO rewriting, we could model the action of opening or closing the top of the cube. This operation would involve redefining the relationships between the Faces and Edges objects to "remove" the connections that form the top face. Similarly, unfolding the cube into a flat layout would radically alter the connections between Faces, Edges, and Vertices to represent the cube in an unfolded state. Such transformations are powerful for visualizing and reasoning about the structural possibilities of objects in three-dimensional space.

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

We can create an instance of a cube, by defining such a $$\mathsf{C}$-Set, where $$\mathsf{C}$$ is `SchCube`.

```{code-cell}
closedCube = @acset_colim ySchCube begin
  (f1, f2, f3, f4, f5, f6)::Face
  (e1, e2, e3, e4, e5, e6, e7, e8, e9, e10, e11, e12)::Edge
  (v1, v2, v3, v4, v5, v6, v7, v8)::Vertex

  top(f1) == e1; right(f1) == e4; bottom(f1) == e3; left(f1) == e2
  top(f2) == e8; right(f2) == e7; bottom(f2) == e5; left(f2) == e4
  top(f3) == e11; right(f3) == e7; bottom(f3) == e6; left(f3) == e10
  top(f4) == e12; right(f4) == e10; bottom(f4) == e9; left(f4) == e2
  top(f5) == e11; right(f5) == e8; bottofm(f5) == e1; left(f5) == e12
  top(f6) == e6; right(f6) == e5; bottom(f6) == e3; left(f6) == e9

  src(e1) == v1; tgt(e1) == v2
  src(e2) == v1; tgt(e2) == v3
  src(e3) == v3; tgt(e3) == v4
  src(e4) == v4; tgt(e4) == v2
  src(e5) == v4; tgt(e5) == v8
  src(e6) == v7; tgt(e6) == v8
  src(e7) == v8; tgt(e7) == v6
  src(e8) == v6; tgt(e8) == v2
  src(e9) == v7; tgt(e9) == v3
  src(e10) == v7; tgt(e10) == v5
  src(e11) == v6; tgt(e11) == v5
  src(e12) == v5; tgt(e12) == v1
end
```

This instance defines a box that looks like Figure ??. 

**[INSERT FIGURE OF LABELED CUBE]**

Now, once we deliver the birthday cake, we want be able to open the box so the birthday celebrant can enjoy their sweet treat. We can model this by designing a DPO rule that opens a face of the box.

The `L` part of the rule should represent the condition that must be satisfied in order for the rule to be applied. In this case, we want to be able to open a face that is currently shut. A face is considered shut if it is connected to four other faces by four common edges. We can model this by defining a cube that has these qualities. 

```{code-cell}
L = @acset_colim ySchCube begin
  (face, face1, face2, face3, face4)::Face

  top(face) == top(face1)
  right(face) == top(face2)
  bottom(face) == top(face3)
  left(face) == top(face4)
end
```

The `R` part of the rule should represent the state in which the open face is connected to only one other face by a common edge that acts as the hinge to the open box.

```{code-cell}
R = @acset_colim ySchCube begin
  (face, face1)::Face

  top(face) == top(face1)
end
```

The `K` part tells me that open face and the face that it is attached to are the same. They are also attached by the same edge.

```{code-cell}
K = @acset_colim ySchCube begin
  (face, face1)::Face

  top(face) == top(face1)
end
l = homomorphisms(K, L)[2]
r = ACSetTransformation(K, R, Face=[1, 2], Edge=collect(1:7), Vertex=collect(1:14))
rule = Rule{:DPO}(l, r)
```

This rule is helpful, but the problem is that we can use it on _any_ face of the box. This means that we can technically open the box from the bottom, releasing the cake to the floor. We can constrain this by defining a specific match for our DPO rule. 

```
match = homomorphisms(L, closedCube)[1]
```

## TODO: Need to figure out how to get the rule to note match such that face = face1

In summary, DPO rewriting can help us model various configurations of a box by manipulating the data in the `SchCube`-Set.

## Example 2: Kitchen World Schema
In a more abstract and complex domain, consider the index category, $\mathsf{C}$, representing the schema for a "kitchen world." In this model, objects include {Food, Egg, Bread, Cheese, BreadSlice, Counter, Kitchenware, Entity}, with morphisms designed to reflect both subtype relationships (e.g., Egg, Bread, Cheese, BreadSlice to Food) and spatial relationships (e.g., Counter to Entity, Kitchenware to Entity, and Food to Entity). This schema captures not just the types of objects present in a kitchen but also their potential arrangements and interactions.

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
  CheeseBag::Ob
  cheeseBag_is_food::Hom(CheeseBag, Food)
  Cheese::Ob
  cheese_is_food::Hom(Cheese, Food)

  Knife::Ob
  knife_is_ware::Hom(Knife, Kitchenware)
  Plate::Ob
  plate_is_ware::Hom(Plate, Kitchenware)
end
to_graphviz(SchKitchen)

@acset_type Kitchen(SchKitchen)
```

DPO rewriting here can model transformations in the kitchen's state, such as changing the arrangement of items. For example, applying a DPO rewrite rule could simulate the action of placing cheese on a bread slice, altering the morphisms to reflect this new arrangement. Another rule might model the process of slicing bread, transforming a Bread object into multiple BreadSlice objects and updating the relationships accordingly. This example demonstrates the potential of DPO rewriting for simulating and reasoning about the complex interactions and transformations of objects in a defined space.

Both examples illustrate the versatility of double-pushout rewriting in modeling transformations across different contexts. From the reconfiguration of physical structures like cubes to the dynamic arrangement of items in a kitchen, DPO rewriting provides a powerful tool for modeling and simulating changes in systems defined using $\mathsf{C}$-Sets.

