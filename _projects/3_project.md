---
layout: page
title: Fusion4D
description: A fusion method proposed by Microsoft Research
img: assets/img/fusion4d.png
importance: 2
category: research
---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/bad_noc.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Badly calibrated and noisy mesh.
</div>

<ul>
<li><h3>Function4D like approach</h3><p>In Function4D, a volume at frame t is warped to frame t + 1 by warping the voxels defining the volume. The warp function is estimated from non-rigid ICP on the embedded deformation graph associated with the source mesh as frame t and the dense pointcloud of frame target frame t + 1 and extrapolated to voxels using dual quaternion blending.
This warped volume is then blended with the volume at t + 1 created from the depth maps. As the clean, fused mesh was being warped every time, we kept adding noise to it (from the non-rigid ICP and blending) and hence the final result was noisy.</p>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/function_noc.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Noisy mesh after Function4D based fusion.
</div>

It was decided to keep a stationary canonical volume and blend the already noisy live data to this clean volume. As such even if a bit of noise is added to the already noisy data, it won't make too much of a difference. Then the canonical mesh can be warped to the live frame using the inverse of the warp function already calculated. This was the approach followed by DynamicFusion hence we tried that next.</li>

<li><h3>DynamicFusion</h3><p>In DynamicFusion the volume created from depth maps at the zeroth frame is treated as the canonical volume. The warp function is calculated between frame t and t + 1. Using the warp function between frame 0:t and t:t + 1 the warp function between frame 0:t + 1 is obtained. Then the volume at frame t + 1 is warped to the zeroth frame (by warping the voxels) and fused there. Then the mesh is extracted from this fused volume and warped to frame t + 1 by warping the mesh vertices and normals using the same warp function calculated above. This did not provide any provision for non-rigid ICP failing and the topology changing. As such long-range tracking failed and artifacts were introduced in the canonical frame which introduced noise in the canonical mesh warped to the live frame.</p>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/df_clean.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/df.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Fused and warped clean live mesh. Right: Mesh after tracking failed
</div>
</li>


<li><h3>Fusion4d</h3><p>Fusion4D addressed most of the shortcomings of the previous methods. It maintained two volumes, the data volume, and the canonical volume. The data volume at the live frame was improved upon by the canonical volume. Also, the canonical volume was updated to the data volume periodically to account for topology changes, tracking failures, etc.
Fusion4D warped the live data to the canonical frame and updated it similarly to DynamicFusion. At the same time, it also warped the canonical volume to the live frame very selectively and fused the live data there to create the data volume. This data volume had losses to ensure quality at least as good as the live data despite tracking failure, etc.
The canonical volume was warped and a regular voxel lattice at the data volume was selectively filled using gradient-based interpolation and euclidean distance-based weighting from the warped voxels. Then the live data was selectively fused into this regular lattice.
A major improvement by Fusion4D over DynamicFusion was the addition of rendering-based losses. It rendered the data volume and compared it with live data to ensure there was no misalignment, both while warping the canonical volume and while fusing the live volume into the warped canonical volume. It also accounted for voxels coming from different surfaces colliding with each other.</p>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/fusion4d.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Fusion4D mesh result.
</div>

This solved most of the problems in the fusion process and was decided as the final algorithm to be used, followed by some post-processing to remove boundary noise, etc.
</li>
</ul>

