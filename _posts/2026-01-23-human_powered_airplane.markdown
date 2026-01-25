---
layout: post
read_time: true
show_date: true
title: "Designing a Human-Powered Aircraft"
date: 2025-12-25
img: posts/20260123/human_powered_aircraft_splash.png
tags: [Aerospace, Design, Human Powered, Fixed Wing]
category: research
author: Ryan Blanchard
description: "A lifelong idea, finally approached with real engineering tools."
mathjax: true
---

## A Childhood Idea That Never Went Away

Some engineering projects start with a problem statement.  
This one started with a PBS documentary.

I was a kid when I first saw a program about the **Daedalus Project**, the human-powered aircraft that successfully flew from Crete to Santorini. I don’t remember all the details, but I *do* remember what it did to my imagination. Suddenly, flying didn’t feel like something reserved for jet engines and control towers — it felt almost personal. Human. Attainable.


<iframe width="560" height="315" src="https://www.youtube.com/embed/4pbKp7ufwns" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


Not long after, I started sketching designs for a human-powered helicopter that my friends and I could pedal up into the nearby mountains for a camping trip. Needless to say, the physics were… optimistic.

Around the same time, a PBS science show flashed an equation on the screen:

<div class="math-boundary">
$$
F_L = \tfrac{1}{2}\rho U^2 C_L A
$$
</div>

I remember scrambling to find out when the episode would re-air so I could be ready with pen and paper. I copied it down, stared at it, and immediately realized I was in way over my head. I didn’t know what density was. I definitely didn’t know how anyone could possibly determine $C_L$.

But that equation stuck.

---

## Coming Back With Better Tools

Decades later, that same equation still sits at the heart of this project — but now it’s surrounded by tools I didn’t have back then.

Designing a human-powered aircraft is a brutally constrained problem:

- **Very low available power** (on the order of 200–300 W sustained)
- **Very low flight speeds**
- **Extremely low Reynolds numbers**
- **Severe mass and structural constraints**

Every design decision is coupled. Wing area drives drag. Drag drives power. Power caps speed. Speed feeds directly back into lift.

The basic lift requirement is still simple:

<div class="math-boundary">
$$
L = W
$$
</div>

But achieving that lift efficiently at walking-to-jogging speeds is anything but.

This project is where all of the threads from earlier posts come together:  
airfoil analysis with **XFOIL**, wing design and induced drag management, propeller optimization for static and low-speed thrust, and an unapologetically iterative design-build mindset.

---

## The Configuration

The current concept is a **tractor configuration** with:

- A very high aspect ratio wing
- Carefully managed twist distribution
- A lightweight fuselage nacelle just large enough for the pilot and drivetrain
- A propeller optimized specifically for low-speed efficiency

The aircraft is designed around *the pilot*, not the other way around. The seating position, drivetrain geometry, and center of gravity placement are all chosen to minimize trim drag and structural penalties.

![Isometric view of the human-powered aircraft](assets/img/posts/20260123/fig1_iso.png)
*Isometric CAD rendering showing overall configuration and pilot integration*

---

## Designing for Low Reynolds Number Flight

At the speeds a human can realistically sustain, Reynolds numbers are typically an order of magnitude lower than conventional aircraft. That changes everything:

<div class="math-boundary">
$$
\mathrm{Re} = \frac{\rho U c}{\mu}
$$
</div>

At low Reynolds numbers:

- Laminar separation becomes a dominant concern
- Small changes in airfoil geometry have outsized effects
- Empirical intuition from conventional aircraft breaks down

This is why tools like **XFOIL** are indispensable. They allow me to go from *geometry* to *aerodynamic performance* and close the loop between concept and reality.

But airfoils are only half the story.

---

## Wing Planform and Structural Reality

The wing planform is driven by two competing goals:

- Minimize induced drag through a near-elliptical lift distribution
- Maintain a structure that can actually be built and carried

Twist is used aggressively to shape the spanwise loading while keeping root bending moments manageable. The wing is broken into discrete sections that balance aerodynamic idealism with structural pragmatism.

![Front view of the aircraft](assets/img/posts/20260123/fig2-front.png)
*Front view highlighting extreme aspect ratio and spanwise twist*

![Top view of the aircraft](assets/img/posts/20260123/fig3-top.png)
*Top view showing planform, control surfaces, and wing segmentation*

---

## Why This Project Matters (to Me)

This isn’t about breaking records or chasing headlines.

It’s about finally revisiting an idea that’s been quietly present for most of my life — but doing it with the right tools, the right physics, and the right level of respect for how hard the problem actually is.

Every part of this aircraft exists because of something I once struggled to understand:

- That mysterious lift equation
- Why drag matters more than you think
- Why efficiency is a system-level property
- Why building hardware teaches you things simulations never will

This project sits at the intersection of **curiosity, discipline, and persistence** — and for me, that’s where the most meaningful engineering lives.

There’s still a long way to go. But for the first time, this isn’t a sketch in a notebook.

It’s an airplane.
