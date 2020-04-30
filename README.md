# Temporal-aware-knowledge-graph

## HyTE-modify
根据HyTE的代码和文件改了一下，主要是time_proj.py

## 我现在做了啥
原文的意思是h,r,t三元组表示的这组关系在不同时间点上有不同的投影，即要求h+r-t在$\tao$ 时间点的超平面上投影为0，数学上等价于h,r,t三者分别投影后得到的h'+r'-t'=0

但是分别来看就有点奇怪，h和t在不同时间有不同表示可以理解，就像一个人不同年纪有不同的特征，但关系r在不同时间特征不同就有点奇怪。他这里r的类型有was born in/died in/works at之类的，我感觉直观上不同年代"was born in"的含义应该是一样的。
所以我试了一下只将h,t投影到\tao上，r不投影，结果好像差不多。。。

结果在result.xlsx中，现在只试了yago数据集。三行分别为他报告的结果、我跑他代码得到的结果、r不投影的结果。

我跑他代码的结果比他论文里写的差一些，我是按照他说的最优超参数设置的，不排除他可能虚报了一些。

若不看report的结果，会发现r不投影的方法在relation prediction上效果较好，说明r确实不应该虽时间变化。但其在entitiy prediction上效果不如r投影的好，不知为啥

图1：关系"was born in" 在不同年代的表示（用r投影的方法得到的） (PCA)

![original_wasbornin](./Figure/original_wasbornin.png)

图2：关系"was born in" 在不同年代的表示（用r不投影的方法得到的）

![r_not_transfer_wasbornin](./Figure/r_not_transfer_wasbornin.png)

图3：所有关系 在不同年代的表示（用r投影的方法得到的） 

![original_whole](./Figure/original_whole.png)

图4：所有关系 在不同年代的表示（用r不投影的方法得到的）

![r_not_transfer_whole](./Figure/r_not_transfer_whole.png)

另外我看了一下知识图谱表示方法的transE/TransH/transR等系列方法

## Update 4.30
我试了一下让时间平面连续变化，在loss里面加了个 $$ \sum_{t=1}^T |(t1-t2)-(t2-t3)|_{l_1} $$ 
效果好像差不多 见result.xlsx


## 接下来
对于怎么继续改进他的东西，我没什么想法。。
**ASK FOR YOUR IDEA!**

以下是我瞎想的：

要不就想想把班主任那篇的方法结合进来。

她那篇好像就是求loss时，对年代久远的loss乘个系数少算一些，对年代较近的多算一些

她是用前几年的数据训练，后几年的数据测试

现在HyTE这个是随机选。

但是HyTE这个如果用前几年的数据训练后几年的数据测试有个问题：后几年的\tao平面因为没有训练数据学不到。。

一种可能的方法：把tao约束成连续递增的，并且斜率不变，这样就可以得到后几年的tao。但不知道可不可行，也不清楚怎么约束

HyTE论文里说他没有对tao进行限制，但学出来的tao平面是会按时间聚类的

## 关于中期 除了上面的
将yago数据集可视化一下分析一下 用neo4j之类的

**ASK FOR YOUR IDEA!**
