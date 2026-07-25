---
layout: post
title: "Aggregating Spectral Data: Kinematic Analysis from KAOSS"
date: 2025-02-11
categories: data-science
author: Jack Birkin
---

## Background

During graduate school, I led the processing and analysis of a ~300GB KMOS Large Program aimed at understanding the 
two-dimensional kinematics of highly star-forming galaxies at high redshift. The project was part of the KAOSS 
(KMOS-ALMA Observations of Submillimetre Sources) survey, designed to investigate how early galaxies acquire and 
redistribute angular momentum.

Working with this volume of IFU data posed both computational and scientific challenges. Each galaxy’s data cube captured three dimensions—two spatial and one spectral—providing a detailed view of its internal motions. My job was to turn those data cubes into physical insight.

## The Analysis

To extract meaningful kinematic measurements from noisy, high-redshift observations, I built a custom pipeline combining signal enhancement, dimensionality reduction, and physical modeling:

- **Data Cleaning**: The raw data required significant preprocessing. I used Principal Component Analysis (PCA) to identify and suppress residual sky features, and sigma clipping to isolate and remove noise-driven outliers in both spatial and spectral dimensions.

- **Cross-Matching**: I cross-matched each galaxy with ancillary photometry and redshift catalogs to accurately calibrate the astrometry and systemic velocity frame, ensuring consistency across the dataset.

- **Adaptive Binning and Modeling**: To maximize signal-to-noise in faint outer regions, I implemented an adaptive binning routine that preserved spatial resolution in high-S/N regions while aggregating lower-S/N areas. I then applied 3D kinematic modeling to extract rotation curves and dispersion profiles for each galaxy.

<figure class="figure">
  <img src="/images/kaoss_poster.png" alt="KAOSS poster">
  <figcaption>A visual summary of the galaxy sample, showing rotation fields as a function of stellar mass and star formation rate.
  </figcaption>
</figure>

## Summary

This project represents a full-stack approach to IFU kinematic analysis—from data cleaning and calibration to rotation curve modeling and statistical visualization. It reflects my ability to manage large observational datasets, apply advanced analysis techniques, and deliver high-impact results.

The findings contributed to [this peer-reviewed publication in MNRAS](https://ui.adsabs.harvard.edu/abs/2023arXiv230105720B/abstract), which has been cited over a dozen times. Our work provides new insight into how star-forming galaxies at z~2 acquire their rotation and evolve structurally over time.

## Try It Yourself

If you’re working with IFU data and looking for ways to model galaxy kinematics, I’m happy to share code snippets or tools used in this project. Whether you're dealing with adaptive binning, velocity field extraction, or rotation curve fitting, feel free to [reach out](mailto:your.email@example.com) or start a discussion on GitHub.

<p style="font-size: 0.9em; color: #888; font-style: italic;">Published: February 2025</p>
