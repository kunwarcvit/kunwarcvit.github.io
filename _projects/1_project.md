---
layout: page
title: Depth based calibration
description: Azure Kinect camera calibration using novel depth based calibration method.
img: assets/img/cali.png
importance: 1
category: research
---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/cali.png" title="Calibration image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Calibration results using ground truth model
</div>

<p>
We wanted to solve the calibration of the Azure cameras. While camera calibration is solved using checkerboards for RGB cameras, the RGB and depth in the Azure Kinect cameras are misaligned because of incorrect warping matrix from factory. As such, we had to perform calibration of the cameras in-depth space. For this, we tried multiple approaches as follows:
</p>

<ul>
<li><h3>Single model calibration for all cameras</h3><p>Here we had a single stationary model (either a rigid structure or a human standing still) and we captured the model for about 10 seconds from all the cameras. Then we performed Kinect Fusion on this data to remove the noise and tried to align all the cameras to a single camera using ICP (initialised using noisy RGB based ARUCO). e.g. if camera 1 is chosen as the base camera, then first camera 2 is aligned to camera 1 and then camera 3 is aligned to camera 2, etc. 
There was not enough overlap between all the cameras in this method and the alignment for cameras with overlap wasn't reliable either. Therefore we moved to the next approach.</p></li>

<li><h3>Different model for each camera pair</h3><p>To ensure enough overlap between the camera pairs we used a different model between each camera pair. This was created in a way that ensured overlap between the camera pairs. Since we knew which camera pairs we needed for calibration, we only needed to do it for those pairs. e.g. we didn't need to do it for camera pairs 3 and 5 since 5 will be aligned using camera pairs 4 and 5. We performed Kinect Fusion on these meshes, removed the boundary tangents, and tried calibrating using these pairwise meshes.
Despite having enough overlap to provide sufficient correspondences, the quality of the alignment using ICP was not reliable because both the meshes in the pair had some boundary noise left.</p></li>

<li><h3>Aligning each camera to a ground truth</h3><p>Finally we decided to use a ground truth mesh obtained using a high-quality handheld scanner and to align all the meshes to this ground truth. This mesh will be aligned to the first camera by providing manual correspondences and running scalable ICP and then every camera was aligned to this ground truth mesh after that. This ensured enough overlap and good quality mesh data for the meshes from the Azure camera to align to using ICP.</p></li>

</ul>
