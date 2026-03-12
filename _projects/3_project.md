---
layout: page
title: GNN for Meteorological Data
description: A graph neural network approach for interpolating station-based point meteorological data.
img: assets/img/7.jpg
importance: 3
category: research
related_publications: false
---

Meteorological station data is inherently sparse and irregularly distributed — yet most ML workflows for Earth system science require spatially continuous fields. Traditional interpolation methods (e.g., inverse distance weighting, kriging) do not learn from data and can struggle with complex terrain or sparse networks.

This project develops a **graph neural network (GNN)** based method for interpolating point meteorological observations to continuous spatial fields, with awareness of the irregular graph structure of station networks.

## Approach

Station networks naturally form a graph — stations are nodes, and their spatial relationships define edges. GNNs are well-suited for this structure because they:

- Operate directly on irregular spatial graphs without requiring a fixed grid
- Learn to aggregate information from neighboring stations in a data-driven way
- Can incorporate station metadata (elevation, land cover) as node features

## Status

Active development at the **Computational Hydrology Lab, University of Arizona** (Jun. 2025 – Present).
