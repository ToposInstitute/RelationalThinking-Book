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

