
---

### 2018-12-12 ~ 2018-12-18

### **论文阅读**

**题目**：ADVERSARIAL LEARNING FOR SEMI-SUPERVISED SEMANTIC SEGMENTATION
**作者**：Wei-Chih Hung, Yi-Hsuan Tsai, Yan-Ting Liou, Yen-Yu Lin, Ming-Hsuan Yang
**机构**: 美国加州大学默塞德分校、美国NEC实验室、国立台湾大学、中国科学院，台湾
**简介** ：采用对抗训练的思想进行半监督语义分割，重新设计鉴别器,与一般的弱监督方法不同，除了像素级标签数据外，另外的数据不带有任何标签
**技术要点** ：
![cycle](https://github.com/shenhongqian/GAN/blob/master/paper/img/semiGAN/1.png)

  分割网络：输出类概率图（H*W*3），可以是任何形式的分割网络
  鉴别网络：输出空间概率置信度图(H*W*1)，
          置信度图指示了分割的局部质量，
          使得分割网络在训练期间信任哪些区域
          其中像素点p代表这个来自gournd truth(p=1)
          还是generator(p=0)
  损失函数：input: Xn
  segmentation network: S(.)
  predicted probability map :S(Xn)
  ground truth: Yn
  训练过程:
  鉴别器只使用带标签数据
  先进行全监督训练，迭代5000次，然后进行半监督训练
  半监督训练时，随机迭代带标记数据和无标记数据

**代码链接**：[semiGAN](https://github.com/hfslyc/AdvSemiSeg)

**数据集**：PASCAL VOC 2012 数据集（21分类），是关于语义分割的数据集，[下载地址](http://host.robots.ox.ac.uk/pascal/VOC/voc2012/index.html)
            Cityscapes  数据集（19分类）
**原文实验效果**：

![cycle](https://github.com/shenhongqian/GAN/blob/master/paper/img/semiGAN/2.png)

![cycle](https://github.com/shenhongqian/GAN/blob/master/paper/img/semiGAN/3.png)

![cycle](https://github.com/shenhongqian/GAN/blob/master/paper/img/semiGAN/4.png)

![cycle](https://github.com/shenhongqian/GAN/blob/master/paper/img/semiGAN/5.png)

![cycle](https://github.com/shenhongqian/GAN/blob/master/paper/img/semiGAN/6.png)



**感想与思路**： 在gan的基础上进行改进，优点是把原来只判断图像是真实的还是虚假的，改成了图像像素点的分割置信度图，有一定的实验效果，但是看原文给的置信度图，觉得还没有起到真正的作用
---
### 2018-11-28 ~ 2018-12-05
### **论文阅读**
**题目**：Unpaired Brain MR-to-CT Synthesis using a Structure-Constrained CycleGAN

**作者**：Heran Yang，Jian Sun，Aaron Carass，Can Zhao，

**机构**: 西安交大&约翰霍普金斯

**简介** 使用CycleGAN完成由脑部MR到CT的转换，在CycleGAN中加入结构性约束
**技术要点** ：加入的结构性约束为MIND(Modality independent neighbourhood descriptor for multi-modal deformable registration （2012 Medical Image Analysis）)
主要是说利用图像patch计算某一个图像块的特征，得到的这个特征和模态无关，最早用来解决医学图像不同模态之间的配准问题。
计算方法： 
![cycle](https://github.com/shenhongqian/GAN/blob/master/paper/img/StructureConstrainedCycleGAN/0.png)
损失函数：
![cycle](https://github.com/shenhongqian/GAN/blob/master/paper/img/StructureConstrainedCycleGAN/1.png)
**代码链接**：无

**数据集**： 共45例患者的脑部MR和CT（每个MR或CT病例大约有270张切面图像）
27个病例用于训练集
3个病例用于验证集
15个病例用于测试集



**原文实验效果**：
![cycle](https://github.com/shenhongqian/GAN/blob/master/paper/img/StructureConstrainedCycleGAN/2.png)

**感想与思路**：
找到cycleGAN的两个转化方的一个共同的特征进行约束

### 2018-09-17 ~ 2018-09-27
###  **实验** 

**实验意图与方向**：甲状腺分割

**实验来源**：实现论文[Semantic Segmentation using Adversarial Networks](https://arxiv.org/abs/1611.08408v1)的方法；；
	
**实验结果**：
|实验|交并比(%)|
|:-|:-|
|FCN|78.12|
|GAN-1|78.19|
|GAN-2|77.44|
|GAN-3|78.45|

1.  FCN 为基础FCN-8s，由224px下采样5次,然后上采样到56px,进行双线性插值到224px.
2.  GAN-1为原始GAN
3.  GAN-2 为调节生成器和判别器的训练不属迭代，k=3
4. GAN-3 为改变判别器的输入，原始图像+掩码/预测

**存在问题**：

1. 训练过程判别分支的正确率在0.5上下摆动，在生成网络中起到的作用可能不是很大
2. 实验效果并没有向论文中说的效果，反而GAN生成的交并比小于0.4的图片与FCN的效果差距有些大，预测出来的结节部分偏小，而且不连续

---
### 2018-09-10 ~ 2018-09-17

### **论文阅读**
**题目**：Semantic Segmentation using Adversarial Networks

**作者**：Pauline Luc，Camille Couprie，Soumith Chintala，Jakob Verbeek

**机构**: Facebook AI Research

**简介** ：属于语义分割大类、发表于NIPS 2016年、第一次应用GAN做语义分割，思路新颖
**技术要点** ：在生成对抗网络原有的基础上，对生成器和判别器两个分支进行修改，以适应语义分割应用。
               生成器使用正常的分割网络即可，判别器对生成的map和原有真实图片进行判断，
			   在生成器中应该对生成的map图像标真标签，这样才能起到博弈作用

**代码链接**：[seGAN](https://github.com/oyam/Semantic-Segmentation-using-Adversarial-Networks)

**数据集**：PASCAL VOC 2012 数据集，是关于语义分割的数据集，[下载地址](http://host.robots.ox.ac.uk/pascal/VOC/voc2012/index.html)

**原文实验效果**：使用生成对抗网络进行语义分割，据作者观点，大物体的空间连续性更强，小物体的边缘特征更加清晰，平均交并比比不使用GAN的实验高约1%

**感想与思路**：本篇文章的思路很新颖，感觉本篇文章的效果可以替代CRF后处理，但是由于GAN本身存在硬伤，不好训练，效果不一定



###  **实验** 

**实验意图与方向**：甲状腺分割

**实验来源**：实现论文[Semantic Segmentation using Adversarial Networks](https://arxiv.org/abs/1611.08408v1)的方法；；
	

**实验基础**：

- FCN 在甲状腺的数据集上做分割交并比79%
  目前使用GAN方法做的效果和单纯FCN的方法没有什么区别，没有提高


  [1]: https://github.com/shenhongqian/GAN/blob/master/paper/img/StructureConstrainedCycleGAN/0.png
