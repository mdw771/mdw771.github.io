---
layout: page
title: Research
permalink: /research/
---

My research brings together computational imaging, scientific machine learning, and high-performance computing. I develop methods that connect physical models with measured data, and build software that makes these methods useful in experiments. My recent work focuses on ptychographic reconstruction, learned image priors, and AI agents for experimental automation and algorithm discovery.

## Neural priors for ptychographic reconstruction

Ptychography recovers an object's structure from overlapping diffraction measurements. In practice, limited sampling, changing illumination, and ambiguities between sample layers can introduce artifacts that are difficult to remove through optimization alone. I investigate how neural-network priors can improve reconstruction while retaining agreement with the imaging model and measured data.

<!-- Original full-size figure downloaded from Optica: https://opg.optica.org/viewmedia.cfm?uri=optica-13-1-45&figure=optica-13-1-45-g004&imagetype=full (2425 x 1512 JPEG). -->
[![Comparison of experimental ptychographic phase reconstructions with conventional algorithms, spectral filtering, and a diffusion-based plug-and-play prior]({{ '/images/research_img_pnp.jpg' | relative_url }})]({{ '/images/research_img_pnp.jpg' | relative_url }})

*Experimental reconstruction comparisons from Du et al., [Optica 13, 45–56 (2026)](https://doi.org/10.1364/OPTICA.579456), Fig. 4. © 2026 Optica Publishing Group.*

In our diffusion-based plug-and-play method, a pretrained text-to-image model removes specified artifacts within a physics-based reconstruction loop. An alternating direction method of multipliers (ADMM) formulation couples image editing to data fidelity. We demonstrated suppression of grid artifacts and multislice crosstalk on simulated and experimental data.

I also develop reconstruction with deep image priors, where a neural network represents the object and is optimized directly against diffraction measurements. In a case study in the Pty-Chi paper, this approach improved contrast and suppressed artifacts when only 2% of the original diffraction patterns were retained. Earlier work used a modified double deep image prior to reduce crosstalk between reconstructed layers in multislice ptychography.

In related work on probe-position estimation, we combined single-shot neural-network phase retrieval with image registration and a global least-squares solution for scan coordinates. This links learned image predictions to a geometric calibration problem that directly affects reconstruction quality.

[Diffusion priors · Optica (2026)](https://doi.org/10.1364/OPTICA.579456) · [Deep image priors in Pty-Chi · preprint (2025)](https://arxiv.org/abs/2510.20929) · [Multislice crosstalk · J. Synchrotron Rad. (2021)](https://doi.org/10.1107/S1600577521003507) · [Probe-position estimation · Optics Express (2024)](https://doi.org/10.1364/OE.524317)

## Vision-language agents for experimental automation

Scientific instruments require both procedural knowledge and interpretation of what an experiment is showing. I developed Experiment Automation Agents (EAA), an agent harness that connects vision-language models to instrument-control and analysis tools. It supports interactive measurements as well as workflows with different levels of agent involvement.

<!-- Original article image: https://cdn.ncbi.nlm.nih.gov/pmc/blobs/a034/13633048/f589391e0a2e/nihms-2210081-f0001.webp (1599 x 630 WebP). -->
[![EAA architecture connecting instrument tools, a task manager, a vision-language agent, and optional long-term memory]({{ '/images/research_img_eaa.webp' | relative_url }})]({{ '/images/research_img_eaa.webp' | relative_url }})

*Architecture of the original EAA implementation, from Du et al., [npj Computational Materials (2026)](https://doi.org/10.1038/s41524-026-02213-8), Fig. 1 ([image via PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC13633048/#F1); CC BY 4.0). The current EAA-Pi implementation is linked on the [Software page]({{ '/software/' | relative_url }}#eaa-and-eaa-pi).*

At the Advanced Photon Source, we demonstrated automated X-ray optics focusing, searches for features described in natural language, and interactive data acquisition. This work combines multimodal feedback, tool use, and workflow design to make beamline operations more accessible and reduce repetitive manual work.

[Publication · npj Computational Materials (2026)](https://doi.org/10.1038/s41524-026-02213-8) · [Software]({{ '/software/' | relative_url }}#eaa-and-eaa-pi)

## Autonomous discovery of scientific image-processing algorithms

Many scientific images need specialized processing, but developing a reliable algorithm often requires repeated implementation, testing, and inspection. I developed CVEvolve, an automated research harness that searches for algorithms from a task description and example data.

<!-- Original standalone vector PDF: figures/workflow.pdf in https://arxiv.org/src/2605.11359v3 . PNG preview rendered at 300 dpi with antialiasing; full-size link opens the original PDF. -->
[![CVEvolve workflow showing data preparation, baseline evaluation, algorithm generation and refinement, candidate history, and holdout testing]({{ '/images/research_img_cvevolve.png' | relative_url }})]({{ '/images/research_img_cvevolve.pdf' | relative_url }})

*Algorithm-search workflow from Du et al., [arXiv:2605.11359v3](https://arxiv.org/abs/2605.11359v3), Fig. 1 (CC BY 4.0). Click the image to open the original vector PDF.*

CVEvolve combines code execution, evaluation, persistent search history, and visual inspection. Holdout testing helps assess whether improvements carry over to unseen data. We demonstrated the approach on X-ray fluorescence image registration, Bragg peak detection, and diffraction-image segmentation.

[Preprint (2026)](https://arxiv.org/abs/2605.11359) · [Code](https://github.com/AdvancedPhotonSource/CVEvolve)

## Scientific machine learning and distributed computing

I develop models and distributed workflows for learning from large scientific datasets. In the AlphaDiffract collaboration, I led model architecture development and implementation for predicting crystal symmetry and lattice parameters directly from powder X-ray diffraction patterns.

<!-- Original vector figure: https://arxiv.org/html/2603.23367v1/Fig1.svg . -->
[![AlphaDiffract architecture with a one-dimensional ConvNeXt backbone and prediction heads for crystal system, space group, and lattice parameters]({{ '/images/research_img_alphadiffract.svg' | relative_url }})]({{ '/images/research_img_alphadiffract.svg' | relative_url }})

*Model architecture from Andrejevic, Du et al., [arXiv:2603.23367v1](https://arxiv.org/abs/2603.23367v1), Fig. 1 (CC BY-NC-SA 4.0).*

My distributed-training work uses MPI and PyTorch DistributedDataParallel, including runs on more than 100 GPUs on Argonne's Polaris supercomputer. The AlphaDiffract models learn from tens of millions of diffraction patterns using a one-dimensional ConvNeXt backbone and dedicated prediction heads.

In ongoing work for Genesis Mission SYNAPS-I, I led the design and implementation of an HPC data-generation pipeline for ptychographic phase retrieval. The pipeline reuses Pty-Chi's forward model and uses `torch.distributed` to generate more than 50 million simulated diffraction patterns. I also contributed to model design and fine-tuning for PtychoFM; the manuscript is in preparation.

[AlphaDiffract preprint (2026)](https://arxiv.org/abs/2603.23367) · [Code](https://github.com/AdvancedPhotonSource/OpenAlphaDiffract)

## Bayesian adaptive sampling for X-ray spectroscopy

Dense energy scans can limit the time resolution of X-ray absorption near-edge structure (XANES) spectroscopy. I developed an adaptive workflow that uses a Gaussian-process model and an acquisition function informed by spectral structure to select the next energy to measure.

<!-- Original publisher image: https://media.springernature.com/full/springer-static/image/art%3A10.1038%2Fs41524-025-01771-7/MediaObjects/41524_2025_1771_Fig1_HTML.png (1750 x 781 PNG). -->
[![Adaptive XANES workflow linking a battery experiment to Gaussian-process updates and selection of the next measurement energy]({{ '/images/research_img_xanes.png' | relative_url }})]({{ '/images/research_img_xanes.png' | relative_url }})

*Adaptive measurement workflow from Du et al., [npj Computational Materials 11, 320 (2025)](https://doi.org/10.1038/s41524-025-01771-7), Fig. 1 (CC BY 4.0).*

In the reported test cases, the method used approximately 15–20% of the measurement points in a dense scan, with normalized root-mean-square errors generally below 0.005. We also demonstrated the workflow in a live battery experiment at the Advanced Photon Source, connecting adaptive sampling to instrument control and evolving sample chemistry.

[Publication · npj Computational Materials (2025)](https://doi.org/10.1038/s41524-025-01771-7)

## Differentiable physics and 3D imaging beyond the depth of focus

At high resolution, a thick specimen can no longer be treated as a simple projection: the wave changes as it propagates through the material. I developed differentiable multislice imaging methods that model this propagation and reconstruct the three-dimensional object through numerical optimization.

[![Three-dimensional X-ray imaging beyond the depth-of-focus limit]({{ '/images/research_img_bdof.png' | relative_url }})]({{ '/images/research_img_bdof.png' | relative_url }})

This work enabled a common reconstruction approach for ptychographic and full-field holographic imaging beyond the depth-of-focus limit. I subsequently developed Adorym, a modular framework that uses automatic differentiation to reconstruct images and jointly refine experimental parameters. It supports multiple imaging geometries and distributed computation. I also contributed to studies of distributed-memory algorithms for wave propagation through inhomogeneous media.

[3D imaging · Science Advances (2020)](https://doi.org/10.1126/sciadv.aay3700) · [Adorym · Optics Express (2021)](https://doi.org/10.1364/OE.418296) · [Distributed wave propagation · Optics Express (2020)](https://doi.org/10.1364/OE.400240)

## Large-field-of-view and teravoxel tomography

High-resolution tomography is often limited by the size of the detector or illuminating beam. I co-developed Tomosaic, a Python pipeline that combines separately acquired projection tiles to reconstruct specimens larger than a single field of view.

[![Mosaic tomography for high-resolution imaging of specimens larger than the detector field of view]({{ '/images/research_img_tomosaic.png' | relative_url }})]({{ '/images/research_img_tomosaic.png' | relative_url }})

Using parallel processing on an HPC cluster, we demonstrated a 9.36-trillion-voxel reconstruction from 5.8 TB of measurements. Related work compared acquisition strategies for extended objects and connected automated data collection, transfer, and processing. I also acquired tomography data and reconstructed large mouse-brain volumes used in collaborative neuroimaging studies.

[Tomosaic · J. Synchrotron Rad. (2018)](https://doi.org/10.1107/S1600577518010093) · [Acquisition strategies · JOSA A (2018)](https://doi.org/10.1364/JOSAA.35.001871) · [Automated workflow · conference report (2018)](https://doi.org/10.1364/BRAIN.2018.BF4C.2)

## Imaging contrast and radiation-dose modeling

[![Calculated contrast and dose requirements for X-ray and electron microscopy of hydrated specimens]({{ '/images/research_img_ultramic.png' | relative_url }})]({{ '/images/research_img_ultramic.png' | relative_url }})

How do specimen thickness and desired resolution determine the most suitable imaging method? In my doctoral research, I developed a unified model of image contrast and radiation dose for X-ray and electron microscopy of hydrated organic materials. The model accounts for plural and inelastic scattering and the use of energy filters, helping quantify the trade-offs between resolution, specimen thickness, and radiation damage. It explains the advantages of electron microscopy for thin specimens and X-ray microscopy for thicker volumes.

[Publication · Ultramicroscopy (2018)](https://doi.org/10.1016/j.ultramic.2017.10.003)
