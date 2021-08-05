# background
### architecture search
techs like reinforcement learning and evolution have been used to discover model structures from data  
### smart augmentation
a network that automatically generates augmented data by merging two or more samples from the same class  
### DeVries and Taylor
simple transformations in the learned feature space to augment data  
### Tran
Bayesian approach to generate data based on the distribution learned from the traning set.  
### generative methods
key different is that this method only generates symbolic transformation operations instead of directly generating samples  
### Proximal Policy Optimization(PPO)

# aim and issues
### issues
1. Common data augmentation methods are designed manually  
2. best augmentation strategies are dataset-specific  
3. architecture search could not get enough good results  
### aim
automatically search for improved data augmentation policies  
# method
### main idea
design a search space where a policy consists of many sub-policies, one of which is randomly chosen for each image in each mini-batch.A sub-policy consists of two operations, each operation being an image processing function such as translation, rotation, or shearing, and the probabilities and magnitudes with which the functions are applied. Author use a search algorithm to find the best policy such that the neural network yields the highest validation acc on a target dataset.  
### search algorithm
1. controller RNN(1 layer LSTM with 100 hidden units):samples a data augmentation policy S, and updated based on validation acc R    
2. neural netwrok with a fixed architecture:use augmentation policy S to train and feed back validation accuracy R  
3. controller training algorithm:Proximal Policy Optimization algorithm  
at each step, the controller predicts a decision produced by a softmax, the prediction is then fed into the next step as an embedding. It has 30 softmax predictions in order to predict 5 sub-policies(5*2(SUB)*3(TYPE,PROB,MAG)). And it is trained with a reward signal, which is how good the policy is in improving the generalization of a child model(validation set to measure the generalization of a child model)  

### search space
1. policy S:consists of 5 sub-policies with each sub-policies consisting of two image operation to be applied in sequence.  
2. operation:each operation is associated with two hyperparameters that is the probability of applying the operation and the magnitude of the operation  
3. operation space:all image transformation functions in PIL(ShearX/Y, TranslateX/Y, Rotate, AutoContrast, Invert, Equalize, Solarize, Posterize, Contrast, Color, Brightness, Sharpness) and two other aug tech:Cutout and SamplePairing.  
4. searching space:16(number of operations)*10(default range of magnitudes)*11(discrete probalibity, uniform spacing), so sub-policy:(16*10*11)^2, policy((16*10*11)^2)^5  
### model structure and applying example
1. model structure  
![image]<picture/autoaug_structure.png>  
2. example  
![image]<picture/autoaug_example.png>  
![image]<picture/autoaug_example2.png>  
# experiment results
SOTA on CIFAR-10,CIFAR-100,SVHN and ImageNet(without additional data)  
### CIFAR-10
98.5%,0.6% higher  
### Image-Net
83.5%,0.4% higher  

# other
### transferable
THe policy learned on ImageNet transfers well to achieve significant improvements on other datasets, such as Oxford Flowers, Caltech-101, Oxford-IIT Pets, FGVC Air-craft, and Stanford Cars.  
### difference to other automated data aug methods
1. Different from GAN, author tries to make sure the augmented images are similar to the current training images(Like in NLP, choose nearest words)  
2. performance  
### training steps
1. requires a certain number of epochs per sub-policy for auto-aug to be effective  
2. child-model use 5 sub-policies, so they need to be trained for more than 80-100 epochs before the model can fully benefit from all of the sub-policies.
