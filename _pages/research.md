---
layout: archive
title: ""
permalink: /research/
author_profile: true
---

{% include base_path %}

## Finance

<!-- <hr size="5" noshade>  -->
<hr size="3" noshade>

### A Test of Lookahead Bias in LLM Forecasts
[arxiv]()&nbsp;&nbsp;&nbsp; [ssrn]()  
   *Keywords*: 
   <details open>
    <summary>Abstract</summary>
<!--     <div class="language-plaintext highlighter-rouge"> -->

<!--     </div> -->
    </details>

### DatedGPT: Preventing Lookahead Bias in Large Language Models with Time-Aware Pretraining
  [Poster]() &nbsp;&nbsp;&nbsp; 
  Co-authors: 
  *Keywords*: Machine Learning, Optimization  
  Venue: [WiML Workshop, Conference on Neural Information Processing Systems 2019](https://wimlworkshop.org/)
  <details>
    <summary>Abstract</summary>
<!--     <div class="language-plaintext highlighter-rouge"> -->

<!--     </div> -->
  </details>



### Deciphering Green Preferences and Climate Risk Perceptions: An NLP Approach (*draft available upon request*)
   Authors: Darwin Choi, Zhenyu Gao, Wenxi Jiang, *Yutong Yan*, and Hulai Zhang  
   *Keywords*: Climate Finance, Institutional Investors, ESG, Textual Analysis, Natural Language Processing
  <details>
    <summary>Abstract</summary>
<!--     <div class="language-plaintext highlighter-rouge"> -->

<!--     </div> -->
  </details>




## Machine Learning

<hr size="3" noshade>



### Bandit Algorithms for Factorial Experiments
  [PDF](https://yutongyan.xyz/files/bandits_for_factorial_report.pdf) &nbsp;&nbsp;&nbsp; [Poster](https://yutongyan.xyz/files/bandits_for_factorial_poster.pdf) &nbsp;&nbsp;&nbsp;  [Slides](https://yutongyan.xyz/files/bandits_for_factorial_slides.pdf)  
  Co-authors: Audrey Durand and Joelle Pineau  
  *Keywords*: Machine Learning, Optimization  
  Venue: [WiML Workshop, Conference on Neural Information Processing Systems 2019](https://wimlworkshop.org/)
  <details>
    <summary>Abstract</summary>
<!--     <div class="language-plaintext highlighter-rouge"> -->
    A multi-armed bandit algorithm is developed for factorial experiments. Using tools from advanced probability theory, I first prove that UCT algorithm with Laplace bound has a lower computational complexity than the naïve UCT algorithm. I begin by analyzing UCB1 for non-stationary bandit problems, and then prove UCT algorithm with Laplace Bounds achieves a better lower bound. Also, I demonstrate that the probability of suboptimal choices will converge to zero with a convergence of failure probability. In settings of deep learning, experimental results are also consistent with the theoretical regret bound.
<!--     </div> -->
  </details>

### A Theoretical Analysis of Upper Confidence Bound applied to Trees
  [PDF](https://yutongyan.xyz/files/uct_proof.pdf) &nbsp;&nbsp;&nbsp; [Slides](https://yutongyan.xyz/files/uct_slides.pdf)  
  Co-authors: Audrey Durand and Joelle Pineau  
  *Keywords*: Machine Learning, Optimization  
  <details>
    <summary>Abstract</summary>
      Using Factorial experiments, I explore multi-armed bandit problems in which a player selects actions (here a sequence) episodically and observes the outcomes. I consider the Upper Confidence Bound applied to Trees (UCT), a popular algorithm for tree search, in order to identify the sequence of choices that maximizes some objective function. Using synthetic experiments, I demonstrate that applying tighter concentration bounds to Linear Bandits can significantly improve the performance of UCT for tree search. Next step is to investigate various factorial experimental design configurations. I also compare the performance of algorithms under three different formulations of the factorial experiment: 1) standard bandits; 2) linear bandits; and 3) bandits for tree search. I observe that capturing the underlying tree structure is essential for robustness, whether the outcome function is linear or not. Furthermore, I observe that the algorithms employed under the bandits formulation of tree search for factorial experimental designs appear more robust to the noise variance than other approaches.
  </details>


<!--
{% for post in site.writing-sample reversed %}
  {% include archive-single.html %}
{% endfor %}
-->
