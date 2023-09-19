# <center>Lab1 QAM Modulation & Decode</center>

<center>汪海玉 12011331 &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp 张旭东 12011923 </center>

## Introduction

### QAM Demodulation

The full demodulation flow is shown as below:

![](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/chart2.svg)

1. First we should use a sampling signal with T=T~s~ to convert the analog signal to digital.
2. Perform the under-sampling to obtain y[n].
3. Locate the input symbols.
4. convert the symbols to corresponding bit according to the inverse symbol map.

In simulation, we simplify the process. After under-sampling, we only need to deal with the y[n]. And the procedure will be depicted in the following report.

### Maximum-likelihood Receiver

Suppose we are under gauss channel, and every symbol appears in the same possibility. We can get the following equation.
$$
\begin{align*}
\widehat{m}&=\mathrm{arg}\max_{1\leqslant m\leqslant M} \left[ \frac{N_0}{2}\ln P_{\mathrm{m}}-\frac{1}{2}\left\| \boldsymbol{r}-s_{\mathrm{m}} \right\| \right] 
\\
&=\mathrm{arg}\max_{1\leqslant m\leqslant M} \left[ -\left\| \boldsymbol{r}-s_{\mathrm{m}} \right\| ^2 \right] 
\\
&=\mathrm{arg}\min_{1\leqslant m\leqslant M} \left\| \boldsymbol{r}-s_{\mathrm{m}} \right\|
\end{align*}
$$
It turns out that if we want to maximize the probability of recovering the correct symbol we need to minimize $ \left\| \boldsymbol{r}-s_{\mathrm{m}} \right\|$. And in the complex plane, it corresponds the Euclidean distance.

The following constellation map can show the process more intuitionistic.

![image-20220914203321716](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914203321716.png)

We can find that $d_1<d_2<d_4<d_3$, so the decode result should be $00$.

## Lab Results and Analysis

### 16/64-QAM Demodulation

#### Program diagram

Inspired by the given QPSK demodulation process, to demodulate 16QAM and 64QAM we just need to give the right symbol map, find the minimal distance and convert it into the right bit stream.

![image-20220914194426370](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914194426370.png)

The symbol map is generated in the `MT Generate System Parameters.vi`. So the program diagram of 16QAM and 64QAM are shown as below:

![image-20220914203954676](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914203954676.png)



![image-20220914204013147](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914204013147.png)

There are two things deserve our attention:

1. We need normalize the power of each symbol just as our mapping rule, or there will be a scale in inverse map and may course some problems.
2. We need to set `samples per symbol` and `M_QAM` to `64` of the `MT Generate QAM System Parameters(M)` in the 64QAM, so that it can generate the right symbol map of 64QAM. We can ignore this step in 16QAM, that is because the default value of this module is 16.

![image-20220914205714733](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914205714733.png)

#### Result

The result of 16QAM and 64QAM are shown as below:

+ 16QAM:

![image-20220914205812469](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914205812469.png)

![image-20220914205842938](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914205842938.png)

![image-20220914205907872](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914205907872.png)

64QAM:

![image-20220914210001611](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914210001611.png)

![image-20220914210059084](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914210059084.png)

![image-20220914210118894](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220914210118894.png)

1. We can deduce from the `received constellation` and `average bit-error rate (ABER)` that:

   1. As `noise power` increases, so will `ABER.`
   2. The `noise power` has a threshold value below which the `ABER` is zero.
   3. The threshold value varies depending on the modulation. The value for 16QAM is around -10dB, and for 64QAM it is around -20dB.

### `BER` under different `SNR` conditions

To find the relationship between different modulation and the threshold and compare the performance between different modulation. We draw the `SNR-BER` of different modulation in one graph. 

To save the page, I will just introduce the how to draw curve of 64QAM, the same goes for the rest.

The program diagram is shown as below:

![image-20220924170958018](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220924170958018.png)

I control the generation of `SNR` with a `for-loop` and save the value in an array before inserting it into the `AWGN` subvi. I can get the `BER` under the current `SNR` after decoding. We can now use a`shift-register` and another array to store the entire result. After calculating the result, we can `bundle` it and plot it in a graph. If we copy the program and change the attributes, we will get a different curve. The end result is as follows:

1. ![image-20220924164837750](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220924164837750.png)

Since the step length is 0.1dB and a little bit change in BER will vary a lot in log scale, so the end of the curve oscillates violently. It will be more smooth if we change the step length to 1dB

![image-20220924172439582](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20220924172439582.png)

We can find it here:

1. As SNR increases, BER will decrease.
2. BPSK has the highest noise resistance, followed by QPSK, 16QAM, and 64QAM.

## Experience

1. I discovered how Quadrature amplitude modulation (QAM) works.
2. I learnt the function of symbol map.
3. I discovered the performance differences between various modulations.



### In class submission

汪海玉：

Week1:

![](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/1.png)

![2](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/2.png)

Week2:

![QQ图片20220914150037](D:/Files/Download/Windows%20Download/Chrome%20Download/QQ%E5%9B%BE%E7%89%8720220914150037.png)

![屏幕截图 2022-09-14 150104](D:/Files/Download/Windows%20Download/Chrome%20Download/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202022-09-14%20150104.png)

