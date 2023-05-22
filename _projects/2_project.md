---
layout: page
title: RGB-Depth misalignment
description: RGB and depth misalignment in Azure Kinect cameras using innovative differentiable rendering method.
img: assets/img/misalignment.png
importance: 3
category: research
---

We wanted to solve the RGB and depth misalignment in the Azure Kinect cameras so that we could
capture high-quality texture along with the geometry. For this we used a differentiable rendering-based pipeline
to calculate the transformation of the RGB lens w.r.t. the depth lens.



<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/tmp.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/tgt_img5016_1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Misalignment before correction. Sample A (left) and Sample B (right)
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/src_img5016_1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/tgt_img5016_1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Misalignment after correction.
    The more overlap between the ground truth sample A and Azure Kinect sample B, the less the error.
</div>
