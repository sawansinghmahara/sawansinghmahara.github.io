---
layout: page
title: "Multi-Task Federated Learning"
categories: Research
date:   2021-03-22 21:21:21 +0530
tags:
  - Federated Learning
  - Multi-Task Learning
  - Machine Learning
subtitle: In collaboration with Shruti. M and Bharath. B. N
permalink: "/_posts/mtfeel"
---
# Super Short Summary

Taking on the challenges of distributed personalised learning, here is a novel learning algorithm suited to a wireless setting. The proposed scheme utilises a signed gradient feedback that reduces communication overheads while providing each participating device with a custom neural network upon completion. 

The customised neural network provided to each device, characterising the multi-task nature of the algorithm, is found by a weighted average loss across devices that considers the similarity between their data distributions. 

A Probably Approximately Correct (PAC) bound on the true loss in terms of the proposed empirical loss is derived as well, which is in terms of

* The Rademacher complexity
* The discrepancy
* A penalty term.

The distributed algorithm finds the discrepancy between devices as well as fine tunes the neural networks for each of them. Given some set conditions, the proposed algorithm is mathematically shown to converge in a Rayleigh fading uplink channel as well. Some of the experiments are discussed and it was found to outperform having each node just use local training, as well as the conventionally used FedAvg and FedSGD algorithms. In a corrupting channel regime, it is shown to outperform the other viable alternative, FedSGD, until a certain SNR level!

# Details!

Will be put up as soon as possible!

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
