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
@acset_type 3DShape(Sch3DShape)
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

### A closed box

We can create an instance of a cube, by defining such a $\mathsf{C}$-Set, where $\mathsf{C}$ is `Sch3DShape`.

```{code-cell}
closedCube = @acset_colim ySch3DShape begin
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
L = @acset_colim ySch3DShape begin
  (face, face1, face2, face3, face4)::Face

  top(face) == top(face1)
  right(face) == top(face2)
  bottom(face) == top(face3)
  left(face) == top(face4)
end
```

The `R` part of the rule should represent the state in which the open face is connected to only one other face by a common edge that acts as the hinge to the open box.

```{code-cell}
R = @acset_colim ySch3DShape begin
  (face, face1)::Face

  top(face) == top(face1)
end
```

The `K` part tells me that open face and the face that it is attached to are the same. They are also attached by the same edge.

```{code-cell}
K = @acset_colim ySch3DShape begin
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

##### TODO: Need to figure out how to get the rule to match such that face = face1

In summary, DPO rewriting can help us model various configurations of a box by manipulating the data in the `Sch3DShape`-Set.

## Example 2: Kitchen World Schema
In a more abstract and complex domain, consider the index category, $\mathsf{C}$, representing the schema for a "kitchen world." In this model, objects include {Food, Egg, Bread, Cheese, BreadSlice, Counter, Kitchenware, Entity}, with morphisms designed to reflect subtype relationships (e.g., Egg, Bread, Cheese, BreadSlice are Food) and spatial relationships (e.g., Counter is on an Entity, Kitchenware is on an Entity, and Food on Entity).

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

