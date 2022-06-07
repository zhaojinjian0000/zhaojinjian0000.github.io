<!--
.. title: Tracking视觉基础知识
.. slug: Tracking_Basic_knowledge
.. date: 2021-03-26 16:55:18 UTC+08:00
.. tags: CV
.. category: Basic knowledge
.. link: 
.. description: Some Basic knowledge of Tracking
.. type: text
.. has_math: true
-->

- VOT评价指标
- 其他评价方法/指标

<!-- TEASER_END -->
---
# VOT评价指标

### 参考
- [VOT2020 测评指标](https://www.bilibili.com/read/cv7654043/)
- [【技术向】VOT中的EAO是如何计算的](https://blog.csdn.net/sinat_27318881/article/details/84350288)
- [VOT（Visual Object Tracking）评价指标综述](https://blog.csdn.net/Dr_destiny/article/details/80108255)

### 数据集（Datasets）

- 标注：VOT的标注中，IOU大于60%就算合格（VOT2013）
- 属性：数据集包括干扰属性，来衡量tracker在不同情况下的表现、包括camera motion、illumination change、object size change、object motion change、non-degraded。（VOT2013）
- VOT2020标注改为了mask，对应分数也基于mask计算。

###  评价系统（Evaluation systems）
- 重启机制：为了使用到大部分的帧，跟丢5帧后，重新初始化。重启过程中没被使用到的帧叫burn-in period，大约为初始化之后（包括第一帧的初始化和所有重启）的10帧。用到的叫有效帧。（重启的问题：跟踪器的初始化敏感重启后也可能有遮挡，overlap判定重启会导致作弊，跟丢一帧立刻判丢过于严格。）
- 初始化点（initialization points，anchors）（VOT2020）：在初始帧到结束帧之间（闭集），每隔一段序列设置一个初始化点，（50帧，2s左右，会手动移动避免遮挡）。如果右边帧多，正向运行，否则方向运行（即放在视频1/2以前的视频正向运行，否则反向运行）。
- tentative failure（VOT2020）：跟丢的定义变得宽容了一点，用来防止不小心跟丢了几帧就直接判输（gaming）。overlap小于阈值时认为是tentative failure，（这时为防止重启验证会自动输整张图像作为bbox），在一定数量帧（设定为10帧）内仍未恢复才算做跟丢failure（会把判定跟丢前的10帧tentative failure也算作跟丢），如果恢复了，tentative failure算作没跟丢。

### tracker性能指标
- A（Accuracy）：某一个tracker在某个视频中的平均IOU（在所有有效帧上的平均、在所有重复中的平均）
- R（Robustness）：某一个tracker的在某个视频中跟丢的次数（在所有重复中的平均）。（单帧accuracy的值低于设定的阈值判定为失败）,取其-S倍再做e指数（取值为0到1，1表示没有失败，0代表无穷次失败）作为衡量指标。
- A（Accuracy）（VOT2020）：引入anchor后，某个序列在某个anchor的A指：从该initialization points开始直到丢失前的平均overlap。OPE。
- R（Robustness）（VOT2020）：引入anchor后，某个序列在某个anchor的R指：（从该initialization points直到丢失前的帧数）占（从该initialization points开始直到结束的帧数）的比例。OPE。
- Attribute normalized AR Rank/scores：所有帧上的表现→所有序列上的表现→（按照序列长度加权平均）所有属性上的平均表现→在所有属性上的（不加权）平均表现。表现包含A和R，如Rank则对A和R分别排名，再取平均，得到综合排名。
- Pooled AR Rank/scores：所有帧上的表现→所有序列上的表现→（按照序列长度加权平均）所有序列上的平均表现。即跳过属性这一步。
- EFO（Equivalent Filter Operations ）（VOT2014）：衡量tracking速度。基准单位：600x600x1图像上用30x30x1x1的最大值滤波器滤波的时间。
- EAO（VOT2015）：按照每次跟丢的位置可以把视频分成若干个序列。以长度为横坐标获得直方图，以某一长度序列的平均帧覆盖率（average of per-frame overlaps）为纵坐标，随着视频帧长度的增加，平均覆盖率值总体上会降低。求平均帧覆盖率关于视频帧长度的积分，积分限为典型视频长度的范围。典型视频长度的所有序列个数占所有序列个数的50%，且离最高点最近的取值范围。
- EAO（VOT2020）：按照anchor位置划分，而不是跟丢的位置。

### tracker衡量两个tracker差异
- 非参数检验：针对accuracy，应用 Wilcoxon Signed-Rank test；针对robustness，应用Wilcoxon Rank-Sum (MannWhitney U-test)
- practical difference（VOT2014）只针对accuracy判断两个tracker表现是否相同。

---
# 其他评价方法/指标：AUC、ROC
### OPE、TRE、SRE、OPER、SPER ：
- 参考：[Online Object Tracking Benchmark OOTB 目标跟踪系统评估方式](https://blog.csdn.net/hjl240/article/details/52453030)
- OPE（One-pass evaluation）：无重启机制，用ground-truth中目标的位置初始化第一帧，然后运行跟踪算法得到平均精度和成功率
- OPER（One-pass evaluation with restart）：在跟踪期间，如果跟踪失败，那么就在下一帧重新初始化然后再跟踪，
-  SRER（Spatial robustness evaluation with restart）
- TODO
### AUC ：
### ROC
# MOT 入门 
- https://www.youtube.com/watch?v=ay_QLAHcZLY&list=PLadnyz93xCLhSlm2tMYJSKaik39EZV_Uk
# DIMP、ATOM
- TODO:


[//]: # "<span><div style="text-align: center;">"
[//]: # "![zhaojinjian0000](/images/zhaojinjian0000.thumbnail.jpg)"
[//]: # " </div></span>"

