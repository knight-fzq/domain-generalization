This is the code for paper(ID:3669) [FedSkip: Combatting Statistical Heterogeneity with Federated Skip Aggregation].

## Dependencies
* PyTorch >= 1.0.0
* torchvision >= 0.2.1
* scikit-learn >= 0.23.1



## Parameters
| Parameter                      | Description                                 |
| ----------------------------- | ---------------------------------------- |
| `skip` | Number of skip between two aggregations. |
| `model`                     | The model architecture. Options: `simple-cnn`, `resnet50` .|
| `dataset`      | Dataset to use. Options: `cifar10`. `cifar100`, `femnist`|
| `lr` | Learning rate. |
| `batch-size` | Batch size. |
| `epochs` | Number of local epochs. |
| `n_parties` | Number of parties. |
| `sample_fraction` | the fraction of parties to be sampled in each round. |
| `comm_round`    | Number of communication rounds. |
| `partition` | The partition approach. Options: `noniid`, `iid`. |
| `beta` | The concentration parameter of the Dirichlet distribution for non-IID partition. |
| `datadir` | The path of the dataset. |
| `logdir` | The path to store the logs. |
| `device` | Specify the device to run the program. |
| `seed` | The initial seed. |  

For Cifar-10 and FEMNIST, you should use simple-cnn while for CIFAR-100, you should use resnet50. You can set beta as large as possible to simulate IID when partition=non-iid. We set lr=0.01, epochs=10 and batch-size=64 by default in the paper.

## Usage
Here is an example to run FedSkip-3 on CIFAR-10 with a simple CNN:
```
python main.py --dataset=cifar10 \
    --model=simple-cnn \
    --skip=3 \
    --lr=0.01 \
    --epochs=10 \
    --comm_round=100 \
    --n_parties=10 \
    --partition=noniid \
    --beta=0.5 \
    --sample_fraction=1.0 \
    --logdir='./logs/' \
    --datadir='./data/' \
```
