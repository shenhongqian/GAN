### **GAN系列论文阅读**
**CGAN**：[Wasserstein GAN](https://arxiv.org/abs/1701.07875v2)

**作者**：Martin Arjovsky（第一作者，单位Courant Institute of Mathematical Sciences，Facebook AI Research）

**解决问题** ：解决传统GAN的训练难和训练过程不稳定的过程，主要从损失函数的角度进行了改进。

**基础原理** ：属于理论推导论文， 损失函数改进后即使使用全连接层也能表现出较好的结果。

 （1）从理论上给出了GAN训练不稳定的原因，即交叉熵（JS散度）不适合衡量具有不相交部分的分布之间的距离，转而使用wassertein距离去衡量生成数据分布和真实数据分布之间的距离，理论上解决了训练不稳定的问题。
 
 （2）解决了模式崩溃的（collapse mode）问题，生成结果多样性更丰富。 
 
 （3）对GAN的训练提供了一个指标，此指标数值越小，表示GAN训练的越差，反之越好。 
 
**改进点**： 

（1）生成器和判别器的loss不取log 

(2) 对更新后的权重强制截断到一定范围内，比如[-0.01，0.01]，以满足论文中提到的lipschitz连续性条件。。



