---
title:  "MLSS2020 Causality Lectures - a brief summary"
date:   2020-06-05
categories: [summaries, MLSS2020]
tags: [conferences]
---

The first week of the virtual Machine Learning Summer School in Tübingen is over
and it is time to take a brief look back at the lessons learned and the
experience made during this time. In the following I will briefly some of the
insights I made during the first week.

## Causality and Causal Inference lectures
The Causality lectures were held by [Bernhard
Schölkopf](https://www.is.mpg.de/~bs) and [Stefan
Bauer](https://www.is.mpg.de/person/sbauer). As a heads up, I am most definitely
not an expert in this field and I am solely summarizing the most interesting
points according to my personal opinion :).

### Causality I
The first lecture by Prof. B. Schölkopf was a more general introduction to
causality, the required terms needed to understand the literature, how to
infer causal structure in smaller scale experiments with stochastic or
deterministic relationships between observations and finally the implications
of causality to semi-supervised learning.

Here I found the work on deriving causality for deterministic cases of
particular interest (see [this](https://arxiv.org/abs/1203.3475) paper).  In
this work, the authors use an assumption termed the *independence of input and
mechanism* to derive a causal inference rule which does not require any
assumptions on noise. The assumption states, that $ p(C) $ (the probability
distribution of the cause) and $p(E|C)$ (the probability distribution of the
effect conditional on the cause) should be independent. This implies, that the
distribution of the effect would then in some way be dependent on the function
$f$ mapping from cause to effect and thus $Cov(log f', p_C) = 0$ (encoding
the independence assumption) and $Cov(log f^{-1'}, p_E) > 0$ (encoding the
dependence between the function mapping from cause to effect and the
distribution of the effect). This leads to the inference rule that
$X \rightarrow Y$ if  $\int \log | f'(x) | p(x) dx \leq \int \log | f^{-1}(y)|
p(y) dy $. This can also be computed using empirical estimators for the
slope of the function mapping between X and Y.

Finally, B. Schölkopf presented implications of causality in the domain of
semi-supervised learning. In particular, if *independence of input and
mechanism* is true, he shows that semi-supervised learning can theoretically not
benefit learning in the causal direction. In other words when a model is trying
to infer effect from cause (thus $p(E|C)$), additional data from the cause
distribution $p(C)$ will not help the model learn due to $ p(C) $ being
independent of $ p(E|C) $. In contrast, if learning is in the anti-causal
direction, semi-supervised learning can be beneficial.

### Causality II
The second talk by Stefan Bauer focused on how to infer Structural Causal
Models and how causality can be integrated with Deep Learning approaches.

In the first part of his talk, Stefan showed bridges between causal inference
and non-linear ICA and further shows that there will always be both a causal as
well as anti-causal linear model if additive Gaussian noise is assumed.
Afterwards, Stefan talks about time series models and how ODEs are perfect
models for causal structure in the presence of time.

In the second half, the topic switches to describing a causal perspective on
representation learning. Here the bridges between disentangles representations
and causality become evident. This is due to the assumption, that a change in
distribution of the data would arise from a sparse change in causal
conditionals or causal mechanisms, thus leading to similar properties as
desired in disentangled representations.  Nevertheless, recent research shows
that disentangled representations cannot be learned in a completely
unsupervised way (see [Locatello et al, ICML
2019](https://arxiv.org/abs/1811.12359)), also leading to potential issues with
the discovery of causal mechanisms.  There is some hope though, as few labels
seem to help with determining correct disentangled representations (see
[Locatello et al., ICML 2020](https://arxiv.org/abs/2002.02886)).

Finally, Stefan talks about some exciting work on encoding causal structure
into machine learning architectures.  Here a decoder is designed to resemble
the structure of a general Structural Causal Model and trained to match the
observations of the training data.  These *Structural Causal Autoencoders* (see
[Leeb et al., under review NeurIPS 2020](https://arxiv.org/abs/2006.07796))
were shown to yield good performance in learning representations for generating
images and transferring between similar tasks.

All in all some very exciting directions. I am looking forward to seeing the
future development of Causality and Machine Learning.
