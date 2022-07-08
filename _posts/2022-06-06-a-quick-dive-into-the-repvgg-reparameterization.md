---
layout: postmj
title:  A quick dive into the RegVGG reparameterization
date:   2022-06-06 21:38:27 +0000
categories: aicv
---

`RepVGG: Making VGG-style ConvNets Great Again` is paper published in the conference CVPR 2021.

URL: `http://openaccess.thecvf.com/content/CVPR2021/html/Ding_RepVGG_Making_VGG-Style_ConvNets_Great_Again_CVPR_2021_paper.html`

In this blog post, we focus on the mathematical details on the `structural re-parameterization` of a RepVGG block (see section 3.3 and fig. 4 of the paper). This block transformation describes how to transform a RepVGG training-time block to a single RepVGG $$3\times3$$ inference-time block.

Figure 4 image

We take the same notations from the paper. We have:
- Let $$W^{(3)} \in \mathbb{R}^{C_2 \times C_1 \times 3 \times 3}$$ to denote the kernel of a $$3 \times 3$$ convolution layer with $$C_1$$ input channels and $$C_2$$ output channels.
- Let the definition of $$W^{(1)}$$ be analogous of $$W^{(3)}$$

In figure 4, the authors display the architecture of a RepVGG block at training-time and inference-time along with an illustration of the re-parameterization equations. At training-time, we have the output expressed as:
$$ y = y_1 + y_2 + y_3 = BN^{(3)}(Conv^{(3)}(x)) + BN^{(1)}(Conv^{(1)}(x)) + BN^{(0)}(x)$$

$$ y = BN^{(3)}(W^{(3)}x + b^{(3)}) + BN^{(1)}(W^{(1)}x + b^{(1)}) + BN^{(0)}(x)$$

$$ y = BN^{(3)}(W^{(3)}x + b^{(3)}) + BN^{(1)}(W^{(1)}x + b^{(1)}) + BN^{(0)}(x)$$

$$ y = \gamma^{(3)}(W^{(3)}x + b^{(3)}) + \beta^{(3)} + \gamma^{(1)}(W^{(1)}x + \beta^{(1)}) + \gamma^{(0)}x + \beta^{(0)}$$

The expression $$\gamma^{(0)}x + \beta^{(0)}$$ can be rewritten as a $$1\times 1$$ convolution with identity kernel, padding $$0$$ and stride $$1$$.

In a second time, let's write the re-parameterization equation of a $$1 \times 1$$ convolution into a $$3 \times 3$$ convolution. Let us note first that the convolution of kernel $$W^{(3)}$$ has padding $$1$$ and stride $$1$$ (so that the input and output resolution of the features remain the same. Similarly, the $$1 \times 1$$ convolution has padding $$0$$. Obviously, the makes the resulting sum $$ y = y_1 + y_2 + y_3$$ of the mutli branch block possible.

In these conditions, a $$1 \times 1$$ convolution kernel $$W_a^{(1)}$$ can simply be padded with zeros to a corresponding convolution kernel $$W_a^{(3)}$$. As a result the new $$3 \times 3$$ convolution will have padding $$1$$ instead of $$0$$. In this case, we have the following equality: $$ W_a^{(1)}x = W_a^{(3)}x $$.

Now, when we rewrite the convolution sum, the kernel part of the summation can be added which is eqwuivalent to sum the two kernels:
$$ \sum_limits_{a, b} \sum_limits_{i, j} F(a + i, b + j) \times W^{(3)}(i, j) + \sum_limits_{a, b} \sum_limits_{i, j} F(a + i, b + j) \times W_a^{(3)}(i, j) = \sum_limits_{a, b} \sum_limits_{i, j} F(a + i, b + j) \times (W^{(3)}(i, j) + W_a^{(3)}(i, j))$$.

In other words, the new kernel $$W'^{(3)} = W^{(3)} + W_a^{(3)}$$ can be viewed as $$W'^{(3)}(i, j) = W^{(3)}(i, j)$$ if $$(i, j) \neq (1, 1)$$ and $$W'^{(3)}(1, 1) = W^{(3)}(1, 1) + W_a^{(3)}(1, 1)$$.







