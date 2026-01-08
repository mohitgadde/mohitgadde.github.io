---
layout: page
title: "No More Blind Spots: Learning Vision-Based Omnidirectional Bipedal Locomotion for Challenging Terrain"
description: <strong>Mohitvishnu S. Gadde</strong>, Pranay Dugar, Ashish Malik, Alan Fern <em>2025 IEEE Humanoids</em>
img: assets/img/humanoids2025/intro_img.jpg
importance: 2
category: work
giscus_comments: false
related_publications: false
---

<div style="width: 100%; display: flex; justify-content: center; font-size: 25px; margin-bottom: 1rem; flex-direction: row; flex-wrap: wrap;"> 
<a style="margin: 5px;" href="https://arxiv.org/abs/2508.11929" rel="external nofollow noopener" target="_blank">[ Paper ]</a>
<!-- <a style="margin: 5px;">[ code (coming soon) ]</a>  -->
</div>

<h2 id="abstract">Abstract</h2>

Effective bipedal locomotion in dynamic environments, such as cluttered indoor spaces or uneven terrain,
requires agile and adaptive movement in all directions. This
necessitates omnidirectional terrain sensing and a controller
capable of processing such input. We present a learning framework for vision-based omnidirectional bipedal locomotion, enabling seamless movement using depth images. A key challenge
is the high computational cost of rendering omnidirectional
depth images in simulation, making traditional sim-to-real
reinforcement learning (RL) impractical. Our method combines
a robust blind controller with a teacher policy that supervises a
vision-based student policy, trained on noise-augmented terrain
data to avoid rendering costs during RL and ensure robustness.
We also introduce a data augmentation technique for supervised
student training, accelerating training by up to 10 times
compared to conventional methods. Our framework is validated through simulation and real-world tests, demonstrating
effective omnidirectional locomotion with minimal reliance on
expensive rendering. This is, to the best of our knowledge,
the first demonstration of vision-based omnidirectional bipedal
locomotion, showcasing its adaptability to diverse terrains.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/humanoids2025/intro_img.jpg" title="Intro Image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Our fully learned controller integrates vision and locomotion for reactive and agile gaits over terrains. The proposed approach enables 
    bipedal robot Cassie traversing over challenging terrains, including random high blocks, stairs, 0.5m step up (∼60% leg length), with 
    speed up to 1m/s.
</div>

<h2 id="approach">Approach</h2>

A key challenge in omnidirectional vision-based locomotion is the high computational cost of rendering omnidirectional depth images during reinforcement learning in simulation. Our approach addresses this through a three-stage training framework:

**Stage 1: Blind Controller.** We first train a robust blind locomotion policy using privileged terrain information. This controller learns to walk in all directions without visual input, providing a strong foundation for subsequent stages.

**Stage 2: Teacher Policy.** We train a teacher policy that uses ground-truth terrain heightmaps to supervise omnidirectional locomotion. The teacher builds on the blind controller and learns to modulate actions based on terrain geometry.

**Stage 3: Vision-Based Student.** Finally, we train a student policy that uses depth images instead of ground-truth heightmaps. The student is trained via supervised learning on noise-augmented terrain data, avoiding the need for expensive depth rendering during RL. We introduce a data augmentation technique that accelerates this training by up to 10x compared to conventional methods.


<h2 id="experiments">Experiments</h2>

We validate our framework through both simulation and real-world experiments on the Cassie bipedal robot. The robot is tested on challenging terrains including random stepping stones, stairs, and large step-ups (up to 0.5m, approximately 60% of leg length).

<div class="row justify-content-sm-center mt-3">
    <div class="col-sm-8">
        <div class="video-container">
            <iframe width="100%" height="400"
                    src="https://www.youtube.com/embed/6kgB8PqvLYU"
                    title="Experiments Video"
                    frameborder="0"
                    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                    allowfullscreen>
            </iframe>
        </div>
    </div>
</div>

<h2 id="results">Results</h2>

Our method achieves omnidirectional walking at speeds up to 1 m/s using depth camera input. Key results include:

- **10x training acceleration** compared to conventional rendering-based approaches
- Successful sim-to-real transfer on the Cassie robot
- Robust locomotion across diverse challenging terrains
- First demonstration of vision-based omnidirectional bipedal locomotion