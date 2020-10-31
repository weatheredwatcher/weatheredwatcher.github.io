---
title:  "Two of our papers were accepted at ICML 2020"
date:   2020-07-17
categories: [summaries, papers]
tags: [conferences, publications]
---

This week we presented two of our papers at ICML 2020, it was a great
experience to talk with others about our research and to think of future
directions and applications of our methods.  I want to use this page to point
out reference materials for these publications.

## Set Functions for Time Series

"Set Functions for Time Series" is work done together with [Michael
Moor](https://michaelmoor.ml/), [Christian Bock](https://christian.bock.ml), [Bastian
Rieck](https://bastian.rieck.me/) and my PhD Advisor Karsten Borgwardt.

In the core we propose to rephrase learning on irregularly sampled time series
data as a set classification problem. This mitigates the necessity of imputing
time series prior to the application of Deep Learning models and allows their
direct application.  Michael Larionov wrote a nice summary of our paper
[on towards data science](https://towardsdatascience.com/set-attention-models-for-time-series-classification-c09360a60349)
where he explains the core components of the model.

For further details please see the links below:

**Paper:**
[ICML 2020](https://proceedings.icml.cc/static/paper_files/icml/2020/4750-Paper.pdf),
[arXiv](https://arxiv.org/abs/1909.12064)

**Talk:**
[ICML 2020](https://icml.cc/virtual/2020/poster/6545), [Slides](/assets/2020-07-16-ICML-SeFT-TopoAE/SeFT-slides.pdf)

**Code:**
[Models](https://github.com/BorgwardtLab/Set_Functions_for_Time_Series), [Datasets](https://github.com/ExpectationMax/medical_ts_datasets)

## Topological Autoencoders
The work "Topological Autoencoders" was joint work with my colleagues [Michael
Moor](https://michaelmoor.ml/) and [Bastian Rieck](https://bastian.rieck.me/)
and my PhD Advisor Karsten Borgwardt.

<img alt="Training visualization Topological Autoencoder" src="/assets/2020-07-16-ICML-SeFT-TopoAE/topoae.gif" width="49%"><img alt="Training visualization Vanilla Autoencoder" src="/assets/2020-07-16-ICML-SeFT-TopoAE/vanilla.gif" width="49%">

In this work we propose to constrain the topology of the  latent representation
of an autoencoder using methods from topological data analysis.
Michael wrote a wonderful blog post about the paper giving an intuitive
introduction [here](https://michaelmoor.ml/blog/topoae/main/).
Below you can see our devised approach in
action compared to a vanilla autoencoder.

**Paper:**
[ICML 2020](https://proceedings.icml.cc/static/paper_files/icml/2020/613-Paper.pdf),
[arXiv](https://arxiv.org/abs/1906.00722)

**Talk:**
[ICML 2020](https://icml.cc/virtual/2020/poster/5851), [Slides](/assets/2020-07-16-ICML-SeFT-TopoAE/TopoAE-slides.pdf)

**Code:** [GitHub](https://github.com/BorgwardtLab/topological-autoencoders)

