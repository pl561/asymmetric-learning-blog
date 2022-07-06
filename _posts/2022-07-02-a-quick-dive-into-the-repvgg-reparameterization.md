---
layout: postmj
title:  A quick dive into the RegVGG reparameterization
date:   2022-07-06 21:38:27 +0000
categories: aicv
---

`RepVGG: Making VGG-style ConvNets Great Again` is paper published in the conference CVPR 2021.

URL: `http://openaccess.thecvf.com/content/CVPR2021/html/Ding_RepVGG_Making_VGG-Style_ConvNets_Great_Again_CVPR_2021_paper.html`

In this blog post, we focus on the mathematical details on the `structural re-parameterization` of a RepVGG block (see section 3.3 and fig. 4 of the paper). This block transformation describes how to transform a RepVGG training-time block to a single RepVGG $3\times3$ inference-time block.

Figure 4 image

We take the same notations from the paper. We have:
- Let $W^{(3)} \in \mathbb{R}^{C_2 \times C_1 \times 3 \times 3}$ to denote the kernel of a $3 \times 3$ convolution layer with $C_1$ input channels and $C_2$ output channels.
- 

