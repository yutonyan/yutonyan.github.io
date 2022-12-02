---
layout: archive
title: ""
permalink: /research/
author_profile: true
---

{% include base_path %}

## Finance

<!-- <hr size="5" noshade>  -->
<hr size="5">

### The Rise of Sustainable Investing: Does Employment of ESG Talents Predict Mutual Funds Activities?
[Request PDF](https://forms.gle/Yxf5xChwoUpibvr4A)&nbsp;&nbsp;&nbsp; [Slides](https://yutongyan.xyz/files/research_proposal_slides.pdf)
   <details open>
    <summary>Abstract</summary>
In this proposal, I plan to examine whether hiring ESG-specialized professionals in mutual funds is related to the funds’ portfolio choice, ESG rating, and subsequent funds’ high ESG portfolio performance and fund flows. It will shed light on the role of ESG-specialized human capital in asset management business in the presence of fast growth in ESG investments. ESG-specialized professionals, who are knowledgeable about the nuances of climate change issues and sustainability, are expected to assist mutual funds to make strategic actions while navigating ESG-related risks and opportunities. To compile a comprehensive LinkedIn-based mutual fund employee database, I use large-scale web scraping and textual analysis techniques to analyze ESG employees’ education backgrounds, past work experiences, expertise (e.g., science- vs management-oriented), etc. In addition, I merge the ESG employee information with the standard mutual fund databases (CRSP and Thomson Reuters) to create a Linkedin employee-fund database. I hypothesize that hiring ESG experts should be associated with the improvement in fund ESG rating and portfolio ESG score, better fund performance on high ESG stocks, and more fund flows.
    </details>

<!--   <div style="background-color:#D3D3D3;padding:20px;"> -->
## Machine Learning

### Bandit Algorithms for Factorial Experiments
  [PDF](https://yutongyan.xyz/files/bandits_for_factorial_report.pdf) &nbsp;&nbsp;&nbsp; [Poster](https://yutongyan.xyz/files/bandits_for_factorial_poster.pdf) &nbsp;&nbsp;&nbsp;  [Slides](https://yutongyan.xyz/files/bandits_for_factorial_slides.pdf)  
  Authors: Yutong Yan, Audrey Durand and Joelle Pineau  
  Venue: [WiML Workshop, Conference on Neural Information Processing Systems 2019](https://wimlworkshop.org/)
  <details>
    <summary>Abstract</summary>
<!--     <div class="language-plaintext highlighter-rouge"> -->
    A multi-armed bandit algorithm is developed for factorial experiments. Using tools from advanced probability theory, I first prove that UCT algorithm with Laplace bound has a lower computational complexity than the naïve UCT algorithm. I begin by analyzing UCB1 for non-stationary bandit problems, and then prove UCT algorithm with Laplace Bounds achieves a better lower bound. Also, I demonstrate that the probability of suboptimal choices will converge to zero with a convergence of failure probability. In settings of deep learning, experimental results are also consistent with the theoretical regret bound.
<!--     </div> -->
  </details>

### A Theoretical Analysis of Upper Confidence Bound applied to Trees
  [PDF](https://yutongyan.xyz/files/uct_proof.pdf) &nbsp;&nbsp;&nbsp; [Slides](https://yutongyan.xyz/files/uct_slides.pdf)  
  Authors: Yutong Yan  
  Research Supervisors: Prof. Joelle Pineau
  <details>
    <summary>Abstract</summary>
      Using Factorial experiments, I explore multi-armed bandit problems in which a player selects actions (here a sequence) episodically and observes the outcomes. I consider the Upper Confidence Bound applied to Trees (UCT), a popular algorithm for tree search, in order to identify the sequence of choices that maximizes some objective function. Using synthetic experiments, I demonstrate that applying tighter concentration bounds to Linear Bandits can significantly improve the performance of UCT for tree search. Next step is to investigate various factorial experimental design configurations. I also compare the performance of algorithms under three different formulations of the factorial experiment: 1) standard bandits; 2) linear bandits; and 3) bandits for tree search. I observe that capturing the underlying tree structure is essential for robustness, whether the outcome function is linear or not. Furthermore, I observe that the algorithms employed under the bandits formulation of tree search for factorial experimental designs appear more robust to the noise variance than other approaches.
  </details>
  
### Improving Reward Shaping via Language Instruction
  [PDF](https://yutongyan.xyz/files/rl_nlp_report.pdf) &nbsp;&nbsp;&nbsp; [Slides](https://yutongyan.xyz/files/rl_nlp_slides.pdf)  
  Authors: Yutong Yan, Irene Zhang and Siva Reddy  
  <details>
    <summary>Abstract</summary>
      Recent literature have started exploring problems in language-assisted reinforcement learning (RL). This is a setting where language itself is not necessary, but incorporating language can be advantageous for solving the task. In this work, We explore one specific case of language-assisted RL, where we address the challenges of solving tasks with sparse reward. We do so by using reward shaping through leveraging natural language instructions. Our project extends a recent work, in which a framework called LanguagE-Action Reward Network (LEARN) is proposed. LEARN maps free-form natural language instructions to intermediate rewards based on actions taken by the agent. The work has shown that language-based rewards can lead to performance advancement compared to learning without language through extensive experiments. However, one draw back of the method is that the reward shaping is only dependent on the actions for simplicity. In this work, we would like to extend the approach by learning a state representation using a VAE, and conduct reward shaping based on both actions and the state representation. Our empirical results show that the VAE is not able to learn representations that can significantly improve baselines, but other methods of representation learning for states should be investigated in future work.
  </details>


### Improving Self-supervised Learning in Computer Vision with Generative Models
  [PDF](https://yutongyan.xyz/files/cv_report.pdf) &nbsp;&nbsp;&nbsp; [Slides](https://yutongyan.xyz/files/cv_slides.pdf)  
  Authors: Zhen Liu and Yutong Yan
  <details>
    <summary>Abstract</summary>
      We investigate the possibility of leveraging generative models to improve the performance of self-supervised training. With the observation that convolutional structures in both encoders and decoders can capture priors in natural images, we speculate that a pretrained generative models on the same or a similar dataset can improve both the accuracy of self-supervisedly trained models and the convergence of self-supervised training. We experimented with two major approaches: 1) use images generated by pretrained generative models and 2) use feature maps from the discriminator/energy-based model of pretrained generative model. We empirically found that while it can be hard for image augmentations to work in general, BYOL can benefit from augmentations with either forms.
  </details>


<!--
{% for post in site.writing-sample reversed %}
  {% include archive-single.html %}
{% endfor %}
-->
