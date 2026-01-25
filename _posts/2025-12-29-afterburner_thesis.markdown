---
layout: post
read_time: true
show_date: true
title: "PhD Thesis: Afterburner Flame Dynamics"
date: 2025-12-29
img: posts/20251222/veegutter_splash.png
tags: [Dynamics, Experiment, Combustion, Afterburner]
category: research
author: Ryan Blanchard
description: "Designing, building, and validating an afterburner flameholder experiment using LES and Proper Orthogonal Decomposition."
mathjax: true
---


## Why Afterburners Are Hard
Afterburners live in a difficult corner of fluid mechanics: high-speed turbulent flow, intense heat release, strong recirculation, and unsteady pressure coupling. A flameholder has to do two contradictory things at once:

* Create a stable recirculation zone to anchor a flame
* Do as little damage as possible to pressure recovery and thrust

For vee-gutter bluff-body flameholders, stability comes from the wake. Hot combustion products recirculate behind the gutter, ignite incoming reactants, and sustain a flame even when residence times are extremely short. The same wake, however, is inherently unsteady and sheds vortices that can couple to acoustics and lead to thermoacoustic instabilities.

Understanding *how* that wake behaves — and whether simulations reproduce it for the right reasons — was the central motivation of my PhD.

---

## What My Thesis Set Out to Do

Most combustion simulations are "validated" using time-averaged quantities: mean velocities, RMS fluctuations, or single-point spectra. Those metrics are necessary, but they are not sufficient for unsteady reacting flows.

This work asked a more demanding question:

> Can we validate simulations based on the *structure and dynamics* of the dominant flow and heat-release modes?

To answer that, I designed and built a dedicated afterburner-scale experiment and paired it with Large Eddy Simulations (LES). Proper Orthogonal Decomposition (POD) was then used as the common language between experiment and computation.

---

## The Experimental Rig

The experimental setup was built from scratch in the Combustion Systems Dynamics Lab at Virginia Tech. It consisted of:

* A modular fuel-injection section
* A long converging section to control boundary-layer development
* A rectangular test section with optical access
* A 70° vee-gutter flameholder spanning the duct

High-speed diagnostics included:

* **Stereo Particle Image Velocimetry (PIV)** for velocity fields
* **Filtered chemiluminescence imaging** as a proxy for heat release

The rig was designed to operate in both non-reacting (cold-flow) and reacting regimes, allowing a controlled progression from pure fluid mechanics to fully coupled combustion dynamics.

---

## Large Eddy Simulation

On the computational side, LES models were developed in both **ANSYS Fluent** and **OpenFOAM**, matched as closely as possible to the experiment.

At the heart of LES is a spatial filtering of the Navier–Stokes equations:
<div class="math-boundary">
$$
\frac{\partial \bar{\rho}}{\partial t} + \nabla \cdot (\bar{\rho} \tilde{\mathbf{u}}) = 0
$$
</div>

<div class="math-boundary">
$$
\frac{\partial \bar{\rho} \tilde{\mathbf{u}}}{\partial t} + \nabla \cdot (\bar{\rho} \tilde{\mathbf{u}} \tilde{\mathbf{u}}) = -\nabla \bar{p} + \nabla \cdot (\bar{\tau} + \tau_{\text{sgs}})
$$
</div>

Unresolved turbulence was modeled using dynamic Smagorinsky-type closures, with grid resolution chosen so that the majority of turbulent kinetic energy was resolved rather than modeled.

A key validation criterion was the **Pope criterion**, ensuring that modeled subgrid energy remained a small fraction of the resolved energy:

<div class="math-boundary">
$$
M = \frac{k_{\text{sgs}}}{k_{\text{resolved}}} \lesssim 0.2
$$
</div>

![Large Eddy Simulation](assets/img/posts/20251222/fig-1.png)
*Instantaneous LES velocity magnitude showing alternating vortex shedding*

---

## Proper Orthogonal Decomposition (POD)

POD provided the bridge between experiment and simulation.

Given a set of instantaneous snapshots assembled into a data matrix $ \mathbf{U} $, POD seeks a low-dimensional representation:

<div class="math-boundary">
$$
\mathbf{U} \approx \sum_{i=1}^N a_i(t) \, \boldsymbol{\phi}_i(\mathbf{x})
$$
</div>

<div markdown="1">

where:

- $ \boldsymbol{\phi}_i $ are spatial modes
- $ a_i(t) $     are temporal coefficients
- Each mode represents a coherent physical structure

</div>

Crucially, POD is agnostic to data source. Experimental measurements and LES fields can be decomposed and compared mode-by-mode.

---

## Cold Flow: Wake Dynamics

In the non-reacting case, POD isolated the dominant vortex-shedding mode of the vee-gutter wake. The shedding frequency collapsed to a Strouhal number of approximately:

<div class="math-boundary">
$$
\mathrm{St} = \frac{f w}{U} \approx 0.24
$$
</div>

The first POD modes from PIV, Fluent, and OpenFOAM showed strong agreement in:

* Spatial structure
* Amplitude
* Streamwise evolution

This went well beyond matching mean velocity profiles and demonstrated that the simulations were capturing the *right physics* of the wake.

![Cold Wake](assets/img/posts/20251222/fig-2.png)
*POD modes of the cold flow PIV data (top row) and CFD simulations (Fluent middle OpenFOAM bottom)*

---

## Reacting Flow: Heat Release Dynamics

For reacting cases, validation became more subtle. Chemiluminescence measurements are line-of-sight integrals, while LES produces local volumetric heat release.

To make the comparison meaningful, the LES heat release was integrated numerically along synthetic lines of sight:

<div class="math-boundary">
$$
I(x,y) = \int \dot{q}^{'''}(x,y,z) \, dz
$$
</div>


POD of these integrated fields revealed:

* A dominant symmetric shedding mode
* Higher-frequency harmonic modes
* A low-frequency, low-energy mode invisible to time-averaged analysis

The agreement in mode shape, frequency, and relative energy between experiment and LES provided strong evidence that the simulations were dynamically faithful.

![Hot Wake](assets/img/posts/20251222/fig-3.png)
*POD modes of the heat release chemiluminescence data (left) and CFD simulation in Fluent (right)*

---

## Why This Matters

The most important outcome of this work was methodological.

By validating **dynamic modes**, not just statistics, we gain confidence that LES models can be trusted to explore:

* Confinement effects
* Boundary condition sensitivity
* Transitions between symmetric and asymmetric flame dynamics

This is essential for predicting combustion instability risk, flameholder performance, and operability limits in real engines.

## So What?

My PhD sat deliberately at the intersection of **design, experiment, simulation, and data-driven analysis**. Building the hardware mattered. Writing the solvers mattered. But the real leverage came from learning how to ask better validation questions.

That mindset — validating *structure*, not just numbers — continues to shape how I approach complex engineering problems today.

