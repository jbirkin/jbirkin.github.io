---
layout: post
title: "Aggregating Spectral Data: Stacking [CII] from CRISTAL"
date: 2025-02-11
categories: data-science
author: Jack Birkin
---

## Background

As part of my postdoctoral research, I am a member of the [CRISTAL](https://sites.google.com/view/alma-cristal) 
collaboration—a team assembled to analyze the data from an ALMA Large Program aiming to understand the 
interstellar medium (ISM) of star-forming galaxies that exist just a billion
years after the Big Bang, at a time when the Universe was undergoing a peak in cosmic star formation. This may make 
the Universe sound very old, but it's around 14 billion years old now, so at the point in time we're looking at it 
was still figuring stuff out.

The program uses high-resolution [CII] 158 μm observations to map gas conditions, star formation, and kinematics in
galaxies that are representative of what we call "the main sequence" at this early epoch - this means that they are
considered to be "typical" of the galaxies at that time and therefore statistically representative. However,
many of the features we seek—particularly broad wings in the [CII] line that may indicate feedback or outflows — are
too faint to detect in individual galaxies.

## My Role

To overcome this, I perform a stacking analysis: combining the spectra of multiple galaxies to amplify faint signals and
recover population-level trends. My role in the collaboration is to handle the data processing and statistical modeling
required to align, normalize, and combine the 3D [CII] cubes, as well as to interpret the composite spectra.

I developed a flexible pipeline to handle:

- Alignment and normalization of data cubes
- Dimensionality reduction and feature extraction
- Spectral stacking with bootstrap and jackknife error estimation
- Bayesian model fitting with MCMC
- Model comparison and selection to assess broad component significance

## The Analysis

This project draws heavily on my experience with **data normalization**, **model fitting**, and **spectral feature
engineering**, combining technical skills with physical intuition about the ISM:

- **Normalization and Dimensionality Reduction**: To ensure each galaxy contributes comparably to the stack, I explored
  multiple normalization strategies based on [CII] luminosity, continuum flux, and star-formation rate. Principal
  Component Analysis (PCA) helped identify common spectral features and isolate subtle variations in the line profiles.

- **Stacking and Resampling**: I applied both median and inverse-variance-weighted stacking, combined with bootstrap and
  jackknife resampling to estimate uncertainties. This allowed us to robustly assess whether faint emission
  components—such as broad wings or asymmetric profiles—were statistically significant or driven by a few outliers.

- **MCMC Fitting and Model Selection**: I used Markov Chain Monte Carlo to fit composite spectral models, testing for
  the presence of broad Gaussian components indicative of outflows. Model comparison via AIC/BIC provided a statistical
  foundation for interpreting these features as real rather than noise.

<figure class="figure">
  <img src="/images/cristal_stack_example.png" alt="[CII] stack from CRISTAL" style="width:80%; height:auto;">
  <figcaption>Composite [CII] profile from 21 galaxies at z~5. A broad component is tentatively detected in the wings of the line
profile.
  </figcaption>
</figure>

## Summary

This project demonstrates how stacking analysis—when applied carefully—can extract meaningful physical insights from the
faintest signals in high-redshift galaxies. Our results provide tentative evidence for broad [CII] emission, potentially
linked to early feedback processes, and offer new constraints on how gas is transported and redistributed in the early
Universe.

By combining statistical rigor with astrophysical modeling, this work shows the power of data aggregation to amplify
subtle signals that would otherwise go undetected.

## Try It Yourself

If you're working with stacked spectra or faint emission lines, and you're interested in techniques for alignment,
normalization, and model testing, feel free to check out the underlying codebase. I've packaged up the
stacking tools for public release—now available [here](https://github.com/jbirkin/alma-stacking-pipeline)!