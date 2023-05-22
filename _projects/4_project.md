---
layout: page
title: 3D virtual tryon dataset
description: A dataset for evaluation of 3D virtual tryon
img: assets/img/kurta.png
importance: 4
category: research
---

The aim of this project was to create a dataset for 3D virtual try-on. I created a dataset with static human
models adorned in various poses of the same garment, along with diverse body types wearing identical garments,
using a multiview Azure Kinect camera setup.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/kurta.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dress.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Dataset samples
</div>

We used the multiview Azure Kinect Camera setup, calibrated using ground truth model based calibration tech-
nique. The captured data was refined by running KinectFusion and Screened Poisson Surface Reconstruction
was used to fill in holes.

Finally we collected a total of 250 meshes, featuring 15 subjects and encompassing 44 distinct garment types,
including loose garments like skirts and dhotis.

This dataset was used to evaluate a self-supervised garment retargeting method. The method, along with
the dataset was submitted to ICCV 2023.