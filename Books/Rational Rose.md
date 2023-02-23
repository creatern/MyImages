# Rational Rose

[RationalRose](https://blog.csdn.net/gz153016/article/details/49641847)

[RationalRose简易教学](file:///c:/notebook/Attach/Rational_Rose/Rational%20Rose简明实用教程_gz153016的博客-CSDN博客_rational%20rose.html)

# UML概述

**UML** 可视化的面向对象的统一**建模语言**

- 软件开发过程中的分析设计阶段使用。
- 支持面向对象
- 独立与软件的实现

## 基本元素

![](c:/notebook/pictures/Snipaste_2023-02-23_14-47-34.png =500x)

### 结构事物 

- 构成模型的静态部分，负责描述静态功能和客观元素
- UML中一共定义了7种结构事物：类、接口、用例、构件、节点 等

#### 类

- 完全对应于面向对象分析中的类，具有自己的属性和操作。 


**创建类**

![](c:/notebook/pictures/Snipaste_2023-02-23_15-13-11.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_14-48-28.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_14-50-03.png =500x)

#### 接口

- 由一组对操作的定义组成，不包括对操作的实现进行详细的描述。
- 用于描述一个类或构件的一个服务的操作集。
- 描述了元素的外部可见的操作。

![](c:/notebook/pictures/Snipaste_2023-02-23_14-57-14.png =200x)
![](c:/notebook/pictures/Snipaste_2023-02-23_14-58-48.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_15-01-53.png =500x)

#### 用例

- 用于表示系统所提供的服务
- 定义了系统是如何被参与者使用的，描述的是参与者为了使用系统所提供的某一完整功能而与系统发生的一段对话。
- 用例是对一组动作序列的抽象描述。
- 系统执行这些动作将产生一个对特点的参与者有价值而且可观察的结果。
- 用例可结构化系统中的行为事物，从而可视化的概括系统需求。

![](c:/notebook/pictures/Snipaste_2023-02-23_15-21-22.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_15-22-17.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_15-27-17.png =500x)

#### 构件

**目的**

1. 将一个大系统分解成若干个小软件（子系统），有助于降低系统开发的复杂度。
2. 采用构件机制开发的系统，在系统需要修改是，更为容易。
3. 构件机制有助于提高复用水平，同时也是一种好的销售方法。

![](c:/notebook/pictures/Snipaste_2023-02-23_15-30-11.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_15-30-48.png =500x)

#### 节点

- 是系统在运行时切实存在的物理对象，表示某种可计算资源，这些资源往往具有一定的存储能力和处理能力。
- 一个节点可以代表一个物理机或一个虚拟物理机。

![](c:/notebook/pictures/Snipaste_2023-02-23_15-40-18.png =300x)
![](c:/notebook/pictures/Snipaste_2023-02-23_15-38-40.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_15-39-44.png =400x)
![](c:/notebook/pictures/Snipaste_2023-02-23_15-42-09.png =400x)

### 行为事物

- UML模型中相关的动态行为，通常使用动名词来描述。
- 将行为事物分为两类：
     - 交互
     - 状态机：结构事物状态的变化

#### 交互

- 交互的可视化表示主要通过消息来表示

##### 顺序图

![](c:/notebook/pictures/Snipaste_2023-02-23_15-51-02.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_15-54-57.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_15-56-34.png =500x)

#### 状态机

- 是一个类对象所有的**生命历程**模型，因此状态机可以用于描述一个对象在其生命周期内响应时间所经历的**状态序列**。
- 当对象探测到一个**外部事件**后，它依照当前的状态做出反应，这种反应包括执行一个相关动作或转换到一个新的状态中去。
- 单个类的状态变换或多个类之间的协作过程都可以使用状态机描述。

![](c:/notebook/pictures/Snipaste_2023-02-23_16-33-07.png =400x)
![](c:/notebook/pictures/Snipaste_2023-02-23_16-34-45.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_16-37-09.png =500x)

### 分组事物

- 是UML对模型中的各种组成部分进行事物分组的机制。可以把分组事物当成是一个“盒子“，不同的盒子存放不同的模型。

**包**

- 目前只有一种分组事物：包
- 包是一种在概念上对UML模型中各种组成部分进行分组的机制，只存在于软件的开发阶段
- 通常和类图等一起使用   

![](c:/notebook/pictures/Snipaste_2023-02-23_16-57-10.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_16-53-19.png =300x)
![](c:/notebook/pictures/Snipaste_2023-02-23_16-57-47.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_16-58-16.png =500x)
![](c:/notebook/pictures/Snipaste_2023-02-23_16-58-34.png =300x)
![](c:/notebook/pictures/Snipaste_2023-02-23_16-59-03.png =500x)
![](c:/notebook/pictures/ =500x)