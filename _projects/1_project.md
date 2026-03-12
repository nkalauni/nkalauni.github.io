---
layout: page
title: Hybrid Differentiable Land Surface Model
description: A physics-AI hybrid model for improved land-atmosphere flux predictions, implemented end-to-end in JAX.
img: assets/img/lad_arch.png
importance: 1
category: research
related_publications: false
---

Land-atmosphere fluxes — the exchange of energy and water between the land surface and the atmosphere — significantly affect downwind precipitation, weather prediction, climate projections, and drought monitoring. Yet despite decades of development, current Land Surface Models (LSMs) are often outperformed by simple linear regression on standard benchmarks (Best et al., 2015). The core issue: current physical parameterizations are wasting information, and numerical truncation errors (numerical daemons) can corrupt the learning signal for any embedded machine learning component.

This project addresses these limitations by embedding neural networks directly inside a differentiable physics-based land surface model, enabling end-to-end training that respects the underlying physical structure.

---

## The LaD Model

The base model is the **Land Atmosphere Dynamics (LaD)** model (Milly & Shmakin, 2002) — a coupled water and energy balance model that includes:

- A **5-layer soil** with heat diffusion state **T ∈ ℝ⁵**
- A **stomatal resistance** term (r_s) for non-water-stressed conditions
- **Soil sensible heat storage** (S_R) to capture the diurnal cycle
- A **saturated-zone groundwater reservoir** (W_g) for runoff timing

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.html path="assets/img/lad_model_architecture.png" title="LaD Model Architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The LaD model couples energy balance (left) and water balance (right), with snow, root zone, and groundwater reservoirs.
</div>

---

## Hybrid Model Architecture

The key innovation is replacing the physical flux parameterization equations for **latent heat (LE)** and **sensible heat (H)** with a **feedforward neural network (MLP)**, while keeping the rest of the physics intact. The model is implemented in **JAX**, allowing gradients to flow through the ODE solver back into the neural network weights via backpropagation.

<div class="row justify-content-sm-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.html path="assets/img/lad_arch.png" title="Hybrid LaD Model Architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The hybrid model architecture: an MLP embedded inside the LaD ODE system, trained end-to-end via a differentiable ODE solver in JAX against FLUXNET observations.
</div>

Training data comes from **FLUXNET** — a global network of flux tower sites providing 30-minute resolution observations of sensible and latent heat fluxes, along with meteorological forcings (shortwave/longwave radiation, precipitation, temperature, wind speed, humidity).

---

## Results

Experiments were run on two contrasting FLUXNET sites:
- **US-Me2**: Metolius mature ponderosa pine, Oregon
- **US-Wkg**: Walnut Gulch Kendall Grasslands, Arizona

**Finding 1 — Hybrid model outperforms calibrated physics baseline for latent heat flux:**

| Site | Model | RMSE (W/m²) | NSE | KGE |
|------|-------|-------------|-----|-----|
| US-Me2 | Hybrid | 35.72 | 0.655 | 0.738 |
| US-Me2 | Calibrated LaD | 40.56 | 0.555 | 0.758 |
| US-Wkg | Hybrid | 22.98 | 0.731 | 0.822 |
| US-Wkg | Calibrated LaD | 30.58 | 0.523 | 0.633 |

**Finding 2 — Physics states are critical for the MLP:**

When model internal states (soil temperature, soil moisture, snow) are provided as inputs to the MLP, sensible heat flux prediction improves substantially (NSE: 0.841, Bias: -3.45% vs. NSE: 0.856, Bias: +24.81% without states). Without physics states, the MLP develops a large systematic positive bias — it cannot correctly represent the diurnal cycle without knowing the physical state of the system.

---

## Key Takeaways

- Embedding neural networks into a **differentiable physics framework** outperforms calibrated physics models
- **Physics model states are necessary** — the MLP needs internal model states as inputs to learn correct flux parameterizations, not just meteorological forcings
- A **differentiable ODE solver** enables end-to-end training and avoids the numerical daemon problem
- Two design paradigms for hybrid models: (1) learn parameters of physics equations vs. (2) learn flux parameterizations entirely — this work explores the latter

---

**Presented at AGU Annual Meeting 2025, New Orleans, LA** (Oral Talk)

*Supported in part by NOAA award NA23OAR4590382 and startup funding from the University of Arizona.*
