---
layout: post
comments: true
title: MLP、RNN、CNN的区别与联系
excerpt: MLP加上不同的假设，分别得到了CNN和RNN
categories: Work
date: 2018-04-18 22:00:00
---

DNN指的是包含多个隐层的神经网络，如图1所示，根据神经元的特点，可以分为MLP、CNN、RNN等，下文在区分三者的时候，都从神经元的角度来讲解。MLP是最朴素的DNN，CNN是encode了空间相关性的DNN，RNN是encode进了时间相关性的DNN。

<div class="imgcap">
<img src="/assets/2018-04-18-from-mlp-to-cnn-and-rnn-1.png">
<div class="thecap">图1</div>
</div>

MLP是最简单的DNN，它的每一层其实就是fc层（fully connected layer），其神经元见图2所示。

<div class="imgcap">
<img src="/assets/2018-04-18-from-mlp-to-cnn-and-rnn-2.png">
<div class="thecap">图2</div>
</div>

CNN相对于MLP而言，多了一个先验知识，即数据之间存在空间相关性，比如图像，蓝天附近的像素点是白云的概率会大于是水桶的概率。滤波器会扫过整张图像，在扫的过程中，参数共享。图3、图4、图5、图6是一个3x3的输入经过一个2x2的conv的过程，该conv的stride为1，padding为0。图7是最后的计算结果。

<div class="imgcap">
<img src="/assets/2018-04-18-from-mlp-to-cnn-and-rnn-3.png">
<div class="thecap">图3</div>
</div>

<div class="imgcap">
<img src="/assets/2018-04-18-from-mlp-to-cnn-and-rnn-4.png">
<div class="thecap">图4</div>
</div>

<div class="imgcap">
<img src="/assets/2018-04-18-from-mlp-to-cnn-and-rnn-5.png">
<div class="thecap">图5</div>
</div>

<div class="imgcap">
<img src="/assets/2018-04-18-from-mlp-to-cnn-and-rnn-6.png">
<div class="thecap">图6</div>
</div>

<div class="imgcap">
<img src="/assets/2018-04-18-from-mlp-to-cnn-and-rnn-7.png">
<div class="thecap">图7</div>
</div>

RNN相对于MLP而言，也多了一个先验知识，即数据之间存在时间相关性，比如一段文字，前面的字是“上”，后面的字是“学”概率更大，是“狗”的概率很小。RNN神经元的输入会有多个time step，每个time step的输入进入神经元中时会共享参数。图8是一个典型的RNN示意图，图9是将图8在时间上展开的结果，这里只展开了两步。

<div class="imgcap">
<img src="/assets/2018-04-18-from-mlp-to-cnn-and-rnn-8.png">
<div class="thecap">图8</div>
</div>

<div class="imgcap">
<img src="/assets/2018-04-18-from-mlp-to-cnn-and-rnn-9.png">
<div class="thecap">图9</div>
</div>
