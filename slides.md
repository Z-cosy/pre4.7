---
theme: seriph
transition: fade
background: #fff
class: text-center
layout: image-right
image: './images/cover3.png'  # 注意这里使用相对路径
---

<style>
.blue-bold {
  color:rgb(5, 81, 156);
  font-weight: bold;
}
/* 这会应用于除第一页外的所有幻灯片 */
.slidev-layout:not(.slidev-page-1 .slidev-layout)::before {
  content: '';
  position: fixed;
  top: 10px;
  right: 10px;
  width: 140px;
  height: 60px;
  background-image: url('./images/ee.png'), url('./images/zju.png');
  background-position: right top, right 70px top;
  background-repeat: no-repeat;
  background-size: 60px auto;
  z-index: 100;
  pointer-events: none;
}

/* 首页左上角图标样式 */
.slidev-page-1 .slidev-layout::before {
  content: '';
  position: fixed;
  top: 10px;
  left: 10px;
  width: 140px;
  height: 60px;
  background-image: url('./images/ee.png'), url('./images/zju.png');
  background-position: left top, left 70px top;
  background-repeat: no-repeat;
  background-size: 60px auto;
  z-index: 100;
  pointer-events: none;
}

/* 目录样式 */
.custom-toc {
  width: 80%;
  margin: 40px auto;
  font-size: 1.3em;
  line-height: 1.8;
}

.toc-item {
  margin-bottom: 0.5em;
  display: flex;
}

.toc-number {
  width: 30px;
  text-align: right;
  margin-right: 10px;
}

.sub-toc-item {
  margin-left: 40px;
  display: flex;
}

.sub-toc-number {
  width: 30px;
  text-align: right;
  margin-right: 10px;
}

/* 高亮和暗化样式 */
.dimmed {
  opacity: 0.3;
}

.highlighted {
  color: #000000; /* 改为黑色 */
  font-weight: bold;
}

/* 首页特殊样式 */
.cover-content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 100%;
  padding: 0 1rem;
}

.cover-content h1 {
  font-size: 2.2rem;
  line-height: 1.3;
  margin-bottom: 1.5rem;
  font-weight: bold;
}

.cover-content .subtitle {
  font-size: 1.5rem;
  margin-top: 1.5rem;
  color: #555;
}

.cover-content .logos {
  margin-top: 3rem;
  display: flex;
  align-items: center;
}

.cover-content .logos img {
  height: 3rem;
  margin-right: 1.5rem;
}
</style>

<div class="cover-content">
  <h1>电网络分析小组探究成果汇报</h1>
  <h2>日光灯功率因数的探究</h2>
  <div class="subtitle">答辩人：曾文博</div>
  <div class="subtitle">组员：余浩棋</div>
</div>

---
layout: two-cols
hideInToc: true
---
# 目录 Table of Content

## 理论基础

<br>
    1. 关于奇次谐波的产生
    <br><br>
    2. 关于并联电容“谐波”放大效应
    <br><br>
    3. 关于非正弦下的功率——IEEE标准
<br><br>

## 实验验证
<br>
    1. 电流FFT
    <br>
    <br>
    2. 功率因数

::right::
# 关键问题 Crucial Problems

1. 为什么会产生谐波？
<br><br>
2. 为什么谐波频率都是基波频率的奇数倍？
<br><br>
2. 为什么并上电容之后才能观察到谐波？
    - 事实上，这也为三相电路实验中线电流提供了解释
<br><br>
3. 为什么功率表测不准？
<br><br>




---
layout: section
---

# 理论基础 <sup>[1]</sup> <sup>[2]</sup>

<div class="absolute bottom-4 left-10 right-10 text-xs text-gray-500">
  <hr class="border-t border-gray-300 w-1/3 mb-2 mt-8 ml-0">
  <p class="text-left"><sup>[1]</sup> "IEEE Standard Definitions for the Measurement of Electric Power Quantities Under Sinusoidal, Nonsinusoidal, Balanced, or Unbalanced Conditions," in IEEE Std 1459-2010 (Revision of IEEE Std 1459-2000), vol., no., pp.1-50, 19 March 2010, doi: 10.1109/IEEESTD.2010.5439063.</p>
  <p class="text-left"><sup>[2]</sup> 孔斌.并联电容器回路谐波放大及电能损耗特性的测试与分析[D].郑州大学,2004.</p>
</div>




---
layout: two-cols

---

# 关于奇数次谐波的产生

<div class="text-sm">

气体放电管（如日光灯灯管）的核心是一种气体放电现象。其导通过程具有高度的**非线性**。

为了数学上描述这种非线性，我们可以用一个（简化的）非线性函数来近似灯管的伏安特性 $i = f(v)$。一个常用的方法是**使用幂级数来表示**（假设在工作点附近可导）：

$$i(t) = a_0 + a_1v(t) + a_2v(t)^2 + a_3v(t)^3 + ...$$

其中 $a_0, a_1, a_2, a_3, ...$ 是描述非线性特性的系数。对于纯线性电阻，只有 $a_1$ 非零。

假设施加在灯管上的电压近似为正弦波：$v(t) = V_m \sin(\omega t)$。将其代入上述幂级数：

$$i(t) = a_0 + a_1(V_m\sin(\omega t)) + a_2(V_m\sin(\omega t))^2 \\+ a_3(V_m\sin(\omega t))^3 + ...$$

</div>

::right::

<div class="text-sm pt-10">

👉现在，我们需要使用三角函数恒等式来展开高次项：

$$\sin^2(\omega t) = (1 - \cos(2\omega t)) / 2$$

$$\sin^3(\omega t) = (3\sin(\omega t) - \sin(3\omega t)) / 4$$

$$\begin{aligned}
\sin^4(\omega t) &= \sin^2(\omega t) \cdot \sin^2(\omega t) \\
&= [(1 - \cos(2\omega t))/2]^2 \\
&= (1 - 2\cos(2\omega t) + \cos^2(2\omega t))/4 \\
&= (1 - 2\cos(2\omega t) + (1+\cos(4\omega t))/2)/4 \\
&= (3 - 4\cos(2\omega t) + \cos(4\omega t)) / 8
\end{aligned}$$

👉对于理想的气体放电（忽略电极材料、温度等不对称因素），在正半周和负半周施加相同大小的电压时，其电流响应的大小也应该是相同的，只是方向相反。即，伏安特性曲线 $i = f(v)$ 应该具有**中心对称性** (关于原点对称)。数学上，中心对称意味着 $f(-v) = -f(v)$。

这直接导致了**所有偶次幂的系数为零→所有偶次谐波系数为零**

</div>

---
layout: center
---
# 结论

<div class="grid grid-cols-2 gap-8 mt-8">
  <div class="bg-blue-100 border-l-4 border-blue-500 text-blue-700 p-4 rounded-lg shadow-md">
    <div class="flex items-center">
      <div class="text-2xl mr-4">💡</div>
      <div>
        <p class="font-bold">灯管是高次谐波的源头，这是一个非线性元件</p>
      </div>
    </div>
  </div>
  
  <div class="bg-blue-100 border-l-4 border-blue-500 text-blue-700 p-4 rounded-lg shadow-md">
    <div class="flex items-center">
      <div class="text-2xl mr-4">💡</div>
      <div>
        <p class="font-bold">谐波只会有奇次谐波，不会含有偶次分量</p>
      </div>
    </div>
  </div>
</div>


---
layout: two-cols
---

# 关于并联电容"谐波"放大效应

<div class="text-sm">



设电容器组末加串联电阻器。忽略电阻，则由图2.1知系统母线n次谐波电压为

$$U_n = \left|\frac{x_{sn} x_{cn}}{x_{sn} - x_{cn}}\right|I_n$$

注入系统的谐波电流为

$$I_{pn} = \left|\frac{x_{cn}}{x_{sn} - x_{cn}}\right|I_n$$

电容器支路的谐波电流为

$$I_{cn} = \left|\frac{x_{sn}}{x_{sn} - x_{cn}}\right|I_n$$

设电容器支路与系统等值支路的谐波电抗之比为α，即

$$\alpha = -x_{cn}/x_{sn} = \frac{x_c}{n^2x_s}$$



</div>

::right::
<br>
<div class="text-sm pt-10">
则母线谐波电压和支路谐波电流与总谐波电流In的比值与α有关，即

$$U_n/I_n = \left|\frac{\alpha}{1+\alpha}\right|x_{sn}$$

$$I_{pn}/I_n = \left|\frac{\alpha}{1+\alpha}\right|$$

$$I_{cn}/I_n = \left|\frac{1}{1+\alpha}\right|$$


</div>
<img src="/images/image.png" class="h-48 mx-auto my-2" />


---
layout: center
---

# 谐波放大效应分析

<div class="text-sm">

当$\alpha = -2$，即谐波次数$n = \sqrt{x_c/2x_s} = n_a$时，$I_{pn}/I_n = 2$，$I_{cn}/I_n = 1$，$U_n/I_n = 2x_{sn}$，注入系统的谐波电流被**放大1倍**。

当$\alpha = -1/2$，即谐波次数$n = \sqrt{2x_c/x_s} = n_b$时，$I_{pn}/I_n = 1$，$I_{cn}/I_n = 2$，$U_n/I_n = x_{sn}$，电容器支路的谐波电流被**放大1倍**。

当$\alpha = -1$，即谐波次数$n = \sqrt{x_c/x_s} = n_0$时，系统出现**并联谐振**，$I_{pn}$，$I_{cn}$和$U_n$都达到最大值，谐波放大达到最严重的程度。

显然$n_a < n_0 < n_b$。在谐波次数处于$n_a \leq n \leq n_b$范围内，谐波电流放大严重，这一谐波范围称为**严重放大区**。

</div>

---
layout: image
image: '/images/image 3.png'
class: 'bg-cover text-center flex flex-col justify-center'
style: |
  .slidev-layout {
    background-size: cover;
    background-position: center;
  }
  .slidev-layout h1, .slidev-layout p {
    text-align: center;
    width: 100%;
    color: white; /* 可根据背景图片调整文字颜色 */
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.5); /* 添加文字阴影提高可读性 */
  }
---





<div class="grid grid-cols-2 gap-8 mt-8">
  <div class="bg-blue-100 border-l-4 border-blue-500 text-blue-700 p-4 rounded-lg shadow-md">
    <div class="flex items-center">
      <div class="text-2xl mr-4">💡</div>
      <div>
        <p class="font-bold">电容会放大谐波</p>
      </div>
    </div>
  </div>
  
  <div class="bg-blue-100 border-l-4 border-blue-500 text-blue-700 p-4 rounded-lg shadow-md">
    <div class="flex items-center">
      <div class="text-2xl mr-4">💡</div>
      <div>
        <p class="font-bold">这是无功补偿中常见的现象</p>
      </div>
    </div>
  </div>
</div>


---
layout: center
---

# IEEE标准
| Quantity or indicator | Combined | Fundamental powers | Nonfundamental powers |
| --- | --- | --- | --- |
| Apparent | $S$(VA) | $S_1$(VA) | $S_n S_h$(VA) |
| Active | $P$(W) | $P_1$(W) | $P_h$(W) |
| Nonactive | $N$(var) | $Q_1$(var) | $D_i D_v D_h$(var) |
| Line utilization | $PF = P/S$ | $PF_1 = P_1/S_1$ | — |
| Harmonic pollution | — | — | $S_n/S_1$ |




---
layout: image
image: '/images/image 1.png'
class: 'bg-cover text-center flex flex-col justify-center'
style: |
  .slidev-layout {
    background-size: cover;
    background-position: center;
  }
  .slidev-layout h1, .slidev-layout p {
    text-align: center;
    width: 100%;
    color: white; /* 可根据背景图片调整文字颜色 */
    text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.5); /* 添加文字阴影提高可读性 */
  }
---



# IEEE标准文件

<div class="grid grid-cols-2 gap-8 mt-8">
  <div class="bg-blue-100 border-l-4 border-blue-500 text-blue-700 p-4 rounded-lg shadow-md">
    <div class="flex items-center">
      <div class="text-2xl mr-4">💡</div>
      <div>
        <p class="font-bold">IEEE给出了明确的定义和标准。在IEEE体系下，非正弦条件下不存在简单的无功的概念</p>
      </div>
    </div>
  </div>
  
  <div class="bg-blue-100 border-l-4 border-blue-500 text-blue-700 p-4 rounded-lg shadow-md">
    <div class="flex items-center">
      <div class="text-2xl mr-4">💡</div>
      <div>
        <p class="font-bold">重要的不是谁对谁错，而是给出一种定义，把事情说清楚</p>
      </div>
    </div>
  </div>
</div>



---
layout: section
---

# 实验验证 <sup>[1]</sup><sup>[2]</sup>

<div class="absolute bottom-4 left-10 right-10 text-xs text-gray-500">
  <hr class="border-t border-gray-300 w-1/3 mb-2 mt-8 ml-0">
  <p class="text-left"><sup>[1]</sup> 以下实验均采用采集卡自主编程完成；</p>
  <p class="text-left"><sup>[2]</sup> 考虑课堂展示效果，这里只展示了部分实验结果。完整实验内容将会以报告的形式上交；</p>
</div>

---

# 电流FFT

<div class="flex justify-center">
  <img src="/images/image 2.png" class="h-80" />
</div>



---


# 功率因数

|  | $PF_1$ | $PF_2$ |
| --- | --- | --- |
| $C= 0\mu F$ | 0.428 | 0.417 |
| $C= 3.0 \mu F$ | 0.905 | 0.858 |
| $C=3.9 \mu F$ | 1.000 | 0.875 |
- 第一种算法是采集卡编程计算的，具体算法是通过**计算相位差**换算得到功率因数
$$ PF = \cos{\theta_{shift}}$$

- 第二种算法是功率表读数，是通过有功功率/视在功率得到的

$$ PF = \frac{P}{S} \ne \frac{P}{\sqrt{P^2 + Q^2}}$$




---
layout: end
---

# 感谢观看，恳请指正！
