---
layout: post
title: "3-D Modelling of JWST Data"
date: 2025-02-11
categories: data-science
author: Jack Birkin
---

## Background

I am a member of the international collaboration [TEMPLATES](https://sites.google.com/view/jwst-templates/), an ongoing
program designed to spatially resolve the star formation in four galaxies at the peak of cosmic star formation. These
galaxies are gravitationally lensed, meaning they appear much brighter than they do intrinsically, allowing for
observations at much higher spatial resolutions. This enables an incredibly detailed study of these galaxies.

As an expert in near-infrared IFU observations, my role is to extract the relevant data features to give a complete
picture of the underlying systems while effectively visualizing the data. To achieve this, I developed a
powerful [open-source Python module](https://github.com/jbirkin/cubespecfit) that simultaneously models the emission
from the galaxy and produces a complete visualization of the entire cube.

## The paper

I utilized this tool to
produce [this peer-reviewed publication](https://ui.adsabs.harvard.edu/abs/2023ApJ...958...64B/abstract) in the
Astrophysical Journal, which has been cited over 20 times. In the paper, we model the emission from Hydrogen and
Nitrogen in the galaxies to measure the proportion of metals they contain. As galaxies evolve, their stars are born,
grow and die, in the process producing "metals" (astronomers used this to mean any metal heavier than Helium). In
addition, gas is both driven into and out of galaxies, which can remove and add metals, depending on the how
metal-rich that gas is. All this is to say - knowing about the amount of metals in a galaxy provides a window into
understanding how the galaxy has evolved.

## The analysis

So what about the technical details? This project draws on my expertise in **3D data modeling, spectral fitting, and
scientific visualization**, along with my ability to process and analyze large-scale astronomical datasets
effectively:

- Large-scale multi-dimensional datasets: JWST IFU observations generate enormous data cubes, with two spatial
  dimensions and one spectral dimension. Handling these datasets requires efficient data pipelines, memory-aware
  analysis routines, and the ability to extract meaningful science from high-dimensional spaces. My workflow is built to
  scale—processing entire mosaics of spaxels in parallel and managing noise, variance propagation, and masking with
  precision.
- Spectral fitting: Accurately extracting physical properties like metallicity or star-formation rates from galaxy
  spectra requires robust fitting of complex emission line profiles. For this project, I used a custom-built fitting
  routine that models blended and asymmetric lines, accounts for instrumental line spread functions, and outputs
  confidence intervals on derived quantities. This enables pixel-by-pixel measurements of key diagnostics like [NII]
  /H$\alpha$ across the galaxy.
- 3-D data modeling and Data visualization: While 3-dimensional data cubes are one of the most powerful tools in
  astronomy, they can be
  difficult to understand and visualize intuitively. 3-D data cannot be looked at and understood immediately like a
  2-D image can. There are a couple of ways around this, such as "collapsing" the cube by summing it over the
  wavelength dimension, or producing an animation that steps through each 2-D "frame" of the cube. For this project
  I develop a Python module that models the cube and visualizes it as a 2-D "grid" where each "pixel" shows the
  emission line spectrum at that position in the image.

<figure class="figure">
  <img src="/images/birkin23_plots.png" alt="JWST Data Analysis">
  <figcaption>Left: Comparing measurements of system properties with existing scaling relations. Right: Identifying 
trends between galaxy dust mass and galaxy dust enrichment.
  </figcaption>
</figure>


## Summary

This project highlights how custom, scalable spectral fitting tools can unlock the full scientific value of JWST IFU
datasets. By combining 3D modeling, pixel-level spectral analysis, and intuitive visualizations, we were able to map the
spatial distribution of metallicity in high-redshift galaxies with unprecedented resolution. These measurements provide
new constraints on how metals are produced, retained, and redistributed during galaxy evolution at the peak epoch of
star formation.

Beyond the scientific results, this work demonstrates the power of open-source software and tailored data workflows for
making complex datasets more accessible and informative. It reflects my broader goal: to develop tools that not only
extract insights from data but also make those insights easier to explore, communicate, and build upon.

## Try It Yourself

If you're working with IFU datasets—JWST or otherwise—and want to explore or extend this kind of analysis, the full cube
modeling and visualization toolkit is open-source and available here:  
👉 [GitHub: jbirkin/cubespecfit](https://github.com/jbirkin/cubespecfit) The code includes documentation, examples,
and flexible interfaces for fitting and visualizing any 3D cube with emission
line features.

Feel free to reach out, open an issue on GitHub, or fork the repository to get started!
