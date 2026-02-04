---
layout: post
read_time: true
show_date: true
title: "Optimization Is Easy. Posing the Problem Is Hard."
date: 2025-11-21
img: posts/20251224_optimize/gradients.png
tags: [Optimization, CFD, Algorithms, Engineering]
category: research
author: Ryan Blanchard
description: "Three lessons on optimization—from Excel Solver to CFD—about why problem formulation matters more than algorithms."
mathjax: true
---

## Why Optimization Hooked Me Early

I’ve been obsessed with optimization for as long as I’ve known it existed.

As a freshman undergraduate, a friend showed me **Excel Solver**. I don’t even remember what we were trying to optimize — I just remember the feeling that *a computer could search a design space for you*. That single demo permanently rewired how I thought about engineering problems.

Not long after, I signed up for a **graduate-level optimization algorithms course as a sophomore**, before I had even taken linear algebra. I was wildly underprepared mathematically, but completely hooked intellectually.

That semester, I applied *every* algorithm we learned to whatever engineering problem I could get my hands on:

- Generalized Reduced Gradient (GRG)
- Branch and Bound
- Simulated Annealing
- Simplex and multi-objective methods
- Genetic algorithms

If something had tunable parameters, I tried to optimize it.

At the time, I thought optimization was about finding the *best* solution.

What I actually learned — slowly, painfully, and repeatedly — is that **optimization is mostly about finding the weaknesses in your own thinking**.

---

## Formula SAE: When the Algorithm Beats You Instead

[Formula SAE](https://ryanblanchard.io/formulaSAE.html) racecar design was my first real optimization playground .

I used optimization scripts for:
- Suspension kinematics (minimizing bump steer and scrub radius while hitting camber gain targets)
- Inlet plenum and choke orifice geometry
- Tradeoffs between packaging, stiffness, and manufacturability

I quickly learned something uncomfortable:  
> **Optimization algorithms are very good at exploiting unphysical assumptions.**

If you let them, they will:
- Drive variables to trivial limits
- Exploit numerical discontinuities
- “Win” the objective while violating the spirit of the design

This wasn’t a failure of the algorithms.  
It was a failure of **problem formulation**.

That lesson would come back later, much harder, and much more consequential.

---

## Graduate School: A Chemical Equilibrium Solver from Hell

In grad school, I was given what remains one of the best homework assignments I’ve ever seen.

We were asked to write a **chemical equilibrium solver** for an arbitrary hydrocarbon–air mixture over a wide range of temperatures and equivalence ratios.

The grading rubric was brutal and honest:

- **D**: Converges for *hot + lean* (easy)
- **C**: Also converges for *hot + rich*
- **B**: Also converges for *cold + lean*
- **A**: Converges for *cold + rich*

The professor warned us:
- He had tried Excel Solver himself — and failed
- Most numerical solvers would fail catastrophically
- No software was off-limits

The system consisted of:
- 7 reversible reactions with temperature-dependent equilibrium constants
- Elemental conservation of C, H, O, and N
- 11 nonlinear equality constraints

The kicker:  
Some species concentrations varied by **nearly 40 orders of magnitude** across conditions.

Most of the class tried MATLAB or Python optimization libraries.  
They all failed.

The equations were smooth and continuous — perfect for gradient-based methods — but *numerically hostile*.

---

## The Trick: Separate the Physics from the Solver

The breakthrough came from reframing the problem entirely.

Instead of solving directly for mole fractions, I introduced **dummy optimization variables**:
<div class="math-boundary">
$$
x_i \;\;\rightarrow\;\; y_i = 10^{x_i}
$$
</div>

The solver operated in the space of $\( x_i \)$, while the physical mole fractions were computed as $\( y_i \)$.

This did three things at once:

1. All optimization variables lived on a similar numerical scale  
2. Finite-difference gradients could use a **single, large step size**
3. The solver no longer “saw” the 40-order-of-magnitude problem

I also removed one degree of freedom by enforcing:
<div class="math-boundary">
$$
X_{N_2} = 1 - \sum_{i \neq N_2} X_i
$$
</div>
The result:
- The solver converged **across the full operating space**
- It outperformed the commercial equilibrium solver we were benchmarking against
- I was the only student in the class to get an **A**

The professor later asked if he could use my Excel-based solver for his diesel engine research.

That was the moment I truly internalized this rule:

> **Optimization lives or dies by how you pose the problem.**

---

## Industry Scale: CFD Optimization Where Mistakes Cost Millions

Years later, that lesson reappeared in a very different context.

I worked on a **radiant syngas cooler** — a massive heat exchanger roughly the size of the Statue of Liberty, built from expensive nickel-based alloys. Shrinking it even slightly without sacrificing performance could save **millions of dollars**.

The objectives:
- Minimize volume (cost surrogate)
- Maintain heat recovery (duty)
- Do not increase peak heat flux (PHF)

The design variables included:
- Vessel diameter and height
- Platen width and length
- **Number of platens** — an integer

At first glance, this seems incompatible with gradient-based optimization.

But the CFD model used a **periodic wedge**, which meant the effective number of platens could be treated as *continuous* during sensitivity analysis.

That single modeling decision unlocked everything.

---

## Making Gradients Behave in Noisy CFD

Each CFD solve took ~8 hours. Gradients were expensive and noisy.

So we borrowed another trick from the equilibrium solver:
- All geometric parameters were perturbed by **1% of their baseline value**
- Not a fixed absolute step

This ensured:
- Comparable numerical meaning across variables
- Step sizes large enough to overcome solver noise
- Stable finite-difference gradients

Multiple workstations ran simulations in parallel, allowing full gradient and line-search evaluations over a single weekend.

---

## Choosing a Direction That Physics Allows

The gradients told a clear story:

- Cost and duty gradients nearly opposed each other
- PHF pointed somewhere else entirely

So instead of following steepest descent blindly, we projected the cost gradient onto the plane normal to the duty gradient:
<div class="math-boundary">
$$
\mathbf{d} = \mathbf{g}_{\text{cost}} -
\frac{\mathbf{g}_{\text{cost}} \cdot \mathbf{g}_{\text{duty}}}
     {\|\mathbf{g}_{\text{duty}}\|^2}
\mathbf{g}_{\text{duty}}
$$
</div>

By luck — and physics — this direction also slightly *reduced* PHF.

After two gradient + line-search iterations:
- The integer platen count was rounded
- One final reduced optimization was run

Total cost:
- **35 CFD simulations**
- ~10 days of compute time
- Completed in a single weekend via parallelism

The cooler got smaller.  
Duty held.  
PHF dropped.

---

## What All of This Taught Me

I’ve used a lot of optimization algorithms over the years.

What matters far more than the algorithm is:

- Variable scaling
- Constraint formulation
- Physical insight
- Numerical humility

Optimization is not about asking *“what’s the best answer?”*  
It’s about asking *“what question can the solver actually answer honestly?”*

That mindset — from Excel Solver, to equilibrium chemistry, to industrial CFD — is how I approach complex engineering problems today.
