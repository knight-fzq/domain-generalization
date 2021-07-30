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
Consistency regularization utilizes unlabeled data by relying on the assumption that model should output similar predictions when fed perturbed versions of the same images.  
$\sum_{b=1}^{µB}{||p_{m}(y|α(u_{b}))-p_{m}(y|α(u_{b}))||_{2}^2}$

##### In this paper
When the confidence of prediction is quite high, the prediction of week augmentation should be equal to strong augmentation.  
##### Definition of loss
$\l_{u}=\frac{1}{μB}\sum_{b=1}^{μB}{1(max(q_b)≥\tau)H(q_b^',p_m(y|A(u_b)))}$ 
### pseudo-labeling

