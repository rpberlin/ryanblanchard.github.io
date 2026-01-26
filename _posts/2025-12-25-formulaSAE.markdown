---
layout: post
read_time: true
show_date: true
title: "Starting a Formula SAE Team from Scratch"
date: 2025-12-25
img: posts/20251225/fsae_splash.png
tags: [Motorsport, Design, CAD, FEA, Systems Engineering]
category: research
author: Ryan Blanchard
description: "How building a Formula SAE car from nothing shaped how I think about engineering, design, and learning by doing."
mathjax: false
---

## What Formula SAE Is

Formula SAE is a student engineering competition where teams design, build, and race a small, open-wheel formula-style car. On paper it’s a motorsports competition. In reality, it’s one of the most intense systems-engineering learning environments you can experience as an undergraduate.

Teams are judged not just on lap time, but on:
- Engineering design
- Manufacturing quality
- Cost and business case
- Reliability and execution

![FSAE final](assets/img/posts/20251225/fsae_splash.png)
*The final car*

Most established teams iterate year-to-year, refining designs using previous cars as testbeds. That luxury mattered a lot to us — because we didn’t have it.

---

## One F1 Obsession and One Physics Brain

The project started with a friend of mine who was, in the best possible way, an F1 obsessive. He didn’t just *watch* Formula 1 — he lived it. Telemetry, suspension geometry, aero trade studies, team politics. All of it.

I came from a different angle. I loved the physics: fluid flow, dynamics, structures, control. Formula SAE immediately struck me as the ultimate forcing function — a way to turn abstract coursework into something brutally real.

Together, we convinced our university to approve Formula SAE as a senior capstone project. That approval came with a modest budget and, more importantly, legitimacy. By the end, about **20 undergraduate students** were involved in designing, building, testing, and driving the car.

---

## A Lens for Learning

I easily spent **twice as much time on Formula SAE as on all my coursework combined** — and it paid off in ways no class ever could.

Every new concept suddenly mattered:
- Fluid flow → intake and cooling
- System dynamics → suspension and driveline
- CAD → everything
- Design for manufacturing → everything again
- Kinematics → suspension geometry
- Instrumentation → testing and debugging
- Statistics → data interpretation
- FEA and CFD → structural and aero intuition
- Optimization → tradeoffs everywhere

Formula SAE gave me a *lens*. Instead of learning topics in isolation, every lecture felt like it could help make a better race car.

---

## CAD First, Simulation Later (If Ever)

We realized very quickly that **CAD was non-negotiable**. Everything had to fit. Everything interacted. Everything had consequences.

The car’s structure was designed entirely by students, balancing stiffness, mass, manufacturability, and safety.

![FSAE CAD Rendering](assets/img/posts/20251225/fsae_CADjpg.jpg)
*Early CAD rendering of the Formula SAE car, showing chassis layout, suspension geometry, and packaging constraints*

![FSAE CAD Rendering](assets/img/posts/20251225/fsae_CAD2.jpg)
*Refined CAD model incorporating lessons learned from the prototype build and early testing*

We used finite element analysis selectively — not to chase perfect stress predictions, but to identify load paths, calculate stiffnesses, and filter out bad design concepts.  

![FSAE FEA](assets/img/posts/20251225/FSAE_FEA.png)
*Finite element analysis of press-fit titanium axle into an aluminum upright under cornering load*

Simulation was a guide, not a crutch.

---

What we *couldn’t* do — at least not well — was simulate the full system. About 90% of the teams we competed against had previous cars to validate models against. We had nothing.

So we made a counterintuitive decision.

---

## Two Cars, Not One

Instead of betting everything on a single “perfect” design, we committed to building **two cars**.

- A rough prototype car
- A refined final competition car

We started in **September** and set an aggressive internal deadline:
> The first car would drive under its own power by January 1.

We missed that deadline by three days — but on **January 4**, the prototype rolled under its own power.

<video controls autoplay loop muted playsinline style="max-width:100%;">
  <source src="assets/img/posts/20251225/tirewarmer.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
*Well, more than rolled*

That moment changed everything. Suddenly we had:
- A testbed
- Real data
- Real failures
- Real intuition

Everything we learned went directly into the final car.

---

## Driving the Thing

I only drove the car once.

It was… **violent**.

- 0–100 km/h in about **3 seconds**
- More than **1 g lateral acceleration**
- The driver wasn’t so much *sitting* as *lying down* to keep the center of gravity low
- My shoulders were a few centimeters from the exhaust headers

They were red for days afterward.

It was raw, loud, uncomfortable, and absolutely unforgettable.

---

## Results and Trajectories

That year, we won **Formula SAE Rookie of the Year**.

But the bigger outcome was personal trajectory.

- My friend went on to work for several F1 teams over the years (another story for another day)
- I went to graduate school to learn combustion and how to design real experiments


For me, Formula SAE permanently set the direction:
> taking ideas from my head,  
> turning them into models and mockups,  
> and then building hardware that actually works

---

## Why It Still Matters

That project taught me something I still rely on today:

> You don’t learn engineering by avoiding complexity.  
> You learn it by confronting complexity early, imperfectly, and honestly.

Formula SAE wasn’t just a car.  
It was the moment engineering stopped being theoretical for me — and started being real.
