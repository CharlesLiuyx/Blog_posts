---
title: TensorSpace 一个3D神经网络可视化框架
date: 2018-09-28 16:00:20
tags:
- Visualization
- Machine Learning
categories:
- Machine Learning
---

【阅读时间】5min | 2k字
【阅读内容】[TensorSpace](https://tensorspace.org) - 是一款3D模型可视化框架，支持多种模型，帮助你可视化层间输出，更直观的展示模型的输入输出，帮助理解模型结构，输出方法。[Github](https://github.com/tensorspace-team/tensorspace)

<!-- more -->

>  机器学习入门？先上手玩玩看！

# 是什么（What）

`TensorSpace`是一款**3D模型可视化框架**，一动图胜千言。[官网链接](tensorspace.org)，[Github链接](https://github.com/tensorspace-team/tensorspace)

<div align="center"><img src="TensorSpace-3D神经网络可视化框架/TensorSpaceLeNet.gif" alt="" width="1000"></div>

`TensorSpace`擅长直观展示**模型结构**和**层间数据**，生成的模型**可交互**。官方支持[手写字符识别](https://tensorspace.org/html/playground/lenet.html)，[物体识别](https://tensorspace.org/html/playground/mobilenetv1.html)，[`0-9`字符对抗生成网络案例](https://tensorspace.org/html/playground/acgan.html)等

# 为什么（Why）

本部分说明：**❓为什么要使用这个框架❓这个框架主要解决了什么问题❓我们的灵感来源于何处**

## 3D神经网络可视化一片空白

在**机器学习可视化**上，每个**机器学习框架**都有自己的【御用工具】，[Tensorboard](https://github.com/tensorflow/tensorboard)之于`Tensorflow` ，[Visdom](https://github.com/facebookresearch/visdom)之于`Pytorch`，[MXboard](https://github.com/awslabs/mxboard)之于`MXnet`。这些工具的Slogan不一而同的选择了`Visualization Learning`(TensorBoard的Slogan)，也就是**面向专业机器学习开发者**，针对**训练过程，调参**等设计的专业向可视化工具

但面向一般的**计算机工程师**和**非技术类人才（市场，营销，产品）**，一片空白，没有一个优秀的工具来帮助他们理解<span sytle="font-weight:bold; color: red">❓机器学习模型到底做了什么，能解决一个什么问题</span>

机器学习开发和工程使用并不是那么遥不可及，`TensorSpace`搭建桥梁链接**实际问题**和**机器学习模型**

## 3D可视化的信息密度更高更直观

市面上常见的机器学习可视化框架都是**基于图表**（2D），这是由它们的**应用领域**（训练调试）决定的。但3D可视化**不仅能同时表示层间信息，更能直观的展示模型结构**，这一点是2D可视化不具备的。例如在何凯明大神的Mask-RCNN论文中：

<div align="center"><img src="TensorSpace-3D神经网络可视化框架/Show.png" alt="" width="600"></div>

有这么一幅图来**描述模型结构**（很多模型设计类和应用落地类的论文都会有这么一副图）而`TensoSpace`可以让用户使用浏览器方便的构建出一个**可交互的神经网络3D结构**

更进一步的，利用3D模型的表意能力特点，结合[Tensorflow.js](https://js.tensorflow.org/)，可在**浏览器中进行模型预测**（跑已经训练好的模型看输入输出分别是什么❗️），帮助理解模型

## 【模型结构】黑盒子的真面目是什么？

模型就像是一个盛水的容器，而预训练模型就给这个容器装满了水，可以用来解决实际问题。**搞明白一个模型的输入是什么，输出是什么，如何转化成我们可理解的数据结构格式**（比如输出的是一个物体标识框的左上角左下角目标）就可以方便的理解某个模型具体做了什么

例如，❓[Yolo](http://tensorspace.org/html/playground/yolov2-tiny.html)到底是如何算出最后的物体识别框的？❓[LeNet](http://tensorspace.org/html/playground/lenet.html)是如何做手写识别的？❓[ACGAN](http://tensorspace.org/html/playground/acgan.html)是怎么一步一步生成一个`0-9`的图片的？这些都可以在提供的`Playground`中**自行探索**

如下图所示，模型层间的链接信息可通过直接鼠标悬停具体查看

<div align="center"><img src="TensorSpace-3D神经网络可视化框架/ShowStruc.png" alt="" width="1000"></div>

<br>

## 【层间数据】神经网络的每一层都做了什么？

3D模型不仅仅可以直观的展示出神经网络的结构特征（哪些层相连，每一层的数据和计算是从哪里来），结合[Tensorflow.js](https://js.tensorflow.org/)，可在**浏览器中进行模型预测**。由于我们已经有了模型结果，所有的层间数据直观可见，如下图所示

<div align="center"><img src="TensorSpace-3D神经网络可视化框架/InData.png" alt="" width="1000"></div>

在`TensoSpace`内部，调用`Callback Function`可以方便的拿到每一层的输出数据（未经处理），工程和应用上，了解一个模型的原始输出数据方便工程落地

# 怎么建（How）

首先你需要有一个使用常用框架训练好的的**预训练模型**，常见的模型都是只有**输入输出**两个暴露给用户的接口。`TensorSpace`可以全面的**展示层间数据**，不过需要用户将模型转换成**多输出的模型**，过程详见[此文档](https://tensorspace.org/html/docs/preIntro_zh.html)。具体流程如下图所示：

<div align="center"><img src="TensorSpace-3D神经网络可视化框架/Flow.png" alt="" width="900"></div>

用`TensorSpace`构建对应模型这一步，下面一段构建`LetNet`的代码可能更加直观，如果要在本地运行，需要Host本地Http Server

<iframe height='400' scrolling='no' title='TensorSpace LetNet' src='//codepen.io/syt123450/embed/667a7943b0f23727790ca38c93389689/?height=300&theme-id=33158&default-tab=js,resultundefined&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>See the Pen TensorSpace Hello World by syt123450 (@syt123450) on CodePen. </iframe>

你最需要的是**模型结构的相关信息**，`Tensorflow`，`Keras`都有对应的API打印模型结构信息，比如`Keras`的`model.summary()`。还有类似生成结构图的方式，生成如下图的模型结构2D示意图

<div align="center"><img src="TensorSpace-3D神经网络可视化框架/LeNet-Model.png" alt="" width="300"></div>

是的，**你需要对模型结构非常了解**才可能构建出对应的`TensoSpace`模型，未来版本已计划推出自动脚本，通过导入对应的模型预训练文件，**一键生成多输出模型**。但是`TensoSpace`的Playground会未来子项目会力所能及的收集更多的模型，在模型应用落地和直观展示这个领域努力做出自己的贡献

# 谁可能用（Who）

做这样一款开源框架，除了填补3D可视化的一般解决方案的框架空白外，还思索了几个可能可行的**应用场景**

## 前端开发者过度机器学习

> 前端（全栈）开发者，产品经理等

未来，前端的重复性工作可能会慢慢减少。最近有一个[原型图 ➜ HTML代码的项目](https://zhuanlan.zhihu.com/p/48580981)，另一个2017年的[开源项目](https://github.com/tonybeltramelli/pix2code)都在尝试利用机器学习自动化一些Coding中的**重复劳动，提高效率**

机器学习一定**不会**取代前端工程师，但掌握机器学习工具的工程师会有优势（这种工具会不会整合进Sketch等工具不好说），既然入了工程师行，终身学习势在必行！

`TensorSpace`虽然不是能帮忙训练和设计模型，但擅长帮助工程师**理解已有的模型**，找到**可应用的领域**。并且在接驳光法开发者到机器学习的大道上做了一点微小的工作，走一个可视化的`Model Zoo`

## 机器学习教育

> 机器学习课程教育者

使用`Tensorspace`直观的在浏览器上显示模型细节和数据流动方向，讲解常见模型的实现原理，比如ResNet，Yolo等，可以让学生更直观的了解一个**模型的前世今生**，输入什么，输出什么，怎么处理数据等等

我们只是提供了一个框架，每一个模型如果需要直观的展示对数据的处理过程，都**值得3D化**

## 模型演示和传播

> 机器学习开发者

`JavaScript`最大的优势就是可以**在浏览器中运行**，没有烦人的依赖，不需要踩过各种坑。有一个版本不那么落后的浏览器和一台性能还成的电脑就可以完整访问所有内容

如果您的项目对展示**自己的模型可以做什么**，**是怎么做**的有需求，私以为，不应错过`TensorSpace`

用`TensorSpace`**教学模型原理**效果非常好。提供了一个接口去写代码，搞清楚每一个输出代表了什么，是如何转化成最后结果（从输出到最后结果的转换还是需要写`JavaScript`代码去构建模型结构，在这个过程中也能更进一步**理解模型的构造细节**）

现在还没有完成的`Yolov2-tiny`就是因为`JavaScript`的轮子较少（大多数处理轮子都使用Python完成），所有的数据处理都需徒手搭建。时间的力量是强大的，我们搭建一个地基，万丈高楼平地起！

# 致谢

## 机器学习部分

我们最初的灵感来源于一个真正教会我深度卷积网是如何工作的网站：[http://scs.ryerson.ca/~aharley/vis/conv/](http://scs.ryerson.ca/~aharley/vis/conv/)（源码只能下载，我Host了一份在[Github](https://github.com/CharlesLiuyx/3DVis_ConvNN)上）

这个网站的效果，**也是团队未来努力的方向**（大网络上，因为实体过多，性能无法支持。为了解决性能问题，我们优化为：不是一个Pixel一个Pixel的渲染，而是一个特征图一个特征图的处理）

## 前端部分

使用[Tensorflow.js](https://js.tensorflow.org/) [Three.js](https://threejs.org/) [Tween.js](https://github.com/tweenjs/tween.js/) 等框架完成这个项目，感谢前人给的宽阔肩膀让我们有机会去探索更广阔的世界

## 开发团队们

感谢每一个为这个项目付出的伙伴，没有你们每个人，就没有这个开源项目破土而出！团队中负责开发的[syt123450](https://github.com/syt123450)（主力）和[Chenhua Zhu](https://github.com/zchholmes)，设计师[Qi(Nora)](https://github.com/lq3297401)，感谢他们！还有负责机器学习模型部分和文档的[我](https://github.com/CharlesLiuyx)

也欢迎你有什么想法给我留言，或直接在[Github](https://github.com/tensorspace-team/tensorspace/issues)上提出Pull Request