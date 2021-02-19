# Fully Convolution One Stage Object Detector (FCOS)

Paper link: [fcos][link id]

FCOS, as its name, is an one-stage object detector that only use fully convolution network to predict result bounding box. In my opinion, the authors:
- propose an one-stage network that can predict boxes without using anchors.
- propose a solution for ambiguity of labels on overlapping bounding boxes.
- achieve state-of-the-art result for anchor-free object detector
  
**Problem Formulation**

Given an image, we pass it through a FCN to get *feature vectors* represent the whole image. Assume the feature vectors have size of $Ch*H*W$, then the goal of FCOS is to predict 3 things (or so called 3 branches):
- $4*H*W$ mask for (left, top, right, bottom) prediction for each pixel, where l,t,r,b are calculated by:
  - $l = x- x_0$
  - $r = x_1 - x$
  - $t = y - y_0$
  - $b = y_1 - y$
- $C*H*W$ mask for class prediction for each pixel. (where C represents the number of classes).
- $1*H*W$ mask for centerness score prediction. The centerness score is calculated as follow:
    $$centerness = \sqrt{\frac{min(l,r)}{max(l,r)}*\frac{min(b,t)}{max(b,t)}}$$

****
**Arising Problems that was tackled in the paper**

**1. Ambiguity of overlapping boxes:**   
   So the very first question is: How can you determine (l,t,r,b) of a *pixel in joint regions* of 2 or more bounding boxes? *Which box* should the label be account for?

   So in the paper, the author choose to label it with the smallest-area box. The label for class for this boxes will be ambiguity, rather than to assign to any specific label.
    
**2. Multi-scale detection:** 

Then in order to solve the problems of multi-scale overlapping object, the authors also use a feature pyramid network and calculate the loss at multiple heads, then they join predictions at all scales together for the final result.

With multi-scale detection, we can eliminate a lot of pixels fall into ambiguity since the pixels in bigger scale will account for the big boxes, while the ones in small scale account for the small boxes. Of course, there will still be some boxes of ambiguity, but it should not harm to the model (as the author said).

[link id]: https://arxiv.org/pdf/1904.01355.pdf