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

# Sample Markdown with Code

The kernel runs **Julia 1.10.0**

**Installed Packages:** graphviz

**Supported Julia libraries:** \
AlgebraicDynamics v0.2.2
AlgebraicRewriting v0.3.0
Catlab v0.16.5
LabelledArrays v1.15.0
OrdinaryDiffEq v6.66.0
Plots v1.40.0
PrettyTables v2.3.1
SciMLBase v2.10.0

Each of the code blocks are editable

+++

## How to run the code in this page?

This page allows live code execution. To run the code below, click the "rocket icon" at the top right corner of this page, and select "Live code". This will invoke a pre-configured kernel from mybinder.org. Configuration details given above. 

:::{note}
The kernel may take sometime to build !! 
:::

If you would like to see the detailed status of the kernel invokation, select "Binder" under the rocket icon. 

+++

## Julia code

+++

```{code-cell}
my_hidden_variable = "Testing Julia!"
print( my_hidden_variable )
```

+++

## Drawing directed wiring diagrams

The below code generates a directed wiring diagram with two boxes each with one input and one output. The output of one each box is connected to the other. 

+++

```{code-cell}
# Example directed wiring diagram

using Catlab.WiringDiagrams, Catlab.Programs, Catlab.Graphics

mood_pattern = WiringDiagram([], [:friend1_mood, :friend2_mood])
friend_1_box = add_box!(mood_pattern, Box(:friend1, [:my_mood], [:external_mood]))
friend_2_box = add_box!(mood_pattern, Box(:friend2, [:my_mood], [:external_mood]))

add_wires!(mood_pattern, Pair[
    (friend_1_box, 1) => (friend_2_box, 1),
    (friend_2_box, 1)    => (friend_1_box, 1),
    (friend_1_box, 1) => (output_id(mood_pattern), 1),
    (friend_2_box, 1)    => (output_id(mood_pattern), 2)
])

# Displaying the Directed Wiring Diagram using graphviz

draw(d::WiringDiagram; labels=true) = to_graphviz(d,
  orientation=LeftToRight,
  labels=labels, label_attr=:xlabel
)

draw(mood_pattern, labels=true)
```

+++

## Drawing undirected wiring diagrams

The following code generates a UWD of Rabbit-Fox predation

+++

The following code draws UWD using Graphviz library

+++

```{code-cell}
# Example undirected wiring diagram 
using Catlab.WiringDiagrams, Catlab.Programs, Catlab.Graphics

# Define the composition pattern
rf = @relation (rabbits,foxes) begin
    growth(rabbits) # box_name(junction_name)
    predation(rabbits,foxes)
    decline(foxes)
end

# Displaying the undirected Wiring Diagram
to_graphviz(rf, box_labels=:name, junction_labels=:variable)
```

+++

Try your own code in the block below:

+++

```{code-cell}
# Put code here
```




