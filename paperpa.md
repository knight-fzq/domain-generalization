# 异构联邦学习中的正则项

## 论文信息

- [MLSys 2020] FEDERATED OPTIMIZATION IN HETEROGENEOUS NETWORKS
- [Asilomar Conference on Signals, Systems, and Computers 2019] FedDANE: A Federated Newton-Type Method
- [ICLR 2021] FEDERATED LEARNING BASED ON DYNAMIC REGULARIZATION

## 背景介绍

联邦学习（FL, Federated Learning）[1] 于2017年被B. McMahan等人提出，用于实现不同的客户端（Client）在不分享原始私有数据等前提下，与服务器端（Server）交互，最终共同训练得到一个共享的全局模型。联邦学习一方面可以有效降低隐私风险，另一方面可以充分利用边缘设备的计算资源，具有很实际的现实意义。然而，与中心化训练不同，联邦学习的训练可能需要考虑以下限制：数据异构性、通讯开销、系统异构性、隐私与安全等。

本文围绕数据异构性展开。数据异构主要是指在联邦学习场景下，不同客户端的数据分布不一致。这将导致不同客户端目标函数的最优解的不一致，最终造成全局模型的收敛点与全局最优点差异较大甚至不收敛。针对该问题已有若干工作，其中一类比较具有代表性的工作均采用了增加**正则项**的思路。

联邦学习的全局目标为最小化下式：

$$
    \bbox[white, 3px]{
    \mathop{\min}_{w}f(w)=\sum_{k=1}^N p_kF_k(w)
    }
$$

其中 $N$ 为客户端的总数量，$F_k(w)$为第k个客户端的本地经验损失。$p_k=\frac{n_k}{n}$，$n_k$ 为第k个客户端的数据集的大小，$\sum_{k=1}^N n_k=n$。

以上所说的**正则项**则是在 $F_k(w)$ 中的原本的损失函数的基础上增加。

## 1. FedProx

已有实验表明，最开始提出的联邦学习算法FedAvg在non-iid数据分布情形下无法收敛。且FedAvg在该实际场景下难以进行理论分析。该论文通过在本地损失函数中增加一项proximal term，进一步提供收敛保证，并分析异构性的影响。

### Method

FedProx 算法与 FedAvg 算法的区别主要在于本地端的优化目标由此前的 $\mathop{\min}_{w}F_k(w)$ 转变为：
$$
    \bbox[white, 3px]{
    \mathop{\min}_{w}h_k(w;w^t)=F_k(w)+ \frac{\mu}{2}||w-w^t||^2
    }
$$

即在本地端训练模型时，一方面进行正常的本地端训练，搜寻最优解；另一方面限制本地模型与初始全局模型的差异，进而缩小本地端模型间的差异。

该算法主要有以下两个优点：

- 通过限制本地更新与初始全局模型的差异缓解由数据异构造成的客户端模型差异过大的问题，而无需通过手动约束的方式（如限制本地轮次一致）；
- 它能够允许由系统异构造成的本地更新量不一致的问题。

![](https://notes.sjtu.edu.cn/uploads/upload_6d6a335fea649945409fa0d0ba49bf53.png)

### Theoretical Analysis
以下分析为 non-convex 的情形。

![](https://notes.sjtu.edu.cn/uploads/upload_33b1bf2d53080f0237ccc04578f44c93.png)

在Assuption 1下，以下分析给出了每进行一步 FedProx 算法目标函数值的下降量，进而说明了 FedProx 算法的收敛性。

![](https://notes.sjtu.edu.cn/uploads/upload_b59779e3c05208a76cd1508da8696e4b.png)


## 2. FedDANE

该论文主要基于两部分前期工作，一是以上提到的FedProx算法，二是经典分布式优化问题中的DANE方法。

### Method

FedDANE 算法在本地端的优化目标中增添两项：一是如 FedProx 的 proximal 项，二是梯度修正项。其与 FedAvg 算法的区别主要在于本地端的优化目标由此前的 $w_k^t=\mathop{\arg \min}_{w}F_k(w)$ 转变为：
$$
    \bbox[white, 3px]{
    w_k^t=\mathop{\arg\min}_{w}F_k(w) + \langle \nabla f(w^{t-1})- \nabla F_k(w^{t-1}){,} w - w^{t-1} \rangle + \frac{\mu}{2}||w-w^{t-1}||^2
    }
$$

其中，梯度修正项利用上一轮计算得到的本地与全局梯度的差异（即 $\nabla f(w^{t-1})- \nabla F_k(w^{t-1})$ ）限制该轮更新的方向。在上式中，$\nabla f(w^{t-1})$ 的计算需要服务器端与所有的客户端进行通信，再聚合各个来自于本地端的梯度。这在大型联邦学习网络的设定下是不切实际的，因此在该算法中利用采样出来的客户端来估计该值，具体的算法如下。

![](https://notes.sjtu.edu.cn/uploads/upload_17a95d2934a44ac1375d5c8c6206c6ff.png)

但该算法有一个明显的缺点，由于需要计算 $g_t$，且其计算需要增加服务器端与客户端之间的一次通信，从通信开销的角度，相较于 FedAvg 与 FedProx 资源损耗较大。

### Theoretical Analysis
首先作出以下两条定义：

![](https://notes.sjtu.edu.cn/uploads/upload_90eb95dfb6a487276ec5e19ed3546e75.png)

![](https://notes.sjtu.edu.cn/uploads/upload_aba5bab0cb50487617b566991b53e256.png)

进而得到在凸与非凸情形下的收敛性分析，以下列出凸情形下的定理。

![](https://notes.sjtu.edu.cn/uploads/upload_5907631e4abb706f3003d2ab286d0d53.png)

## 3. FedDyn

该论文的主要的切入点为通信开销，从理论上证明了其在凸情形下实现了SOTA的收敛速度，从实验上也能明显看出在数据异构的情形下该方法在收敛速度上的优越性。该论文的'dynamic'主要是指在各客户端的本地目标随着轮次的进行动态调整，以实现本地最优点与全局最优点渐进一致。

### Method

FedDyn 算法在本地端的优化目标中增添两项：一是如 FedProx 的 proximal 项，二是动态调整项（梯度修正）。其与 FedAvg 算法的区别主要在于本地端的优化目标由此前的 $\theta_k^t=\mathop{\arg \min}_{\theta}L_k(\theta)$ 转变为：
$$
    \bbox[white, 3px]{
    \theta_k^t=\mathop{\arg\min}_{\theta}[R_k(\theta;\theta_k^{t-1};\theta^{t-1})=L_k(\theta) - \langle \nabla L_k(\theta_k^{t-1}) {,} \theta \rangle + \frac{\alpha}{2}||\theta-\theta^{t-1}||^2]
    }
$$

其中，在客户端本地目标优化后，进行 $\nabla L_k(\theta_k^{t-1})$ 的更新，更新方式如下：
$$
    \bbox[white, 3px]{
    \nabla L_k(\theta_k^{t})=\nabla L_k(\theta_k^{t-1})-\alpha(\theta_k^t-\theta^{t-1})
    }
$$

以下为 FedDyn 的算法。

![](https://notes.sjtu.edu.cn/uploads/upload_0a385a9b7cb7bb17ee5c210b38fdf1a7.png)

在该算法下，假如初始化在某一一致点，即 $\theta_k^{t-1}=\theta^{t-1}$，则对于$\theta=\theta^{t-1}$，$\nabla R_k(\theta;\theta_k^{t-1};\theta^{t-1})=0$。从这个角度而言，某种程度上可以认为本地的驻点与全局模型的驻点一致。

但该算法中，客户端需要存储 $\nabla L_k(\theta_k^{t-1})$ 的状态，需要多余的内存，可能不适用于所有的联邦环境。

### Theoretical Analysis

该文章分别给出了strongly convex，convex以及non-convex情形下的收敛性分析。

![](https://notes.sjtu.edu.cn/uploads/upload_d065a9d9d68f414dd2b10d1f9b4eb4cf.png)

- Strongly convex：期望以线性速度收敛；
- Convex：期望收敛速度为$O(\frac{1}{T}\sqrt{\frac{m}{P}})$；
- Non-convex：期望收敛速度为$O(\frac{1}{T}\frac{m}{P})$。

## 总结

数据异构是联邦学习中一个常见的问题，会很大程度上影响全局模型的整体效果，近年来有若干工作尝试缓解该问题，其中增添正则项是一个较为常见的范式。本文从 FedProx 出发，引入 Proximal 正则项。随后介绍的 FedDANE 与 FedDyn 算法均沿用了该正则项，并增添了与梯度相关的约束项（增加了资源的消耗）。总之，正则项在联邦学习中的合理性已经被若干工作的实验与理论分析所验证，之后可能也将被继续沿用。


[1] Communication-Efficient Learning of Deep Networks from Decentralized Data.

