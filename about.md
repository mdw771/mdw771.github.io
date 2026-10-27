---
layout: page
title: About
permalink: /about/
---

## Who am I

I'm currently working at Argonne National Laboratory as a computational scientist. Before joining KLA,
I worked as an algorithm engineer at KLA (KLA-Tencor) and a postdoc at Argonne National Lab. 

[**Download CV**]({{ '/resources/cv_mingdu.pdf' | relative_url }})

## My research

I am interested in the following areas of research:
- Advanced optimization algorithms and learned priors for phase retrieval in coherent diffraction imaging,
  particularly ptychography.
  - Deep image prior [[Publication]](https://arxiv.org/abs/2510.20929)
  - Text2Img diffusion model as a generative plug-and-play prior for image enhancement [[Publication]](https://opg.optica.org/optica/fulltext.cfm?uri=optica-13-1-45)
- Distributed training and parallel computing for scientific problems on high-performance computers
  - Large-scale training of a crystallography structural prediction model [[Publication]](https://arxiv.org/abs/2603.23367)
  - Distributed ptychographic reconstruction [[Publication 1]](https://arxiv.org/abs/2510.20929) [[Publication 2]](https://opg.optica.org/oe/fulltext.cfm?uri=oe-29-7-10000)
- Automation of imaging and materials characterization experiments
  - Bayesian adaptive sampling for X-ray absorption spectroscopy [[Publication]](https://www.nature.com/articles/s41524-025-01771-7)
  - Multimodal LLM agentic workflows and system design for X-ray imaging beamlines [[Publication]](https://www.nature.com/articles/s41524-026-02213-8)
- Physical modeling of complex image formation processes and their reconstructions
  - Multi-slice modeling for 3D imaging of thick objects beyond the depth-of-focus [[Publication]](https://www.science.org/doi/10.1126/sciadv.aay3700)
- Automatic differentiation (AD) for solving inverse problems in imaging science
  - An AD package that reconstructs multiple imaging techniques and solve various parameters in the image formation model [[Publication]](https://opg.optica.org/oe/fulltext.cfm?uri=oe-29-7-10000)
- Image processing, computer vision, and parallel computing for large-scale tomographic reconstruction [[Publication]](https://journals.iucr.org/s/issues/2018/05/00/il5010/)

## My software packages

- Pty-Chi: PyTorch-based ptychographic reconstruction package supporting both analytical and automatic-differentiation engines [[GitHub]](https://github.com/AdvancedPhotonSource/pty-chi)
- EAA: agent harness for experiment automation
  - Earlier version using LangGraph: [[GitHub]](https://github.com/AdvancedPhotonSource/eaa) 
  - Current version based on the [Pi](https://pi.dev/) harness (EAA-Pi): [[GitHub]](https://github.com/AdvancedPhotonSource/eaa-pi)
- Adorym (archived): automatic differentiation package for various imaging setups and reconstruction parameters [[GitHub]](https://github.com/mdw771/adorym)
- Tomosaic: extending the field-of-view of tomography through image stitching [[GitHub]](https://github.com/mdw771/tomosaic2)

## My hobbies

I am a amateur digital artist that create illustrations and fanarts of my favorite ACG characters! 
My most handy creation tool is iPad Pro + ProCreate. I also used to work on music videos (After Effects),
composing and arrangement (LogicPro) in the Vocaloid/SynthV community. Other than that, I also know
a bit of 3D modeling and animation with [Blender](https://www.blender.org/).
For a (incomplete) list of my past works, please refer to the [Hobby Portfolio](https://mdw771.github.io/portfolio/) page. 

## What I'd write in this site

- Numerical computation and digital image processing in Python;
- Statistical analysis and visualization of research data;
- Use of Tensorflow/Autograd and their applications in solving optimization problems;
- Basic parallelization design based on MPI;
- Building and configuration of scientific-computation-oriented development environment;
- Some random Linux tips. 

## Contact me

[ming.du101@gmail.com](mailto:ming.du101@gmail.com)

[Google Scholar Profile](https://scholar.google.com/citations?user=fw3MNIsAAAAJ)

---

## 关于我

目前，我在阿贡国家实验室（Argonne National Laboratory）担任计算研究员。此前，我曾在 KLA（KLA-Tencor）担任算法工程师，也曾在阿贡从事博士后研究。

[**下载 CV**]({{ '/resources/cv_mingdu.pdf' | relative_url }})

## 我的研究

我的研究兴趣主要包括以下方向：

- 用于相干衍射成像相位恢复的先进优化算法与学习型先验，尤其是叠层衍射成像（ptychography）
  - 深度图像先验（deep image prior）[[论文]](https://arxiv.org/abs/2510.20929)
  - 将文生图扩散模型作为生成式plug-and-play prior，用于图像增强 [[论文]](https://opg.optica.org/optica/fulltext.cfm?uri=optica-13-1-45)
- 面向科学问题的高性能计算，包括分布式训练与并行计算
  - 晶体结构预测模型的大规模训练 [[论文]](https://arxiv.org/abs/2603.23367)
  - 分布式ptychography重构 [[论文 1]](https://arxiv.org/abs/2510.20929) [[论文 2]](https://opg.optica.org/oe/fulltext.cfm?uri=oe-29-7-10000)
- 成像与材料表征实验的自动化
  - 用于X射线吸收光谱的贝叶斯自适应采样 [[论文]](https://www.nature.com/articles/s41524-025-01771-7)
  - 面向X射线成像光束线的多模态大语言模型智能体工作流与系统设计 [[论文]](https://www.nature.com/articles/s41524-026-02213-8)
- 复杂成像过程的物理建模与图像重构
  - 通过多切片建模，实现厚度超过焦深的样品的三维成像 [[论文]](https://www.science.org/doi/10.1126/sciadv.aay3700)
- 利用自动微分（AD）求解成像科学中的逆问题
  - 开发支持多种成像方式的自动微分软件包，同时重构图像并求解成像模型中的多种参数 [[论文]](https://opg.optica.org/oe/fulltext.cfm?uri=oe-29-7-10000)
- 用于大规模层析重构的图像处理、计算机视觉与并行计算 [[论文]](https://journals.iucr.org/s/issues/2018/05/00/il5010/)

## 我的软件项目

- Pty-Chi：基于 PyTorch 的 ptychography 重构软件包，同时支持解析计算和自动微分两种引擎 [[GitHub]](https://github.com/AdvancedPhotonSource/pty-chi)
- EAA：面向实验自动化的智能体运行框架。
  - 早期版本基于 LangGraph：[[GitHub]](https://github.com/AdvancedPhotonSource/eaa)
  - 当前版本 EAA-Pi 基于 [Pi](https://pi.dev/) 框架：[[GitHub]](https://github.com/AdvancedPhotonSource/eaa-pi)
- Adorym（已归档）：基于自动微分的软件包，支持多种成像配置与重构参数的求解 [[GitHub]](https://github.com/mdw771/adorym)
- Tomosaic：通过图像拼接扩展层析成像的视场 [[GitHub]](https://github.com/mdw771/tomosaic2)

## 我的爱好

业余时间，我喜欢画插画，尤其是喜欢的 ACG 角色的同人图，最顺手的创作工具是 iPad Pro 和 Procreate。此前，我也在 Vocaloid / SynthV 社区做过音乐视频（使用 After Effects），以及作曲和编曲（使用 Logic Pro）。此外，我还会用 [Blender](https://www.blender.org/) 做一点三维建模和动画。

[业余作品集](https://mdw771.github.io/portfolio/) 页面收录了我的部分过往作品。

## 本站内容

- 基于 Python 的数值计算与数字图像处理；
- 研究数据的统计分析与可视化；
- TensorFlow / Autograd 的使用及其在求解优化问题中的应用；
- 基于 MPI 的基础并行程序设计；
- 面向科学计算的开发环境搭建与配置；
- 一些 Linux 使用技巧。

## 联系我

[ming.du101@gmail.com](mailto:ming.du101@gmail.com)

[Google Scholar 学术主页](https://scholar.google.com/citations?user=fw3MNIsAAAAJ)

