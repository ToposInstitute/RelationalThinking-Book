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

# Chapter 2: Dynamical Systems


In the previous chapter we learned how to input directed graphs into a computer, but those directed graphs didn't *do* anything. In this chapter we'll bring these graphs to life, animating their evolving states over time.

Recall "example 2" (?) from the last chapter, in which our roommate Tuco took over dishes-duty from me and my wife. 

![DGdishes.jpg](https://prod-files-secure.s3.us-west-2.amazonaws.com/c1491185-3815-4f97-a55d-3f414ef619b7/6b943a5f-5028-45c4-93af-2a51094ecd16/DGdishes.jpg)

This model had an implicit notion of *time*. Every day it is *somebody's* turn to the dishes and, with each passing day, this status gets updated. One is meant to "read" the diagram in an active way: Look at who's turn it currently is ("my wife"), then trace along an arrow with your eyes to see who's turn it will be tomorrow ("me"). Then follow an arrow again to see who's turn it will be the day after tomorrow ("my wife"). And so on, ad infinitum.

- Looped GIF of who's turn it is

This is an example of what mathematicians call a **dynamical system** - a system that evolves in time according to formal rules.

But rather than informally reading the system evolution ourselves, we would like to have the computer do it for us. How can we get the computer to simulate this step-by-step updating rule? The underlying directed graph provides a kind of blueprint, capturing the dependencies in our domain of interest. Now we must add some additional information that will allow the simulation process to unfold. 

Each vertex in our system will get loaded with a number of “states” and an “update rule” which dictates how those states change over time.

## Traffic Lights

—> Here we introduce the notion of STATE. 

**On/Off light:**

Let's start with lightbulb (what could be simpler?) with the possible state values being either “on” or “off”.

If you run the following code you will get an output of a lightbulb that is “off”:

- Code snippet with “off” lightbulb

In the above code, set the lightbulb state from “0” to “1” and run it again. Congratulations! You've turned on the light.

Of course we cannot yet call this a “dynamical” system. It's completely static! The lightbulb just stays at whatever we set it to. Let's make this lightbulb change its state over time, flashing on and off.

—> Here we introduce the notion of an UPDATE RULE.

**Flashing light:**

To accomplish this, we add an UPDATE RULE.  Update rules can take many forms, but for our purposes let's make it a 

—> Here we introduce the idea of INTERACTING PARTS

**Two lights:**

In the last example the flashing light was a self-contained system. But one of the interesting things about modeling dynamical systems is our ability to make different parts of the system affect one another - that an update rule can take into account that state of OTHER parts of the system.

So let's take two lightbulbs and have them reference each other's states 

- Copycat / Same (outcome: Static)
- Copycat / Different (outcome: Alternating)
- Opposites / Same (outcome: Mutual Flashing)
- Opposites / Different (outcome: Static)

**String of lights:**

**Single traffic light:**

**Two interacting traffic lights:**

Dynamical systems come in two broad flavors - discrete and continuous. Think a "drum vs. a trombone," "chess vs. billiards" "sugar cubes vs. honey"

The previous examples were all discrete, having binary on/off values changing in distinct time steps. As we mentioned, other dynamical systems of interest might be **continuous**, with states coming in a range of values that fluctuate smoothly in time. ***Compare the on/off nature of a light bulb to the height of a falling rock.*** Such systems can be modeled just as well, as the following example shows.

## Mood Swings

- ** Kiki and Bourba images

Kiki and Bourba are hanging out. Bourba is feeling super grouchy today but luckily being around Kiki tends to improve Bourba's mood. Kiki, on the other hand, is feeling great! Unfortunately, when Bourba is down in the dumps, it really drags down Kiki's mood too. But when Kiki's in a bad mood it only makes Bourba feel *worse*.

How will these friends' moods *interact*? Will Kiki manage to cheer Bourba up or will they both end up depressed? Or maybe their moods will alternate back and forth in and endless cycle of cheering up and bumming out:

- *** Looping GIF of Kiki and Bourba's moods alternating

First, let's describe the interaction of their moods

The mood change is dependent on three **parameters** namely susceptibility factor, tolerance levels and calm down rates.  Parameter values of a process block remain constant (does not change over time). They can be thought of as weights for the variables (inputs and current state) in the computation of dynamics. 

Here is a possible dynamics of mood change in words based on the above assumptions:

> At each instant of time, each friend takes in a certain amount of other person’s mood based on their susceptibility. They also release certain amount of their mood based on the calm down rate. If they reach their excitement or grumpiness maximum tolerance levels, then they just calm down; no more interaction!
> 

Here is a mathematical representation of the above dynamics: 

> if the maximum grumpiness is reached or the maximum excitement is reached then
> 
> 
>       change in mood = - calm down rate x current mood → The amount by which the current mood moves towards the neutral
> 
> else 
> 
> change_in_mood  = - calm down rate x current mood  + susceptibility factor x incoming mood → The amount of the incoming mood taken in. 
> 

This is how the moods of Kiki and Bourba will change over time. The negative signs in the calculation signifies the direction of change — the decrease in grumpiness or excitement. The calm down rates and susceptibility factors of Kiki and Bourba has values between 0 and 1, inclusive.  

> New mood level = change in mood + current mood
> 

## Conclusions

Update rules can be anything - look up tables, logical conditionals, differential equations, automata, recurrence relations, other dynamical systems, etc.?

Preview of more general powerful models in Algebraic Dynamics. Link so Sophie's assets

Dynamical systems and directed graphs are a useful framework for modeling the world. Indeed, they are a close match for how humans tend to conceptualize things. We instinctively look at the world in terms of cause and effect, in terms of procedures which play out over time, in terms of things-acting-upon-other-things. 

But this is just one way of looking at the world. And not all systems are best understood in these terms. In the coming sections we will develop to a more *relational* approach, a subtle and versatile way of working with graphs having an emphasis on constraints and filters instead of step-by-step procedures. Systems that maintain a kind of equilibrium or balancing act between simultaneous constraints.