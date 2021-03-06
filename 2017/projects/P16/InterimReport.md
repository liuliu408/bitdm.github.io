---
layout: page
mathjax: true
permalink: /2017/projects/p16/midterm/
---

## 短期降水量预测

### 一、小组成员及分工

- 1 李盛楠：2120161009 算法设计、数据分析、文档编写
- 2 洪辉婷：2120160997 算法设计、数据处理、文档编写与PPT制作
- 3 江明明：2120161000 算法设计、程序实现

### 二、问题描述

本项目提供不同时间段的雷达地图——主要包含目标站点和周边覆盖地区的雷达反射率信息——用以预测每个目标站点未来1小时至2小时之间的地面总降水量。该项目主要涉及以下信息分析任务：

- 1.	当前降水量与雷达地图之间的关系。针对雷达地图的分析可以获得当前降水量的相关线索；
- 2.	利用雷达地图中的当前目标站点及其周边地区的雷达反射率信息，可以分析目标地点与周边地区之间的降水关系；
- 3.	根据历史上不同时间段的雷达地图数据，挖掘发现降雨量演变的模式。

### 三、数据集描述
本数据集由深圳气象局提供。数据集包含真实的雷达地图信息以及不同地点的降水量信息。数据集包含训练集及测试集：
1)	训练集：包含10000个数据实例
2)	测试集：包含2000个数据实例

数据集格式为“id，label，radar_map”，详细介绍如下：

- id：位于雷达地图中心的目标地点；
- label：雷达地图中目标地点在未来1小时\~2小时之间的降水量标注（注意，若当前时间为12:00，则未来1小时\~2小时之间为13:00\~14:00之间，即，不考虑12:00\~13:00之间的降水量）；
- radar_map：15个时间段（间隔6分钟）在4个不同高度（0.5\~3.5km，间隔1km）测量的雷达地图（如图1），每个雷达地图占地面积![](https://github.com/xhhszc/image-for-DM/raw/master/function-2.png)，目标位置位于中心，即（50,50）（如图2）。即radar_map共有15*4*101*101个值，按照“THYX（时间、高度、Y轴、X轴）”排列，以空格分开，顺序为：T0H0Y0X0 T0H0Y0X1 … T0H0Y0X100 T0H0Y1X0 … T0H1Y0X0 … T1H0Y0X0 … T14H3Y100X100。
![](https://github.com/xhhszc/image-for-DM/raw/master/image-1.png)
![](https://github.com/xhhszc/image-for-DM/raw/master/image-2.png)



### 四、阶段性结果展示
实验总体是一个回归问题，即通过雷达图数据预测未来1~2小时的降水量。除了数据的收集整理和准备工作以外，实验主要分为两个阶段。
第一阶段即为训练阶段，即利用训练集数据训练模型参数。本实验通过根据雷达图数据预测的未来降水量和实际训练集中给出的未来降水量之间的均方根误差（RMSE）来训练模型。整个训练过程即得到一个使RMSE最小的模型。
第二阶段即为测试阶段，即利用测试集数据测试模型的性能。同样地，也是通过根据雷达图数据预测的未来降水量和实际测试集中给出的未来降水量之间的均方根误差（RMSE）来评估模型的性能。如果，RMSE很大，则表示该模型存在过拟合的问题。也就是说，虽然在训练集上面获得了较好的预测效果，但是在新的数据集上并没有很好的预测能力。这样的模型需要后期继续进行优化改进，解决其过拟合的问题。如果，RMSE很小，则可以大致表明该模型在新数据集上的预测表现也较为良好。
下图是迭代次数与RMSE值的关系图像，两条线分别代表在训练集上的情况和在测试集上的情况。可以看出在训练集上，RMSE值随着迭代次数的增加而减小。这就表明随着迭代次数的增加，该模型再训练集上的预测性能确实有很大程度的提升。而在测试集上面的效果显然不是很优异。RMSE值随着迭代次数的增加并没有显著的持续减小。这就表明该模型存在过拟合的问题，虽然在训练集上表现很好，但是在测试集上的性能仍有待提高。

![](https://github.com/xie-xie/image_for_DMhomework/raw/master/image-1.png)

### 五、仍需解决的问题

就目前的实验结果来看，整个模型存在过拟合的问题。可以看出，该模型在训练集数据上表现十分优异，但是，在测试集数据上所体现的性能却仍有待提高。在接下来的实验过程中，我们会针对过拟合问题对模型进行优化和改进。
