---
title: 第 18 章 基于IDEA插件开发和字节码插桩技术，采集研发过程中代码执行信息
pay: https://t.zsxq.com/0c7qkNTdA
---

# 第 18 章 基于IDEA插件开发和字节码插桩技术，采集研发过程中代码执行信息

作者：小傅哥
<br/>博客：[https://bugstack.cn](https://bugstack.cn)

>沉淀、分享、成长，让自己和他人都能有所收获！

## 一、前言

`哪个架构师没造过轮子？`

你想过这样一件事吗？**是先具备能力在安排职位，还是先安排职位在学习？** 

就像我们上学考试、跆拳道考段、晋升答辩一样，都是先具备了可胜任上一阶段的能力，才给予相应的职位。所以，架构师造轮子从做程序员时候就开始了，只不过到了架构师阶段可以造出更好的轮子。

鉴于实际业务开发的紧急程度，不会允许你造轮子。但造轮子，几乎是每个程序员突破技术瓶颈的最佳方式。千万不要因为一句，**不要重复造轮子**的借口，给自己一个不学习的理由！

## 二、需求背景

本章节的需求是如何保证交付阶段的代码质量？

![图 18-1](https://bugstack.cn/assets/images/middleware/18-1.png)

业务提需求，产品定方案，研发做实现，测试验流程。四种角色的相互配合是确保一个需求上线的必备条件。在整个需求的交付质量级别划分中，研发与测试是非常重的一环，如果研发提测的代码质量不高，就会出现不同级别的修BUG、返工甚至重做的风险。

**那么**，怎么来提高代码质量呢？一般我们都会要求研发在开发代码的过程中编写单元测试，验证自己的代码逻辑。如果最终单元测试覆盖度不足，可以由测试拒绝研发提测。

**但是**，整个需求实现的代码是在全部开发完成后提测的，也就是临近上线的最后一环，大家才知道某个研发的某个功能域的实现是否具备提测条件。如果这个时候代码质量不高，那么接下来就是项目风险的时候。`压测试时间`、`调上线时间`，总之有病拖着最后成大病了！

**当然**，你可以在项目开发期间定期排查代码，或者在日会进度反馈等等手段。可这样需要耗费大量时间1拖1的开发排查方式很难满足复杂流程的较大型项目开发，而且对于项目风险把控也是不可预估的。

**所以**，我们希望采集研发在开发过程中的执行动作，把风险判断提前。实际操作举例就是，`当你开发完成一个接口，开始测试运行时`，我们的插件就可以采集到这个接口的全部信息，包括：接口名称、入参类型和内容、出参类型和内容、异常信息、调用关系链等。而再把这些信息汇总提交到服务端，生成本次需求代码分支下的全部接口动作，以及各系统间的关系链路，并附带随时生成最新的接口文档和一键测试验证功能。后期测试人员介入时就可以参考研发在编码过程中的全部测试用例，也可以查看整个功能的覆盖程度，此外测试人员测试过程中的数据也会被保留下。现在拥有这些数据信息以后，就可以完整的生成一套研发测试质量交付全览图，让整个工程开发交付质量评估透明化。