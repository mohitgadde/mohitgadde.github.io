---
layout: page
title: Learning Vision-Based Bipedal Locomotion for Challenging Terrain
description: Helei Duan, Bikram Pandit, <strong>Mohitvishnu S. Gadde</strong>, Bart van Marum, Jeremy Dao, Chanho Kim, Alan Fern <em>2024 IEEE International Conference on Robotics and Automation (ICRA)</em>
img: assets/img/icra2024/intro_pic.jpg
importance: 2
category: work
giscus_comments: true
---

<div style="width: 100%; display: flex; justify-content: center; font-size: 25px; margin-bottom: 1rem; flex-direction: row; flex-wrap: wrap;"> 
<a style="margin: 5px;" href="https://ieeexplore.ieee.org/document/10611621" rel="external nofollow noopener" target="_blank">[ Paper ]</a>
<!-- <a style="margin: 5px;">[ code (coming soon) ]</a>  -->
</div>

<h2 id="abstract">Abstract</h2>

Reinforcement learning (RL) for bipedal locomotion has recently demonstrated robust gaits over moderate terrains using only proprioceptive sensing. 
However, such blind controllers will fail in environments where robots must anticipate and adapt to local terrain, which requires visual perception. 
In this paper, we propose a fully-learned system that allows bipedal robots to react to local terrain while maintaining commanded travel speed and direction. 
Our approach first trains a controller in simulation using a heightmap expressed in the robot’s local frame. Next, data is collected in simulation to train a 
heightmap predictor, whose input is the history of depth images and robot states. We demonstrate that with appropriate domain randomization, this approach allows 
for successful sim-to-real transfer with no explicit pose estimation and no fine-tuning using real-world data. To the best of our knowledge, 
this is the first example of sim-to-real learning for vision-based bipedal locomotion over challenging terrains.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/icra2024/intro_pic.jpg" title="Intro Image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Our fully learned controller integrates vision and locomotion for reactive and agile gaits over terrains. The proposed approach enables 
    bipedal robot Cassie traversing over challenging terrains, including random high blocks, stairs, 0.5m step up (∼60% leg length), with 
    speed up to 1m/s.
</div>

<h2 id="system_arch">System Architecture</h2>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/icra2024/system_arch.png" title="Intro Image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of the locomotion policy with vision module.
</div>

The above image illustrates the overall system, which has two main components: (1) a locomotion policy, which outputs PD setpoints for the robot actuators based on proprioception,
a local terrain heightmap, and user commands, and (2) a heightmap predictor, which outputs a predicted heightmap based on proprioceptive information and images from a depth
camera. These components are learned in simulation and then transferred to the real robot (Cassie).


<h2 id="policy">Terrain Aware Policy and Heightmap Predictor</h2>

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-5 text-center">
        {% include figure.liquid path="assets/img/icra2024/policy_arch.png" title="Policy Architecture" class="img-fluid rounded z-depth-1" %}
        <div class="caption">Policy Architecture</div>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0 text-center">
        {% include figure.liquid path="assets/img/icra2024/hm_predictor.png" title="Heightmap Predictor" class="img-fluid rounded z-depth-1" %}
        <div class="caption">Heightmap Predictor</div>
    </div>
</div>

The policy here is represented by a neural network that maps observation sequences to actions. It consists of two key components: 
a pretrained blind policy and a vision-based modulator. The blind policy provides a baseline locomotion control signal suitable for moderate terrains. 
For more complex terrain, the vision-based modulator refines the baseline control by incorporating local terrain details, enabling more adaptive and 
robust locomotion.

**Heightmap Predictor** is a neural network that estimates the terrain heightmap ahead of the robot using depth images and robot states. 
It consists of two stages: **Stage 1** uses an LSTM to reconstruct missing terrain details by leveraging temporal information, trained with mean-squared 
error loss. **Stage 2** refines the heightmap using a U-Net, enhancing edge sharpness and surface flatness with L1 loss, resulting in more 
accurate and stable predictions.


<h2 id="experiments">Experiments</h2>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/icra2024/terrain.png" title="terrain" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Various terrains used to train the terrain aware locmotion policy
</div>

<h2 id="results">Results</h2>

<div class="row justify-content-sm-center">
    <div class="col-12 mt-6 mt-md-0">
        {% include figure.liquid path="assets/img/icra2024/results.png" title="results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

**[A]** Ablation study on policy with simulation heightmap. **[B]** Ablation study on policy with different heightmap predictor architectures.
Each ablation study uses data collected from a range of terrains defined in Table I. **Success rate** indicates the robot does not fall down for
10 seconds of rollouts. **Episodes with foot collision** indicates the number of episodes that have one or more foot collision events occurred
during rollouts, and such random collision events are unfavorable towards hardware deployment. **Termination due to foot collision** shows
the percentage of foot collision events that lead to failures. All plots are evaluated with a confidence interval of **95%**.