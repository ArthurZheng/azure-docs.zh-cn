---
title: K-Means 群集：模块参考
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习中的 K-Means 群集模块来训练群集模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 08/04/2020
ms.openlocfilehash: 97cadfb8f5004cfd2701335172d4416c64f05259
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "90907864"
---
# <a name="module-k-means-clustering"></a>模块：K 平均值聚类

本文介绍如何在 Azure 机器学习设计器中使用 *K 平均值聚类分析* 模块来创建未经训练的 K 平均值聚类分析模型。 
 
K-means 是最简单、最常见的非监督式学习算法之一  。 可以将算法用于各种机器学习任务，如： 

* [检测异常数据](https://msdn.microsoft.com/magazine/jj891054.aspx)。
* 群集文本文档。
* 在使用其他分类或回归方法之前，分析数据集。 

若要创建群集化模型，请执行以下操作：

* 将此模块添加到管道。
* 连接数据集。
* 设置参数，例如所需群集数、创建群集时使用的距离指标等。 
  
配置模块超参数后，将未经训练的模型连接到[训练群集模型](train-clustering-model.md)。 由于 K-means 算法是非监督式学习方法，因此有一个可选的标签列。 

+ 如果数据包含标签，可以使用标签值来指导群集选择并优化模型。 

+ 如果数据没有标签，则算法会完全基于数据来创建表示各种可能的类别的群集。  

##  <a name="understand-k-means-clustering"></a>了解 K-Means 群集
 
通常，群集化操作使用迭代技术将数据集中的事例归组到具有相似特征的群集中。 这种归组过程有助于浏览数据、标识数据中的异常情况，并最终帮助进行预测。 群集模型还有助于识别数据集中的关系，这些关系可能无法通过浏览或简单观察数据以逻辑推理的方式推导出来。 因此，通常会在机器学习任务的早期阶段使用群集化来探究数据和发现预期之外的相关性。  
  
 使用 K-means 方法配置群集化模型时，必须指定目标数值 K，该数值指示模型中所需的质心数目   。 质心是代表每个群集的点。 K-means 算法通过最大程度地减少群集内平方和，将每个传入的数据点分配给一个群集。 
 
处理训练数据时，K-means 算法始于一组随机选择的初始质心。 质心充当群集的起点，应用 Lloyd 算法以迭代的方式优化自身位置。 当 K-means 算法满足以下一个或多个条件时，会停止构建和优化群集：  
  
-   质心稳定，意味着各个点的群集分配不再改变，并且算法已收敛到一个解决方案。  
  
-   该算法已运行完指定的迭代数。  
  
 完成训练阶段后，使用[将数据分配给群集](assign-data-to-clusters.md)模块，将新事例分配到使用 K-means 算法找到的某个群集中。 通过计算新事例与每个群集的质心之间的距离，执行群集分配。 将每个新事例分配到质心最近的群集中。  

## <a name="configure-the-k-means-clustering-module"></a>配置“K-Means 群集”模块
  
1.  将“K-Means 群集”模块添加到管道  。  
  
2.  若要指定训练模型的方式，选择“创建训练模式”选项  。  
  
    -   **单个参数**：如果知道要在群集模型中使用的确切参数，可以提供一组特定的值作为参数。  
  
3.  对于“质心的数量”，请键入希望算法初始时处理的群集数  。  
  
     此模型不能保证精确生成这一数量的群集。 算法在初始时处理此数量的数据点，然后执行迭代操作以寻找最佳配置。 可以参考 [sklearn 的源代码](https://github.com/scikit-learn/scikit-learn/blob/fd237278e/sklearn/cluster/_kmeans.py#L1069)。
  
4.  使用 Initialization 属性指定用于定义初始群集配置的算法****。  
  
    -   **第一个 N**：系统会从数据集中选择一定数量的初始数据点，将其用作初始平均值。 
    
         此方法也称为 Forgy 方法**。  
  
    -   **随机**：该算法将某个数据点随机放置在某个群集中，然后计算初始平均值作为群集的随机分配点的质心。 

         此方法也称为“随即划分”方法**。  
  
    -   **K-Means++** ：这是默认的群集初始化方法。  
  
         “K-Means++”算法由 Arthur 和 Sergei Vassilvitskii 于 2007 年提出，用于优化标准“K-Means++”算法的群集化性能****。 K-Means++ 采用了一种不同的方法来选择初始群集中心，从而对标准 K-Means 进行了优化****。  
  
    
5.  对于“随机数种子”，可以选择键入一个值，将其用作群集初始化的种子****。 该值可能会极大影响群集选择。  
  
6.  对于“指标”，选择用于测量群集矢量之间或新数据点与随机选择的质心之间的距离的函数****。 Azure 机器学习支持以下群集距离指标：  
  
    -   **欧几里得**：K-Means 群集化常使用欧几里得距离作为群集散点图的度量值。 常用此指标是因为它最大程度地减少了点与质心之间的平均距离。
  
7.  对于“迭代”，请键入算法在完成质心选择之前应循环访问训练数据的次数****。  
  
     可以根据训练时间调整此参数以平衡精确度。  
  
8.  对于“分配标签模式”，请选择一个选项，用于指定应如何处理数据集中的某个标签列****。  
  
     由于 K-Means 群集化是一种非监督式机器学习方法，因此标签是可选的。 但是，如果数据集已具有标签列，则可以使用这些值来指导群集的选择，或者可以指定忽略这些值。  
  
    -   **忽略标签列**：将忽略标签列中的值，构建模型时不会使用这些值。
  
    -   **填充缺失值**：将标签列的值作为特征使用，帮助构建群集。 如果任何行缺少标签，则使用其他特征来输入值。  
  
    -   **从最接近中心的点开始覆盖**：使用最靠近当前质心的点的标签，将标签列值替换为预测的标签值。  

8.  如果要在训练之前将特征规范化，请选择“规范特征”选项****。
  
     如果应用了规范化，在训练之前数据点将由 MinMaxNormalizer 规范化为 `[0,1]`。

10. 定型模型。  
  
    -   如果将“创建训练程序模式”设置为“单个参数”，请使用[训练群集化模型](train-clustering-model.md)模块添加带标记的数据集并训练模型********。  
  
## <a name="results"></a>结果

配置和训练完模型后，一个能生成分数的模型就创建完成了。 然而，训练模型的方式有多种，查看和使用结果的方式也有多种： 

### <a name="capture-a-snapshot-of-the-model-in-your-workspace"></a>捕获工作区中模型的快照

如果使用了[训练群集模型](train-clustering-model.md)模块：

1. 选择“训练群集模型”模块并打开右侧面板。

2. 选择“输出”选项卡。选择“注册数据集”图标以保存已训练模型的副本。

保存的模型表示保存模型时的训练数据。 如果以后更新了管道中使用的训练数据，已保存的模型不会更新。 

### <a name="see-the-clustering-result-dataset"></a>查看群集化结果数据集 

如果使用了[训练群集模型](train-clustering-model.md)模块：

1. 右键单击“训练群集模型”模块****。

2. 选择“可视化”。

### <a name="tips-for-generating-the-best-clustering-model"></a>有关如何生成最佳群集模型的提示  

已知群集化过程中使用的种子设定过程可能会显著影响模型**。 种子设定指的是将一些初始点设为潜在的质心。
 
例如，如果数据集包含多个离群值，并且选择了一个离群值来设定群集种子，则没有其他数据点能适合该群集，且该群集可能是单一实例。 即它可能只有一个点。  
  
可以通过以下几种方式来避免此问题：  
  
-   更改质心的数量，并尝试多个种子值。  
  
-   创建多个模型，使用不同指标或增加循环访问次数。  
  
通常，使用群集化模型，任何给定的配置都可能会生成一组经过本地优化的群集。 换言之，由模型返回的一组群集只适合当前数据点，而不适用于其他数据。 如果采用不同的初始配置，则使用 K-Means 方法时可能会发现另一种高级的配置。 

## <a name="next-steps"></a>后续步骤

请参阅 Azure 机器学习的[可用模块集](module-reference.md)。 
