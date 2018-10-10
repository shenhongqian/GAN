### **GAN系列论文阅读**
**GAN开山鼻祖**：[Generative Adversarial Networks](https://arxiv.org/abs/1406.2661)

**作者**：Ian J. Goodfellow（第一作者，单位蒙特利尔大学）

**想法来源** ：最初想法是如何让机器自动生成逼真的图片，如果是使用统计学的方法，那么需要考虑的统计量太大，无法实现，因此他想到使用神经网络来解决这个问题。

**基础原理** ：GAN网络分为两个分支进行博弈，两个分支分别为Generatotor(生成器)和Discriminator（判别器），具体见下表。博弈的结果使生成器生成的图片让判别器真假难以辨别。本质问题是生成模型通过观测数据学习样本和标签的联合概率分布P（X,Y），训练好的模型能够生成符合样本分布的新数据。文章在相关工作中介绍以前工作中的生成模型有变分自编码器和自回归模型（现在这两种方法还不太清楚，有待补充）。

![训练过程](./pic/2.png)

  
 
 - 模型参数定义

  data→真实数据（groundtruth）
  
  pdata→真实数据的分布
  
  z→噪音（输入数据）
  
  pz→原始噪音的分布
  
  pg→经过生成器后的数据分布
  
  G()→生成映射函数
  
  D()→判别映射函数
  
 - 目标函数
 
   min<sub>G</sub> max<sub>D</sub> V(D,G)=E<sub>x~pdata(x)</sub>[log D(x)]+E<sub>z~pz(z)</sub>[log (1-D(G(z)))]

   体现最大最小博弈问题
   
   E代表期望，等号右侧前半部分表示真实样本，后半部分表示虚假样本。
   
   对于判别器来说，希望最大等号右侧，对于生成器来说，希望最小等号右侧。

- 生成器和判别器两者交替训练过程

![训练过程](./pic/1.png)

黑色曲线是真实样本的概率分布函数，绿色曲线是虚假样本的概率分布函数，蓝色曲线是判别器的输出，值越大表明判别真实样本的概率越大。
从图中可以看出，训练开始时，判别器和生成器的能力都比较弱，因此蓝色曲线波动较大，随着生成器生的图片越来越逼真，最后判别器对真实图片判别的概率为0.5。

**GAN存在的优势和缺点**
 - 优势
 1. 相比于其他模型，产生的图片更加逼真
 2. 不需要设计任何数学上的模型，可自主学习
 3. 不需要利用马尔可夫链反复采样，学习过程无需推断，回避了近似计算棘手的概率的难题。
 - 缺点
1. 解决不收敛（non-convergence）的问题。
当博弈双方都由神经网络表示时，在没有实际达到均衡的情况下，让它们永远保持对自己策略的调整是可能的。

2. 难以训练：崩溃问题（collapse problem）  
GAN模型被定义为极小极大问题，没有损失函数，在训练过程中很难区分是否正在取得进展。GAN的学习过程可能发生崩溃问题（collapse problem），生成器开始退化，总是生成同样的样本点，无法继续学习。当生成模型崩溃时，判别模型也会对相似的样本点指向相似的方向，训练无法继续。

3. 无需预先建模，模型过于自由不可控。  
与其他生成式模型相比，GAN这种竞争的方式不再要求一个假设的数据分布，而是使用一种分布直接进行采样，从而真正达到理论上可以完全逼近真实数据，这也是GAN最大的优势。然而，这种不需要预先建模的方法缺点是太过自由了，对于较大的图片，较多的 pixel的情形，基于简单 GAN 的方式就不太可控了(超高维)。


