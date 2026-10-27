---
layout: page
title: Software
permalink: /software/
---

I develop scientific software for image reconstruction, experimental automation, and machine learning. These projects turn research methods into reusable tools, with an emphasis on physical modeling, extensible algorithms, and scalable computation.

## Pty-Chi

<!-- Original vector figure: https://arxiv.org/html/2510.20929v1/layers.svg . -->
[![Pty-Chi software layers connecting reconstruction algorithms, automatic differentiation, optimizers, and forward models through PyTorch]({{ '/images/research_img_ptychi.svg' | relative_url }})]({{ '/images/research_img_ptychi.svg' | relative_url }})

*Main building blocks from Du et al., [arXiv:2510.20929v1](https://arxiv.org/abs/2510.20929v1), Fig. 1 (CC BY-NC-ND 4.0).*

I lead the development of Pty-Chi, an open-source PyTorch library for GPU-accelerated ptychographic reconstruction. It brings analytical algorithms and automatic differentiation into a common framework, supporting methods such as LSQML, PIE-family algorithms, difference map, and bilinear Hessian optimization.

Pty-Chi includes multislice modeling, experimental-parameter refinement, and multi-GPU reconstruction. Its modular design also supports new forward models and neural-network object representations, including deep image priors. It is used at synchrotron facilities and universities for reconstruction and algorithm development.

[GitHub](https://github.com/AdvancedPhotonSource/pty-chi) · [Documentation](https://pty-chi.readthedocs.io/) · [Preprint (2025)](https://arxiv.org/abs/2510.20929)

## EAA and EAA-Pi

<!-- Original article image: https://cdn.ncbi.nlm.nih.gov/pmc/blobs/a034/13633048/f589391e0a2e/nihms-2210081-f0001.webp (1599 x 630 WebP). -->
[![Architecture of the original EAA implementation for instrument control and experimental workflows]({{ '/images/research_img_eaa.webp' | relative_url }})]({{ '/images/research_img_eaa.webp' | relative_url }})

*Original EAA architecture from Du et al., [npj Computational Materials (2026)](https://doi.org/10.1038/s41524-026-02213-8), Fig. 1 ([image via PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC13633048/#F1); CC BY 4.0).*

I developed EAA (Experiment Automation Agents), an agent harness for natural-language interaction with scientific instruments. It connects vision-language models to instrument-control and analysis tools, supporting both interactive operation and automated workflows. Demonstrations at the Advanced Photon Source include optics focusing, feature search, and data acquisition.

The earlier EAA implementation, now archived, uses LangGraph and includes a task-manager architecture, image-aware tool interactions, and Model Context Protocol (MCP) integration. The current version, EAA-Pi, is based on the [Pi](https://pi.dev/) harness. The figure above illustrates the earlier implementation described in the paper.

[EAA-Pi · current version](https://github.com/AdvancedPhotonSource/eaa-pi) · [EAA · earlier LangGraph version](https://github.com/AdvancedPhotonSource/eaa) · [Publication (2026)](https://doi.org/10.1038/s41524-026-02213-8)

## CVEvolve

<!-- Shared with Research: original standalone vector PDF from figures/workflow.pdf in https://arxiv.org/src/2605.11359v3 . -->
[![CVEvolve workflow showing data preparation, baseline evaluation, algorithm generation and refinement, candidate history, and holdout testing]({{ '/images/research_img_cvevolve.png' | relative_url }})]({{ '/images/research_img_cvevolve.pdf' | relative_url }})

*Algorithm-search workflow from Du et al., [arXiv:2605.11359v3](https://arxiv.org/abs/2605.11359v3), Fig. 1 (CC BY 4.0). Click the image to open the original vector PDF.*

I developed CVEvolve to automate the search for scientific data-processing algorithms. Starting from a task description and example data, the harness prepares an environment, establishes baselines, and iteratively generates and evaluates candidate algorithms. It maintains a searchable history of candidates and uses holdout tests to assess generalization.

Applications include image registration, diffraction peak detection, and diffraction-image segmentation. The [Research page]({{ '/research/' | relative_url }}#autonomous-discovery-of-scientific-image-processing-algorithms) describes the scientific motivation.

[GitHub](https://github.com/AdvancedPhotonSource/CVEvolve) · [Preprint (2026)](https://arxiv.org/abs/2605.11359)

## AlphaDiffract

AlphaDiffract predicts crystal system, space group, and lattice parameters from powder X-ray diffraction patterns. In this collaborative project, I led model architecture development and implementation and contributed to data processing, training, and evaluation.

The model uses a one-dimensional ConvNeXt backbone with separate prediction heads for crystallographic properties. My associated distributed-training work uses MPI and PyTorch DistributedDataParallel to train on large diffraction datasets on HPC systems.

[GitHub](https://github.com/AdvancedPhotonSource/OpenAlphaDiffract) · [Preprint (2026)](https://arxiv.org/abs/2603.23367)

## Adorym

**Archived project.**

[![Differentiable three-dimensional X-ray reconstruction beyond the depth of focus]({{ '/images/research_img_bdof.png' | relative_url }})]({{ '/images/research_img_bdof.png' | relative_url }})

I developed Adorym, short for *Automatic Differentiation-based Object Retrieval with Dynamic Modeling*, as a general framework for X-ray image reconstruction. Its modular forward models support multiple imaging configurations, while automatic differentiation computes the gradients needed to optimize the object and experimental parameters together.

Adorym uses PyTorch or Autograd and supports GPU acceleration and MPI-based distributed computation. It was designed for large-scale two- and three-dimensional reconstruction, including multislice imaging beyond the depth-of-focus limit.

[GitHub](https://github.com/mdw771/adorym) · [Documentation](https://adorym.readthedocs.io/en/latest/) · [Publication (2021)](https://doi.org/10.1364/OE.418296)

## Tomosaic

[![Tomosaic acquisition and reconstruction for extended specimens]({{ '/images/research_img_tomosaic.png' | relative_url }})]({{ '/images/research_img_tomosaic.png' | relative_url }})

I co-developed Tomosaic, a Python package for mosaic tomography of specimens larger than the detector or beam's field of view. It provides tools for registering and blending projection tiles, organizing large datasets, and reconstructing the combined volume.

The pipeline uses parallel computation to handle terabyte-scale measurements. The published demonstration reconstructed a 9.36-trillion-voxel volume from 5.8 TB of data, extending high-resolution tomography to much larger specimens.

[GitHub](https://github.com/mdw771/tomosaic2) · [Publication (2018)](https://doi.org/10.1107/S1600577518010093)
