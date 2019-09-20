---
layout: page
title: Research
permalink: /research/
---

## Relative merits and limiting factors for x-ray and electron microscopy of thick, hydrated organic materials

![Dose estimation](https://github.com/mdw771/mdw771.github.io/raw/master/images/research_img_ultramic.png "Dose estimation")

Electron and x-ray microscopes allow one to image the entire, unlabeled structure of hydrated materials at a resolution well beyond what visible light microscopes can achieve. However, both approaches involve ionizing radiation, so that radiation damage must be considered as one of the limits to imaging. Drawing upon earlier work, we developed a unified theoretical approach to estimating the image contrast (and thus the required exposure and corresponding radiation dose) in both x-ray and electron microscopy. This approach accounts for factors such as plural and inelastic scattering, and (in electron microscopy) the use of energy filters to obtain so-called “zero loss” images. As expected, it shows that electron microscopy offers lower dose for specimens thinner than about 1 micron (such as for studies of macromolecules, viruses, bacteria and archaebacteria, and thin sectioned material), while x-ray microscopy offers superior characteristics for imaging thicker specimen such as whole eukaryotic cells, thick-sectioned tissues, and organs. The required radiation dose scales strongly as a function of the desired spatial resolution, allowing one to understand the limits of live and frozen hydrated specimen imaging. Also, we consider the factors limiting x-ray microscopy of thicker materials, suggesting that specimens as thick as a whole mouse brain can be imaged with x-ray microscopes without significant image degradation should appropriate image reconstruction methods be identified.

## Tomosaic: efficient acquisition and reconstruction of teravoxel tomography data using limited-size synchrotron x-ray beams

![Tomosaic](https://github.com/mdw771/mdw771.github.io/raw/master/images/research_img_tomosaic.png "Tomosaic")

Computed Tomography (CT) allows one to obtain 3D representations of a specimen based on the collection of 2D projection images. However, the size of the specimens often extends beyond the field of view of the imaging system due to the limited size of CCD detectors and beam width in synchrotron x-ray sources, which usually leads to a trade-off between spatial resolution and sample coverage. To address this issue, we are developing a Python-based pipeline named Tomosaic that allows users to obtain a high-resolution 3D map of the entirety of a large sample by stitching separately collected projection data. Most processes in the pipeline are highly parallelized so that they can take the advantage of a high-performance computer (HPC) to handle terabyte-sized datasets.

## 3D imaging beyond the depth of focus limit

![Beyond DOF](https://github.com/mdw771/mdw771.github.io/raw/master/images/research_img_bdof.png "Beyond DOF")

Conventional x-ray tomography reconstruction theories are based on the pure-projection approximation, which assumes the intensity readings on the detector are line integrals along the ray paths through the sample. This assumption is broken when the ultra-high-resolution imaging of a thick specimen is required. In such cases, the diffraction of x-ray within the sample is no longer negligible, and reconstruction images are heavily plagued by diffraction fringes. The difficulty of deconvoluting the diffraction effects lies in that the back-propagation problem is ill-defined in an unknown heterogeneous medium (i.e., the sample). We are working with mathematicians to address this challenge using an indirect approach, which, utilizing Fourier optics and numerical optimization theories, would potentially provide a solution to the problem.





