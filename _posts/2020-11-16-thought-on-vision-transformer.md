---
layout: post
comments: true
title:  关于Vision Transformer的一些思考
excerpt: Self-attention与CNN的恩怨情仇
categories: Work
date: 2020-11-16 14:00:00
---

##### 自相矛盾

[Vision Transformer](https://arxiv.org/abs/2010.11929)这篇文章最大的贡献点在于abstract里讲的“We show that this reliance on CNNs is not necessary and a pure transformer applied directly to sequences of image patches can perform very well on image classification tasks”。但是文章里提出的方法中会将图片分成多个无overlap的patch，每个patch通过linear projection映射为patch embedding，这个过程其实就是卷积，跟文章里声称的不依赖CNNs自相矛盾。

##### Inductive Bias 
文章conclusion里的“Unlike prior works using self-attention in computer vision, we do not introduce any image-specific inductive biases into the architecture”和introduction里的“We find that large scale training trumps inductive bias”，给人一种inductive bias在大量数据的情况是一个不好的东西，应该抛弃的意思。

inductive bias可以理解为assumptions about the nature of the relation
between examples and labels。如果这些assumptions跟实际数据分布的确是相符的，那么无论我们已有的数据量是大还是小，我们利用这个inductive bias应该是一个更好的选择。

inductive bias是人类通过学习、实践而归纳总结出来的knowledge，我们设计一个机器学习/深度学习模型，其实也是让模型通过训练集去学习knowledge。knowledge的好坏应该由knowledge与实际数据分布的匹配程度来衡量，而不是knowledge的来源来衡量。

反过来想，如果后面有研究者想在尽量减少inductive bias的路上一路狂奔，也许可以直接用全连接层替换self-attention。然后我们绕了个大圈又回到了几十年前的MLP：CNN和RNN的提出本来是将领域的知识作为inductive bias，融入到MLP之中，以加速深度学习在CV和NLP等特定领域的落地，我们现在数据量大了，计算资源强了，钱包鼓了，桌子一掀——要什么领域知识，让机器自己学去。

##### Self-Attention在CV领域的展望
之前在[CNN与GCN的区别、联系及融合](https://zhuanlan.zhihu.com/p/147654689)这篇文章中从数学的层面对比了CNN和self-attention的异同。CNN中每个滑动窗中每个特征的空间变换的参数是独立的，特征与特征之间的相关性是通过空间变换的参数隐式建模的；而self-attention中每个滑动窗（可以理解为滑动窗大小为整张feature map，即全局）中每个特征变换的参数是共享的，特征与特征之间的相关性是显示通过某种度量函数（比如点积）来建模的。

另外，对比一下非全局的CNN和self-attention，会发现前者参数量会更多一些，而计算量会更小一些。

总体来看，在CV领域，二者各有优劣，我认为self-attention在CV领域的未来应该是和CNN互相取长补短，你中有我，我中有你，结合起来构建更强大的模型。

##### 科技快速发展下的个人
科学革命和工业革命之后。整个世界的科技发展日新月异，发展速度越来越快，计算机视觉领域遵循同样的规律，不断有新的理论或者技术涌现，两三周不关注学术界，感觉就要被行业抛弃了。

一个领域发展越快，越有利于年轻人，因为经验可能随时被颠覆，相对不太利于年纪渐长的人，但是每个人都会老去。在知识迭代快的行业里，经验的作用会淡化，更重要的是学习能力，这样面对一个又一个新的理论或者技术出现的时候，可以立马跟上。

但是随着年纪的增长，生理方面的能力必然会下降，比如记忆力、反应速度等等，也就是从生理层面来看，随着年纪的增长，学习能力会下降。这是一个必然而又悲哀的事实。

然后就没解了吗？我觉得有一个缓解方案——积累如何学习的经验，类似机器学习里的meta learning。年纪大的人相比年轻人可以形成的优势主要是可以积累的东西，比如财富、不容易被时代抛弃的经验。关于过去理论或者技术的经验在这个时代容易被时代抛弃，而如何学习的经验相对而言会比较保值。
