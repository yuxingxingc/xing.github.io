---
layout: post
title: 5G测量之测量分类和小区分类
tags: 5G measurement
---

# NR 测量

![](/images/posts/5GMeasurement/measurement_and_cells.jpg)

## 3GPP 相关章节

- 38.331（5.5 Measurements）
- 38.300 （9 Mobility and State Transitions）
- 23.501（5.3.4 UE Mobility）
- 23.502（4.9 Handover procedures）



## HandOver相关参数和流程

​	因为5G系统中相关测量和移动性的知识比较多，我主要整理为三个方面来理解（当然除了下面所说的，NR的handover还包括了UE在4G和5G之间基站的切换，还有和核心网AMF的相关的Handover，这里我只关注NR站间的HandOver）：

1. UE的移动性和测量相关的能力参数。
2. gNB的移动性和测量的相关参数。
3. HO的过程和测量的过程。

**Master Cell Group**: in MR-DC, a group of serving cells associated with the Master Node, comprising of the SpCell
(PCell) and optionally one or more SCells.



**Primary Cell**: The MCG cell, operating on the primary frequency, in which the UE either performs the initial
connection establishment procedure or initiates the connection re-establishment procedure.



**Primary SCG Cell**: For dual connectivity operation, the SCG cell in which the UE performs random access when
performing the Reconfiguration with Sync procedure.



**Secondary Cell**: For a UE configured with CA, a cell providing additional radio resources on top of Special Cell.



**Secondary Cell Group**: For a UE configured with dual connectivity, the subset of serving cells comprising of the
PSCell and zero or more secondary cells.



**Serving Cell**: For a UE in RRC_CONNECTED not configured with CA/DC there is only one serving cell comprising
of the primary cell. For a UE in RRC_CONNECTED configured with CA/ DC the term 'serving cells' is used to denote
the set of cells comprising of the Special Cell(s) and all secondary cells.



**Special Cell**: For Dual Connectivity operation the term Special Cell refers to the PCell of the MCG or the PSCell of the
SCG, otherwise the term Special Cell refers to the PCell.
