---
layout: page
title: HimSim.jl
description: A process-based hydrological model for Himalayan watersheds, with special focus on snow and glacier processes.
img: assets/img/12.jpg
importance: 2
category: research
related_publications: false
---

Himalayan watersheds are among the most hydrologically complex systems on Earth — snowmelt, glacier melt, monsoon rainfall, and high-altitude terrain all interact to control streamflow for hundreds of millions of people downstream. Standard hydrological models often lack the process representations needed to capture this complexity.

**HimSim.jl** is a process-based hydrological model developed in the high-performance **Julia** programming language, designed specifically for Himalayan catchments with significant snow and glacier contributions.

## Key Features

- **Snow and glacier processes**: Explicit representation of snowpack accumulation, melt, and glacial contributions to streamflow
- **ODE-based structure**: Model equations set up using [ModelingToolkit.jl](https://mtk.sciml.ai/), part of the Julia SciML ecosystem, enabling symbolic-numeric computation and automatic differentiation
- **High-performance**: Implemented in Julia for computational efficiency, enabling long simulation periods and ensemble runs

## Development

This model was developed during my time as a Post-Baccalaureate Research Assistant at the **Center for Water Resource Studies (CWRS), Institute of Engineering**, under Prof. Dr. Vishnu Prasad Pandey (Apr. 2023 – Aug. 2024).

The use of ModelingToolkit.jl allows the model to be:
- Automatically differentiated (enabling gradient-based calibration)
- Symbolically simplified before compilation
- Interfaced with Julia's rich ODE solver ecosystem (DifferentialEquations.jl)

This project laid the foundation for my current interest in differentiable and hybrid modeling approaches in hydrology.
