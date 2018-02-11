# 编写属于你自己的Tensorflow Optimizer

****
    
|Author|luochuwei
|---|---
|E-mail|luochuwei@gmail.com

****
## 目录
* [编写属于你自己的Tensorflow Optimizer](#编写属于你自己的Tensorflow Optimizer)
* [例子：AMSGrad](#例子：AMSGrad)
* [相关资料](#相关资料)
****

### 编写属于你自己的Tensorflow Optimizer
Tensorflow更新很快，也拥有很多集成好了的各种Network/Loss/Optimizer的API供我们调用，我们也可以很容易地通过Tensorflow实现自己的网络和loss。那么当我们灵光一现，有了自己的optimizer算法的时候，应该如何使用tensorflow实现呢？
关于这个问题，目前为止网上基本没有中文的教程，tensorflow的官网上关于定义我们自己的optimizer的资料也基本没有(可能是大家觉得SGD/Adam就够用了吧:disappointed_relieved:)。
这里，一个非常直观的方法就是我们先通过tensorflow定义一个普通的GradientDescentOptimizer，然后通过compute_gradients计算出相应的梯度，然后对梯度进行操作，从而完成我们的想法。但是这种方式如何实现类似于Adam这种带momentum的Optimizer呢？
通过搜索Stackoverflow以及阅读tensorflow相关的Optimizer源代码，发现想要实现自己的Optimizer其实很简单。
我们通过定义依赖于原始tensorflow Optimizer类的一个class，然后实现相应的 _create_slots(), _prepare(), _apply_dense(), 和 _apply_sparse()4个函数，就能够实现我们自己的优化器了.
首先，我们需要以下这些modules:
```
from tensorflow.python.eager import context
from tensorflow.python.framework import ops
from tensorflow.python.ops import control_flow_ops
from tensorflow.python.ops import math_ops
from tensorflow.python.ops import resource_variable_ops
from tensorflow.python.ops import state_ops
from tensorflow.python.ops import variable_scope
from tensorflow.python.training import optimizer
```

然后，我们通过 _create_slots() 和 _prepare() 来创建和初始化关于Optimizer的变量，比如说momentum.

接着，我们通过_apply_dense() 和 _apply_sparse() 来实现相应的更新参数的操作，就大功告成了。

### 例子：AMSGrad
强如Adam，也有不足之处。前段时间公布的ICLR2018排名第五的论文《ON THE CONVERGENCE OF ADAM AND BEYOND》，指出了现有的Adam的不足（Adam可能导致不收敛），提出了AMSGrad，通过对Adam的二阶动量的变化进行控制，取得了不错的效果。乘Tensorflow还没有将其变成内置的API，我们赶快动手，自己实现一下～
### 相关资料
1. [Stackoverflow-How to create an optimizer in Tensorflow](https://stackoverflow.com/questions/38431054/how-to-create-an-optimizer-in-tensorflow)
2. [Custom Optimizer in Tensorflow](https://www.bigdatarepublic.nl/custom-optimizer-in-tensorflow/)
3. [Tensorflow Optimizer](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/training/optimizer.py)
4. [Tensorflow Adam Optimizer](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/training/adam.py)
5. [ON THE CONVERGENCE OF ADAM AND BEYOND](https://openreview.net/pdf?id=ryQu7f-RZ)
6. [ICLR2018 top 100论文列表](http://search.iclr2018.smerity.com)

```
部分资料取自互联网，如有侵权，请联系我，立即删除.
Some references are taken from Internet. If any infringement caused, please advise me to remove immediately.
```
