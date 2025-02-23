# semi-supervised-paper-implementation

This repository is designed to reproduce the methods in some semi-supervised papers.

Before running the code, you need to install the packages according to the following command.

```python
pip3 install torch==1.1.0
pip3 install torchvision==0.3.0
pip3 install tensorflow # we use tensorboard in the project
```



## Prepare datasets

### CIFAR-10

Use the following command to unpack the data and generate labeled data path files. 

```python
python3 -m semi_supervised.core.utils.cifar10
```



## Run on CIFAR-10

To reproduce the result in [Temporal Ensembling for Semi-Supervised Learning](https://arxiv.org/abs/1610.02242), run

```python
CUDA_VISIBLE_DEVICES=0 python3 -m semi_supervised.experiments.temporal_ensembling.cifar10_test
```

To reproduce the result in [Mean teachers are better role models](https://arxiv.org/abs/1703.01780). run

```python
CUDA_VISIBLE_DEVICES=0 python3 -m semi_supervised.experiments.mean_teacher.cifar10_test
```

*Note: This code does not be tested on multiple GPUs, so there is no guarantee that the result is satisfying on multiple GPUs.*



## Results on CIFAR-10

| Number of Labeled Data                                       | 1000      | 2000      | 4000      | All labels |
| ------------------------------------------------------------ | --------- | --------- | --------- | ---------- |
| Pi model (from [SNTG](http://openaccess.thecvf.com/content_cvpr_2018/papers/Luo_Smooth_Neighbors_on_CVPR_2018_paper.pdf)) | 68.35 $\pm$ 1.20     | 82.43 $\pm$ 0.44 | 87.64‬$\pm$ 0.31 | 94.44 $\pm$ 0.10 |       |
| **Pi model (this repository)** | 69.615 $\pm$ 1.3013 | 82.92 $\pm$ 0.532 | 87.925 $\pm$ 0.227 | --- |
| Tempens model (from [SNTG](http://openaccess.thecvf.com/content_cvpr_2018/papers/Luo_Smooth_Neighbors_on_CVPR_2018_paper.pdf)) | 76.69 $\pm$ 1.01 | 84.36 $\pm$ 0.39 | 87.84 $\pm$ 0.24 | 94.4 $\pm$ 0.10 | 
| **Tempens model (this repository)**                              | 78.517 $\pm$ 1.1653 | 84.757 $\pm$ 0.42445 | 88.166 $\pm$ 0.24324 | 94.72 $\pm$ 0.14758  |
| Mean Teacher (from [Mean teachers are better role models](https://arxiv.org/abs/1703.01780)) | 78.45     | 84.27     | 87.69 | 94.06      |
| **Mean Teacher (this repository)**                           | 80.421 $\pm$ 1.0264 | 85.236 $\pm$ 0.655 | 88.435 $\pm$ 0.311     | 94.482 $\pm$ 0.1086   |

We report the mean and standard deviation of 10 runs using different random seeds(1000 - 1009).



## Training strategies in semi-supervised learning

In semi-supervised learning, many papers use common training strategies. This section introduces some strategies I know.

### Learning rate

<a href="https://www.codecogs.com/eqnedit.php?latex=lr&space;=&space;rampup\_value&space;*&space;rampdown\_value&space;*&space;init\_lr" target="_blank"><img src="https://latex.codecogs.com/png.latex?lr&space;=&space;rampup\_value&space;*&space;rampdown\_value&space;*&space;init\_lr" title="lr = rampup\_value * rampdown\_value * init\_lr" /></a>

You can find out how to compute rampup_value and rampdown_value in semi_supervised/core/utils/fun_utils.py.

The curve of the learning rate is shown in the figure below.

<img src="semi_supervised/pics/LearningRate.png" alt="alt text" width="350" height="250">

### Optimizer

Many methods in semi-supervised learning use Adam optimizer with beta1 = 0.9 and beta2 = 0.999. During training, beta1 is dynamically changed.

<a href="https://www.codecogs.com/eqnedit.php?latex=adam\_beta1&space;=&space;rampdown\_value&space;*&space;0.9&space;&plus;&space;(1.0&space;-&space;rampdown\_value)&space;*&space;0.5" target="_blank"><img src="https://latex.codecogs.com/png.latex?adam\_beta1&space;=&space;rampdown\_value&space;*&space;0.9&space;&plus;&space;(1.0&space;-&space;rampdown\_value)&space;*&space;0.5" title="adam\_beta1 = rampdown\_value * 0.9 + (1.0 - rampdown\_value) * 0.5" /></a>

The curve of beta1 is shown in the figure below.

<img src="semi_supervised/pics/Adam1.png" alt="alt text" width="350" height="250">

### Consistency Weight

Some methods use dynamically changed weight to balance supervised loss and unsupervised loss. 

<a href="https://www.codecogs.com/eqnedit.php?latex=weight&space;=&space;init\_weight&space;*&space;rampup\_value" target="_blank"><img src="https://latex.codecogs.com/png.latex?weight&space;=&space;init\_weight&space;*&space;rampup\_value" title="weight = init\_weight * rampup\_value" /></a>

The curve of consistency weight is shown in the figure below.

<img src="semi_supervised/pics/ConsistencyWeight.png" alt="alt text" width="350" height="250">



## TODO list

- [x] Mean Teacher
- [x] Pi Model
- [x] Temporal Ensembling Model
- [ ] VAT
- [ ] More....



## References

1.  [Mean teachers are better role models](https://github.com/CuriousAI/mean-teacher)
2.  [Temporal Ensembling for Semi-Supervised Learning](https://github.com/smlaine2/tempens)
3.  [Good Semi-Supervised Learning that Requires a Bad GAN](https://github.com/kimiyoung/ssl_bad_gan)
4.  [Smooth Neighbors on Teacher Graphs for Semi-supervised Learning](https://github.com/xinmei9322/SNTG)

