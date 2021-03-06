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


