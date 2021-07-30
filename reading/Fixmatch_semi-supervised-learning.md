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

### Consistency regularization && pseudo-labeling
##### Consistency regularization
Consistency regularization utilizes unlabeled data by relying on the assumption that model should output similar predictions when fed perturbed versions of the same images.  

$l_{s}=\sum_{b=1}^{µB}{||p_{m}(y|α(u_{b}))-p_{m}(y|α(u_{b}))||_{2}^2}$

##### pseudo-labeling
Encourage prediction of model to be low-entropy on unlabeled data. let $q_{b}^{'}$ be the argmax of the model output of unlabeled data.  
$l_{u}=\frac{1}{μB}\sum_{b=1}^{μB}{1(max(q_b)≥\tau)H(q_{b}^{'},q_{b}))}$

##### In this paper
Design two kinds of loss: supervised loss $l_{s}$ and unsupervised loss %l_{u}%. $l_{s}$ is just the standard cross-entropy loss on weekly augmented labeled examples. While $l_{u}$ means that when the confidence of prediction is quite high, the prediction of week augmentation should be equal to strong augmentation.  

$l_{s}=\frac{1}{B}\sum_{b=1}^{B}{H(p_{b},p_{m}(y|α(x_{b})))}$
$l_{u}=\frac{1}{μB}\sum_{b=1}^{μB}{1(max(q_b)≥\tau)H(q_{b}^{'},p_{m}(y|A(u_{b})))}$
##### Definition of loss




### 

