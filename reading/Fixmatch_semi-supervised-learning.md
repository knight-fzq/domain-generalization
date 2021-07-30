# FixMatch
### Week and Strong augmentation
week augmentation：standard flip-and-shift augmentation strategy  
randomly flip images horizontally with a probability of 50% on all datasets except SVHM and randomly translate images by up to 12.5% vertically and horizontally  

strong augmentation：two AutoAugment based methods(Random&CTA) and then Cutout  
##### Cutout
Randomly mask part of an image and fill with zero pixels.
##### AutoAugment

##### RandomAugment
Given a collection of transformations (e.g., color inversion, translation, contrast adjustment, etc.),
RandAugment randomly selects transformations for each sample in a mini-batch. RandomAugment uses a single fixed global magnitude(optimized by grid search) that controls the severity of all distortions. 

##### CTAAugment

### Consistency regularization
%%
\sum_{b=1}^µB{||p_{m}(y|α(u_{b}))-p_{m}(y|α(u_{b}))||_{2}^2}
%%
### pseudo-labeling
$$
x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}
$$
