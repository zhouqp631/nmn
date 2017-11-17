DeepLab
=====
机器学习、深度学习、量化投资，好用的工具、实用的工具，尽在DeepLab ! 

神经模块网络
---------
### 背景
通常深度学习仅能通过拟合不同样本之间的“差异”识别不同的分类，但神经模块网络能够模拟人类多步推理，每个功能模块都由神经网络拟合，通过将模块的动态组合得到最终的答案。

自从加州伯克利的Jacob Andreas大神提出神经模块网络（Neural module networks，简称nmn）后，深深的被其动态组合、端到端训练特性所吸引，
幸运的是pytorch极好的支持了动态图特性，能够很方便的实现nmn。

### 思路
本文主要参考了李飞飞的Inferring and Executing Programs for Visual Reasoning，该工作是研究VQA视觉问答问题，
需要根据图片中的物体位置关系，进行一定的推理才能回答指定的问题，该工作主要两个步骤：

(1)根据指定的问题获得推理步骤，每个步骤对应一个神经模块

(2)根据推理步骤组合神经模块网络，根据（图片+问题 = 答案）端到端训练

训练完成后每个神经模块获得特点的推理功能，如捕获位置关系

本文通过一个toy问题，聚焦神经模块网络部分，验证nmn的有效性，问题如下：
给出一串数字，如[0, 0.1, 0.2]及其计算步骤（0 + 0.1）- 0.2 是否能够得到最终结果：-0.1 ？
是否能够学习一个神经模块网络，其中每个模块学习到了一个基本的运算步骤，如加法、减法、乘法？

计算过程如下图：

![github](https://github.com/junliangliu/nmn/blob/step.png "思路")

其中，圆圈是计算输入数值，虚线圆圈是中间结果，方框是一个神经网络（一个模块），表示不同的运算如加法、减法、或乘法

### 实验环境及训练数据
环境：pytorch

数据及计算步骤：

数据                     步骤          结果

[0, 0.1, 0.2]            [+, -]       -0.1

[0, 0.3, 0.5，0.6]       [*, +，-]    -0.1

....                     ....         ....

其中计算长度随机指定，每个步骤采用什么运算随机指定。


### 实验结果

![github](https://github.com/junliangliu/nmn/image/result.png "思路")

解释如下：

分布别用学习到加法、减分、乘法模块计算[0.1, 0.3]的加减乘，得到如下结果

        预测值         实际值
        
加法    0.4013        0.4

减法    -0.2025       -0.2

乘法    0.00252       0.003

可以看到通过端到端的训练，nmn中的每个模块独立学到了加法、减法、乘法运算！！

### 代码结构

nmn.py 前半部分用于生成数据，后半部分用于定义及训练nmn


### 参考：
https://github.com/facebookresearch/clevr-iep
https://github.com/jacobandreas/nmn2

