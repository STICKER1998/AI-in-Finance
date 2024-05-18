AB Test
================================
1. AB Test的定义
AB测试是为WEB或者APP界面或流程制作两个或者多个版本，。。。
2.AB Test的优势
AB Test 全量怎么和实验效果不一致？
## 2.AB Test的本质
AB测试的作用：为了检验新功能的效果；
我们假设新功能是无害的，设计AB实验来检验真正的效果；
AB测试的本质：假设检验；


### 1.背景
某电商公司非常注重自己的落地页设计，希望能够改进设计来提高转化率。往年公司的全年转化率在13%左右，现在希望能够使用新页面来提高转换率，希望能够达到15%的转换率。

### 2.A/B测试基本流程
了解了基本的背景之后，就可以进入AB测试。



#### 确定实验目标及衡量指标
在本案例中实验目标是通过AB测试确定新落地页是否可以提升2%的转化率。衡量指标为页面转化率。
#### 设计实验方案
实验变量，实验时间，实验样本量，划分实验组和对照组。
#### 执行实验并收集数据
#### 数据分析和结果评估


### 3.设计A/B test实验
> [!NOTE]
> **AB Test 基本步骤**
> - 提出假设
> - 确定实验分组
> - 计算实验样本量及试验周期
> - 上线AB测试并收集数据
> - 数据分析及假设检验
> - 得出结论及建议

#### 3.1 提出假设
单位检验 or 双尾检验

本案例原则上应该要选择右侧单尾检验，但是我们并不能确定新页面的性能一定比当前页面更好，所以选择双尾检验。

> [!NOTE]
> - 如果备择假设H1是 $\neq$ ，则是双尾检验；
> - 如果备择假设H1是 $\ge$ 或者 $>$ ，则是右侧单尾检验；
> - 如果备择假设H1是 $\le$ 或者 $<$，则是左侧单尾检验；

本案例中假设检验问题可以写成：
- 原假设 $H_0: P=P_0$
- 备择假设 $H_1: P\neq P_0$
其中P_0代表旧版落地页的转化率，P代表新版的转化率。

#### 3.2 确定实验分组
在此次AB实验中，我们分为实验组和对照组两组：
- 对照组：这一组用户将看到旧版落地页；
- 实验组：这一组用户将会看到新版落地页；

#### 3.3 计算实验样本量、试验周期

**实验样本量的确定**
我们之前讲过

$$N = \frac{\sigma^2}{\delta^2}{(Z_{1-\frac{\alpha}{2}}+Z_{1-\frac{\beta}{2}})}^2 $$

在这个公式当中，$\alpha$ 为第一类错误的概率，$\beta$ 为犯第二类错误的概率，$\sigma$ 为样本数据的标准差，$\delta$ 代表的是预期实验组和对照组两组数据的差值。



