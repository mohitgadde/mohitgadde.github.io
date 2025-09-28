---
layout: page
title: Learning Dynamic Bipedal Walking Across Stepping Stones
description: Helei Duan, Ashish Malik, <strong>Mohitvishnu S. Gadde</strong>, Jeremy Dao, Alan Fern, Jonathan Hurst <em> 2022 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)</em>
img: assets/img/iros2022/intro.png
importance: 5
category: work
giscus_comments: false
related_publications: false
---

<div style="width: 100%; display: flex; justify-content: center; font-size: 25px; margin-bottom: 1rem; flex-direction: row; flex-wrap: wrap;"> 
<a style="margin: 5px;" href="https://ieeexplore.ieee.org/document/9981884" rel="external nofollow noopener" target="_blank">[ Paper ]</a>
<!-- <a style="margin: 5px;">[ code (coming soon) ]</a>  -->
</div>

<h2 id="abstract">Abstract</h2>

In this work, we propose a learning approach for 3D dynamic bipedal walking when footsteps are constrained to stepping stones. While recent work has shown progress on this problem, real-world demonstrations have been limited to relatively simple open-loop, perception-free scenarios. Our main
contribution is a more advanced learning approach that enables real-world demonstrations, using the Cassie robot, of closedloop dynamic walking over moderately difficult stepping-stone patterns. Our approach first uses reinforcement learning (RL) in simulation to train a controller that maps footstep commands onto joint actions without any reference motion information. We then learn a model of that controller’s capabilities, which
enables prediction of feasible footsteps given the robot’s current dynamic state. The resulting controller and model are then integrated with a real-time overhead camera system for detecting stepping stone locations. For evaluation, we develop a benchmark set of stepping stone patterns, which are used to test performance in both simulation and the real world. Overall, we demonstrate that sim-to-real learning is extremely promising
for enabling dynamic locomotion over stepping stones. We also identify challenges remaining that motivate important future research directions.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/iros2022/intro.png" title="Intro Image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Here, Cassie navigates towards the next footstep target using real-time position estimation from an overhead camera. The camera tracks Cassie's position relative to the target, marked by green dots in the simulation. The same stepping stone pattern is used in both hardware and simulation.
</div>

<h2 id="system_arch">System Architecture</h2>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/iros2022/arch.png" title="Intro Image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<!-- <div class="caption">
    System Architecture
</div> -->

The **policy network** combines proprioceptive states processed by an LSTM dynamics module with footstep commands and a periodic clock signal. A feed-forward (FF) layer integrates these inputs to produce PD targets for motor control. This design allows task-specific inputs, such as commands, to adapt without altering the pretrained LSTM module, enabling flexible and reusable policy learning.

<h2 id="reachability_pred">Reachability Prediction Model</h2>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/iros2022/reachability_predictor.png" title="reachability_pred" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<!-- <div class="caption">
    System Architecture
</div> -->

The **Reachability Prediction Model** in the paper is a learned model designed to predict reachable footstep locations for the robot based on its current dynamic state. This model encapsulates the robot's dynamics in a latent state and predicts two key outcomes: the step error for the current footstep target and the latent robot state after the next touchdown. By leveraging these predictions, the model identifies regions where the robot can reliably step within a specified error threshold. Additionally, the model supports multi-step lookahead by recursively predicting future latent states, which is especially useful for navigating highly constrained terrains. It is trained using supervised learning on data that captures relationships between the robot's state, footstep targets, and the resulting errors, enabling robust performance across diverse scenarios.

<h2 id="experiments">Experiments</h2>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/iros2022/sim_reach.png" title="terrain" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Simulation test of the utility of prediction model. <strong>Left:</strong>
Stepping stone locations and initial robot position are randomized
for each trial. Cassie is asked to go across the gap by stepping
onto one of the stones. <strong>Right:</strong> “Random” refers to random selection
among the choices. “Closest” and “Closest on touchdown (TD)
side” refer to selecting the closest (in euclidean sense) stone or
only selecting on stones same as the touchdown side. The success
rate is measured from 1000 independent simulation trials. Having a
reachability model allows Cassie have the highest success rate of
successfully crossing the gap.
</div>

<div class="row justify-content-sm-center mt-3">
    <div class="col-sm-8">
        <div class="video-container">
            <iframe width="100%" height="400" 
                    src="https://www.youtube.com/embed/WsRifd2k-OY" 
                    title="Experiments Video" 
                    frameborder="0" 
                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                    allowfullscreen>
            </iframe>
        </div>
    </div>
</div>

<h2 id="results">Results</h2>

<div class="row justify-content-sm-center">
    <div class="col-12 mt-6 mt-md-0">
        {% include figure.liquid path="assets/img/iros2022/results.png" title="results" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

**(a)** The training curve shows the benefit of using a pretrained dynamics layer, which results in higher reward and faster convergence.
**(b)** The performance of the policy is evaluated in during training (∼50millions) shows that the pretrained method performs better than
trained from scratch on the set of patterns. **(c)** The policy can only see the immediate next footstep command. As part of the emergent
behaviors, the learned step frequency allows the robot to successfully achieve the next target step by taking a longer or shorter swing
duration. The policy also learns to elevate the body height in order to enable longer steps.