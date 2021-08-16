# meta-pseudo-labeling

## aim

## method
### loss function
1. Pseudo Labels (PL) trains the student model to minimize the cross-entropy loss on unlabeled data.  
![image](picture/meta_pseudo_label_loss1.png)  
2. Given a good teacher, the hope of Pseudo Labels is that the obtained θS PL would ultimately achieve a low loss on labeled data
![image](picture/meta_pseudo_label_loss2.png)  
3. that is, due to unlabeled data, the output of student net and teacher net should be close. And when predict labeled data, the output of student net should be close to ground truth.  
### student network
1. draw a batch of unlabeled data $X_{u}$, then sample $T(X_{u};\theta_{T}) from teacher's prediction, and optimize objective with SGD:  
![image](picture/meta_pseudo_label_formula1.png)  

### teacher network
1. draw a batch of labeled data $(x_{l},y{l})$, and "reuse" the student's update to optimize objective with SGD:  
![image](picture/meta_pseudo_label_formula2.png)  
![image](picture/meta_pseudo_label.png)  
## 














