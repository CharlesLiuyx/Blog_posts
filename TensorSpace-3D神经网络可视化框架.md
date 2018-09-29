---
title: TensorSpace-3D神经网络可视化框架
date: 2018-09-28 16:00:20
tags:
- Visualization
- Machine Learning
categories:
- Machine Learning
---

【阅读时间】文档介绍类文章
【阅读内容】TensorSpace - 是一款3D模型可视化框架，支持多种模型，帮助你可视化层间输出，更直观的展示模型的输入输出，帮助理解模型结构，输出方法

<!-- more -->

# Conv2d ![#FFFF2E](https://placehold.it/15/FFFF2E/000000?text=+) 

## 输出和使用
```JavaScript
TSP.layers.Conv2d({...参数列表})  
```

### 调用结果

- 创建一个新的对象，往画布内添加一个新的**3D可视化卷积层**
- 默认颜色: #FFFF2E  ![#FFFF2E](https://placehold.it/15/FFFF2E/000000?text=+) 

<div align="left"><img src="https://github.com/zchholmes/tsp_image/blob/master/Document/Conv2D.png" alt="" width="700"></div>

### 参数列表

#### ⭐️ 必要参数

> 使用时必须提供，不能为空。只要提供了这些参数，就能正常创建

- `filters` : `Int` 特征提取过滤器的数量

#### 🔨 推荐参数

> 使用时推荐给定，未给定可以运行，但是在体验和易用性上有隐患

- `name` : `string` 层的命名
- `padding` : `string` 是否使用padding
  - `valid`(default) 使用padding 
  - `same` 不使用padding 

#### 🎨 外观参数

> 可覆盖`TSP.model.Sequential`下的属性进行细调，[详情点击]()

- `color` : `color format` 层的颜色，默认为红色

#### 🎦 动画控制参数

> 可覆盖`TSP.model.Sequential`下的属性进行细调，[详情点击]()

- `initStatus` : `string` 初始状态层是否收缩
  - `close`(default) : 收缩
  - `open` : 展开
- `color` : `color format 0xffffff` 选择当前层实例的颜色
- `animationTimeRatio` : `Int` 张开和伸缩的调整速度，呈倍速关系，例如2就是2倍

#### ⚙️可选参数
> 使用时作为辅助调整参数，根据具体情况**选择性添加**
> 这里的参数对于层的结构（3D可视化形态）没有影响

- `shape` : `[Int]` 
    - 有此参数，会覆盖除了`filters`的其他参数的影响
    - 例如，另 `shape = [ 6, 28, 28 ]` 表示输出为6个特征图，每幅图大小`28*28` 
- `kernelSize` : `Int` 卷积核的尺寸
- `strides` : `Int` 卷积移动框的移动步长

### 什么时候用

如果你是`Keras` | `TensorFlow` | `tfjs` 框架的使用者，构建模型时使用了**卷积层**`Conv2D`（下表列出了可能得使用情景）。一一对应的，在`TensorSpace`中，你应该使用此API

| 框架名称 | 框架中对应层的语法 | 对应框架的代码段 |
| :---: | :---: | :---: |
| [Keras](https://keras.io/layers/convolutional/) | `keras.layers.Conv2D(filters, kernel_size, strides=(1, 1))` | `model.add(Conv2D(32, (3, 3)))` |
| [TensorFlow](https://www.tensorflow.org/api_docs/python/tf/nn/conv2d) | `tf.nn.conv2d(input, filter, strides, padding)` | `x = tf.nn.conv2d(x, W, strides=[1, strides, strides, 1], padding='SAME')` |
| [TensorFlowjs](https://js.tensorflow.org/api/0.13.0/#layers.conv2d) | `tf.layers.conv2d (filters, inputShape)` | `model.add(tf.layers.conv2d({kernelSize: 3, filters: 32, activation: 'relu'}));` |

## 方法

### [.openLayer()]() : void

打开此卷积层

### [.closeLayer()]() : void

关闭此卷积层

## 使用样例

### 添加层

（1）声明一个`Conv2D`的实例，方便复用

```javascript
let convLayer = new TSP.layers.Conv2d({
    kernelSize: 5,
    filters: 6,
    strides: 1,
    animationTimeRatio: 2,
    name: "conv2d1",
    opacityRatio: 2
    initStatus: "open"
});
model.add(convLayer);
```

（2）直接添加`Conv2D`

```javascript
model.add(new TSP.layers.Conv2d({
    kernelSize: 5,
    filters: 16,
    strides: 1,
    name: "conv2d2"
}));
```

### 源码

[tensorspace/src/layer/intemediate/Conv2d.js](https://github.com/syt123450/tensorspace/blob/master/src/layer/intemediate/Conv2d.js)