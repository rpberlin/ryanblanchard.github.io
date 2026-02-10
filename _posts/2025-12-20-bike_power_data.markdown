---
layout: post
read_time: true
show_date: true
title: "Bikes, Sensors, and Differential Equations"
date: 2025-11-27
img: posts/20251220/bikepowerdata.png
tags: [Cycling, Modeling, Optimization, Python]
category: research
author: Ryan Blanchard
description: "From a Perl script to a Python toolchain: fitting an ODE heart-rate model from power-meter data, then simulating and eventually optimizing race pacing."
mathjax: true
---

## The Rabbit Hole Started With $P = \tau\,\omega$

Cycling is one of those rare sports where the *input* can be measured almost directly. The crank doesn’t care about vibes,  power is power:

<div class="math-boundary">
$$
P(t) = \tau(t)\,\omega(t)
$$
</div>

When I bought my first road bike, I tried to estimate power the hard way: stopwatch timing between traffic lights, a hand-wavy drag estimate, and some back-of-the-envelope aero. It was surprisingly close on flat roads… until I bought the “real” sensors: GPS, heart rate, and eventually a power meter.

Suddenly I had the kind of data stream my inner nerd can’t ignore: speed, elevation, slope, cadence, heart rate, and *power*  all synchronized.

And of course that immediately turned into a coding project.

---

## From a Perl Script to a Python Toolchain

This project started years ago as a **Perl script** (I know…), and has gradually evolved into a more mature set of Python scripts.

If you want to poke around the code, it lives in this [GitHub Repo](https://github.com/rpberlin/Python_bin/tree/main/pedal_power)

The current goal is simple to describe but tricky to do well:

> Take a time history of measured power and predict the heart-rate response with a small, physically-motivated model and then fit the model parameters to real rides.

---

## The Data: Five Channels That Tell the Whole Story

A typical ride provides multiple coupled signals: speed changes alter aerodynamic load, grade changes alter gravitational work, cadence shifts alter how you deliver power, and heart rate reacts with both delay and drift.

![Ride data channels](assets/img/posts/20251220/data_channels.png)
*Typical ride time histories: speed, elevation, power, cadence, and heart rate.*

---

## A Heart-Rate Model as an ODE

At the core is a first-order dynamic system: heart rate evolves continuously in time in response to current power output *and* accumulated “stress.”

<div class="math-boundary">
$$
\frac{d\,HR}{dt}
=
F\!\left(
P(t),\, P_{\max},\, P_{\text{low}},\, \tau_{\text{decay}},\, n,\,
HR_{\max},\, HR_{\text{low}},\, S(t)
\right)
$$
</div>

Where:

- $HR(t)$ is heart rate (BPM)
- $P(t)$ is measured power (W)
- $P_{\max},\,P_{\text{low}}$ anchor power capability
- $HR_{\max},\,HR_{\text{low}}$ bound physiological response
- $\tau_{\text{decay}}$ sets recovery time
- $n$ governs nonlinear fatigue accumulation
- $S(t)$ is an accumulated stress state

---

## Stress as a Filtered Power Integral

The stress variable evolves as an integral of power:

<div class="math-boundary">
$$
S(t) = \int_{0}^{t} \left(\frac{P(t)-P_{\text{low}}}{P_{max}-P_{\text{low}}}\right)^n dt
$$
</div>



The heart rate then relaxes toward a power- and stress-dependent target:

<div class="math-boundary">
$$
\frac{dHR}{dt}
=
\frac{1}{\tau_{HR}}
\left(
HR_{\text{target}}(P,S) - HR
\right)
$$
</div>

---

## Parameter Fitting as an Optimization Problem

Given a ride with $N$ samples, the model is simulated forward using the measured power input. The parameters $\theta$ are chosen to minimize mismatch with measured heart rate.

<div class="math-boundary">
$$
\theta =
\left[
P_{\max},\, P_{\text{low}},\, \tau_{\text{decay}},\, n,\,
HR_{\max},\, HR_{\text{low}}, \ldots
\right]
$$
</div>

### Weighted RMS Objective

<div class="math-boundary">
$$
J(\theta)
=
\sqrt{
\frac{1}{N}
\sum_{i=1}^{N}
w(HR_{\text{meas},i})
\left(
HR_{\text{model},i}(\theta)
-
HR_{\text{meas},i}
\right)^2
}
$$
</div>

Where the weight emphasizes accuracy at high heart rates:

<div class="math-boundary">
$$
w(HR)
=
1
+
\alpha
\left(
\frac{HR - HR_{\text{low}}}
     {HR_{\max} - HR_{\text{low}}}
\right)^p
$$
</div>

---
So now I have a model of how my own heart rate responds to the power I'm trying to generate with my legs.
![Heart Rate Model Fitting](assets/img/posts/20251220/HRify_output.png)
*Fitted Heart Rate model versus data from my heart rate monitor*

## Optimization: Making It Fast *and* Robust

My first attempt used a hand-rolled gradient method. It worked, but it was slow and sometimes brittle.

Two changes made this practical:

- Renormalizing parameters to comparable numerical scales
- Switching to **Nelder–Mead** via SciPy

The result: a two-hour ride (~7200 samples) fits in about **30 seconds**, down from ~10 minutes, with much better robustness.

---

## What the Model Enables

Once fitted, the model lets me:

- Track fitness changes over time
- Observe reduced heart-rate drift for the same stress load
- Compare rides in a way that’s not tied to any single metric

And it unlocks the next step.

---

## Simulating Race Performance

With:

- a fitted HR–power model,
- GPS elevation data,
- a physics-based bike model,
- and a rule-based pacing strategy,

I can simulate an entire course and predict speed, power, and heart rate histories.

![Race reconstruction](assets/img/posts/20251220/bikepowerdata.png)
*Measured versus simulated race performance.*

---

## The Next Step: Optimizing Pacing

Ultimately, this becomes an optimal control problem:

<div class="math-boundary">
$$
\min_{P(t)} \; T
\quad
\text{s.t.}
\quad
\dot{HR} = f(HR,P,S),\;
\dot{S} = g(S,P),\;
HR \le HR_{\max},\;
0 \le P \le P_{\max}
$$
</div>

Choosing *when* to spend power is often more important than how much power you have.

---

## Closing Thought

This project started with a single equation and grew into a modeling and optimization framework that blends physics, physiology, and numerical methods.

It’s also a reminder of why I love engineering: once intuition becomes a model, it becomes something you can test, refine, and reuse.

---
