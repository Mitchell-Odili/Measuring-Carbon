# Measuring-Carbon: Satellite-Based Emissions Analysis Platform
Founding Technical Lead | Presented at [EGU General Assembly (Vienna)](https://meetingorganizer.copernicus.org/EGU23/EGU23-15643.html) & [AGU (San Francisco)](https://agu23.ipostersessions.com/?s=B0-22-99-50-8F-7C-70-BF-22-66-DA-D6-60-F8-01-12)

[Measuring Carbon](https://www.measuringcarbon.com/) is a no-code geospatial intelligence platform that enables policymakers, ESG analysts, journalists, and climate researchers to visualize and quantify carbon emissions directly from satellite observations. No atmospheric science background required.

The platform bridges the gap between NASA OCO-2/3 satellite data and actionable climate strategy, combining high-performance rendering of planetary-scale datasets with localized event analysis of wildfires and industrial emission clusters.

This repository documents the product management lifecycle: strategic decisions, system architecture, and trade-off rationale. The core engine is maintained in a private enterprise repository.

### Executive Summary & Product Deep-Dive

🎙️[!TIP]
[Watch the 2-Minute Project Overview](https://github.com/Mitchell-Odili/Measuring-Carbon/blob/main/Images%20and%20video/Measuring_Carbon_brief.mp4)

*This AI-generated deep-dive provides a high-level walkthrough of the platform's vision, technical architecture, and the strategic pivots overcome during development.
Use of Satellite imagery and AI to support decarbonization efforts; offering a path for standardized, affordable and easy to use monitoring, reporting and verification of carbon emissions*

---

## The Problem

Satellite carbon data exists but is locked behind technical expertise. Accessing and interpreting OCO-2 datasets required atmospheric science knowledge, custom Python environments, and manual processing workflows that took days to produce a single regional analysis.

The people who needed this data most: policy teams, climate journalists, ESG analysts and even climate reseachers had no viable path to it.

## System Architecture
![](/Images%20and%20video/Technical%20architecture%20map.png)

*Platform structure: product vision, data sources, core features, target users, and technical architecture*

The platform is organized into four technical layers designed for scale and reliability:
- **Ingestion:** Automated ingestion engine for multi-terabyte datasets from NASA OCO-2/3 and NASA FIRMS (MODIS Near Real-Time FRP) via scheduled API triggers. that reconciles the 3-hour latency of FIRMS hotspots with the 3–12 month scientific calibration lag of OCO-2/3. This infrastructure enables a 24-hour refresh cycle for operational data while maintaining a historical "Gold Standard" backfill for longitudinal climate audits.
- **Transformation:** Architected a pipeline to convert raw .nc4 scientific files into query-optimized formats like Parquet and CSV. These are fed into a centralized database, reducing query latency for multi-year global datasets to enable real-time visualization.
- **Modeling:** A Fourier series decomposition strips seasonal and annual trends from raw $XCO_2$ readings, isolating the residual signal that represents anomalous regional concentrations. These residuals termed "hotspots" are fed into ML models for source classification, attributing anomalies to anthropogenic activity, industrial clusters, or wildfire events. OCO-2 and OCO-3 trajectories are merged to reduce global data gaps, validated against ground-based TCCON sites at $R^2$ of 0.95 and made available as a selectable dataset in the platform alongside the standard daily and monthly feeds.
- **Delivery:** A REST API powers `CarbonFlow`, a no-code interface built to achieve <5s load times for global time-series decomposition. The platform implements geospatial clustering and simplification logic to enable smooth "Time-Slider" functionality for non-technical users.

## Access Strategy
The platform was designed around a hybrid access model: a deliberate tiered architecture intended to serve users from no-code visualization through to foundational model access.
- **Web Interface (Shipped):** The MVP access method. A no-code dashboard for visual exploration of carbon emissions requiring no programming skills, designed for policymakers, ESG analysts, and journalists.
- **Raw Data APIs (Roadmap):** Programmatic access to pull platform datasets into external systems. Commercially, these APIs were scoped to enable carbon emission identification in third-party applications for example, consumer marketplaces calculating carbon offsets for shipping logistics.
- **Inference APIs (Roadmap):** Designed for developers and data scientists wanting to run the platform's pre-trained models against custom input datasets without rebuilding the underlying models. Core to the platform's open science positioning.
- **Model Weights & Architecture (Roadmap):** Direct access to underlying model architecture and weights for researchers wanting to fine-tune or pre-train on Measuring Carbon's foundational work.

## Key Decisions
### 1. Hardware to Satellite Pivot
- **Context:** The original thesis depended on ground-based soil sensing hardware for carbon verification at scale.
- **Trade-off:** Physical deployment faced prohibitive unit economics, high maintenance costs, and logistical scaling bottlenecks in emerging markets.
- **Decision:** Proposed and led the strategic pivot to a "Top-Down" satellite-based SaaS model using public satellite products.
- **Outcome:** Removed physical infrastructure dependency, expanded the addressable market from regional pilots to global coverage, and unlocked a scalable SaaS delivery model.

### 2. High Performance Planetary rendering: (The Magic Window)

- **Problem:** Rendering 7+ years of global $XCO_2$ telemetry (millions of points) caused terminal browser lag and memory exhaustion. 
- **Solution:** Implemented BigQuery Geospatial Clustering and pre-decomposition of residuals at the cell level before delivery to the front end.
- **Outcome:** Achieved sub-3-second load times for global time-series decomposition, enabling smooth time-slider functionality for non-technical users tracking seasonal CO2 shifts and regional anomalies.

### 3. Bridging Research and Engineering Cultures
- **Problem:** Research scientists operating in uncertainty and software engineers needing deterministic requirements were deadlocking on the roadmap.
- **Decision:** Introduced a translation layer in sprint planning, converting open-ended research hypotheses into time-boxed engineering tasks with defined acceptance criteria, while preserving scientific flexibility in the research backlog.
- **Outcome:** Established a continuous delivery cycle that maintained scientific integrity without blocking engineering sprints.

## Key Features
![](/Images%20and%20video/Brazil%20Monthly%20OCO2%20L3.gif)

*CarbonFlow: Monthly XCO2 visualization for Brazil, 2015–2021. NASA GEOS OCO2 L3 dataset.*
- **No-Code Spatial Analysis:** Draw a "Bounding Box" around any region (e.g., an Amazon fire) to instantly calculate total $XCO_2$ release.
- **Time-Series Decomposition:** Toggle between "Actual," "Seasonal Trend," and "Residual" views to identify underlying emission anomalies.
- **Cross-Source Contrast:** Compare the carbon footprint of individual aviation routes against regional industrial sectors in real-time.
- **City-Scale Granularity:** Integrating OCO-3 "Snapshot Area Maps" (SAMs) for point-source industrial monitoring.

## Global Impact & Regional Expansion

- **Global Delivery:** Successfully merged disparate satellite trajectories (OCO-2 and OCO-3), significantly reducing data gaps for global monitoring.
- **Omdena Kenya Partnership:** Served as Product Owner, orchestrating a global cross-functional cohort of 70 ML Engineers over 10 weeks to localize models for Kenya.
- **Sector Mapping:** Led the sectoral mapping of emissions across Agriculture, Energy, and Transportation, producing city-level audits and data-backed policy recommendations for wind energy investment.

Note: The core engine is maintained in a private repository. This documentation covers the product lifecycle, architecture, and technical leadership methodologies used to scale the platform.


