---
Author:汪海玉 张旭东
Date: 2022/10/12
---

<h1 align = "center">Lab2 Pulse Shaping and Matched Filtering</h1>

<center>汪海玉 12011331 &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp 张旭东 12011923 </center>

## Introduction

In the last experience, we finished the QAM modulation and decode. And after symbol mapping and before decode we need to do pulse shaping and matched filtering. Now we are going to discuss them in detail.

### Pulse Shaping 

In the real channel, there is inter-symbol interference except for the noise. So we define the Signal to Interference plus Noise Ratio (SINR) as:
$$
\mathrm{SINR}=\frac{E_{\mathrm{x}}|g\left( 0 \right) |^2}{N_0\int{|G_{\mathrm{rx}}\left( f \right) |^2df+E_{\mathrm{x}}\sum{_{\mathrm{m}\ne 0}|g\left( mT_{\mathrm{s}} \right) |^2}}}
$$
We can't change the power of noise, so we need to minimalize the power of Interference. The best idea is to let the amplitude of interference at the sampling time to be zero, just like:
$$
g\left( nT_{\mathrm{s}} \right) =\begin{cases}
	1  \ \ n=0\\
	0 \ \  n\ne 0\\
\end{cases}\Leftrightarrow \frac{1}{T_{\mathrm{s}}}\sum_{k=-\infty}^{+\infty}{G\left( f-\frac{k}{T_{\mathrm{s}}} \right)}=1
$$
This is called Nyquist principle. But there is quite lot of functions that meet the requirement. So we want the function that consumes the least bandwidth. As a result, we choose the sinc function:
$$
sinc(t)=\frac{\sin  \pi t/T}{\pi t/T}
$$
But if we want to improve the quality of the communication, we would want the damping be as sharp as possible, so we multiple another part, and get the raised-cosine function:
$$
g_{\mathrm{rc}}=\frac{\sin  \pi t/T}{\pi t/T}\frac{\cos \left( \pi \alpha t \right)}{1-4\alpha ^2t^2/T^2}
$$
![image-20221012220913507](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221012220913507.png)

The damping speed is relevant with the filter parameter $\alpha$, when it equals 0, the whole function becomes normal sinc function, and when it equals to 1, the damping is quite fast. But on the hand, it will consume more bandwidth. We can see the following graph:

![image-20221012221652491](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221012221652491.png)

The smaller the filter parameter is, the wider the bandwidth will be. So if we want to have better quality we should use larger filter parameter and wider bandwidth, and if the requirement for quality is not that high, we can use smaller parameter. It is a typical kind of trade off in real engineering problem.  

### Matched Filtering

Actually, we need the response function of the whole system to be raised-cosine function, and it consists of the following part:

![matched_filter](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/matched_filtering.svg)

Normally, the response function of channel is $\delta(t)$. So we have two options: 

1. We choose the raised-cosine function as the pulse shaping function, then we should use delta function as the matched filtering function.
2. We use the square-root raised cosine function as the pulse shaping function, which satisfies $g_{\mathrm{sqrc}}* g_{\mathrm{sqrc}}=g_{\mathrm{rc}}$. And use the same function as the matched filtering function. The square-root raised cosine function is:

$$
g_{\mathrm{sqrc}}=\frac{4\alpha}{\pi \sqrt{T}}\frac{\cos \left[ \left( 1+\alpha \right) \pi t/T \right] +\frac{\sin \left[ \left( 1-\alpha \right) \pi t/T \right]}{4\alpha t/T}}{1-\left( 4\alpha t/T \right) ^2}
$$



In practice, option 2 is better. That is because it meets the best receiver theory, g~rx~(t)=g~tx~^*^(-t).

## Lab Results and Analysis

### Program diagram

Pulse shaping is in the `transmitter.vi`, here we can see the signal comes out of the `source`, and after `MOD`, it will enter the `pulse shaping` part. And the program diagram is shown as below:

![image-20221013100619635](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221013100619635.png)

![image-20221013095945385](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221013095945385.png)

The process of pulse shaping is actually quite simple. It is just the convolution of pulse shaping function and the signal. The thing that we should notice is that how to use the `modulation parameters in` and `MT Generate Filter Coefficients VI`. According to the help sheet.

![image-20221014105616371](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221014105616371.png)

If we want to get the `pulse shaping filter coeffients` we need to pass the corresponding parameters to that vi. So we should use `Unbundle by name.vi` to get those parameters from the `modulation parameters in`. And `MT Generate Filter Coefficients VI` can generate both  `pulse shaping filter coefficients` and `matched filter coefficients`, we should notice the output that we use, do not use the wrong one. After that, we just need to use `Convolution.vi` to get the results.

Matched filtering is in the `receiver.vi`. Here we can see that, after the `RX` receiving the signal it will pass to the `matched filter`,and after down sampling and decode,we will get the information we want. The procedure is shown as below:

![image-20221013100803060](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221013100803060.png)

![image-20221013100105734](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221013100105734.png)

Similar to pulse shaping, it is just the convolution of waveform and `matched filter coefficients` we just notice that, this time we should use another output of `MT Generate Filter Coefficients VI` to do convolution.

### Result

We should notice that, in order to reduce the inter symbol interference we should make sure the whole response function of the system is raised cosine function. And the result of two different kind of condition is shown as below:

Received Constellation with raised-cosine pulse shaping function and $\delta(t)$ matched filter function.

![image-20221014190243209](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221014190243209.png)

We can see that, the constellation is quite clear and SNR is over 250dB, so the result is quite good.

Received constellation with root square cosine pulse shaping function and root square cosine matched filtering function.

 ![image-20221014190546311](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221014190546311.png)

We can see that, in that case,the received symbol is not as stable as the previous one. And the SNR is about 44dB which is much smaller than the previous one. So the performance is not as good as the previous one.

### USRP Verification

​	USRP verification is done in this part. The following pictures are the results.

​	The following pictures are the plots of signal constellation and eye diagram under different factors of square root Raised-cosine function in real environment.

<center class="half">    
    <img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\afa 0 constellation.png" alt="afa 0 constellation" style="zoom:100%;" width="250"/>    
    <img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\afa 0 eye diagram.png" alt="afa 0 eye diagram" style="zoom:100%;" width="250"/> 
</center

<center style="color:;text-decoration:underline">&#945=0</center>

<center class="half">    
    <img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\afa 0.5.png" alt="afa 0.5" style="zoom:100%;"width="250"/>    
    <img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\afa 0.5 eye diagram.png" alt="afa 0.5 eye diagram" style="zoom:100%;" width="250"/> 
</center

<center style="color:;text-decoration:underline">&#945=0.5</center>

<center class="half">    
    <img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\afa 1 constellation.png" alt="afa 1 constellation" style="zoom:100%;"width="250"/>    
    <img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\afa 1 eye diagram.png" alt="afa 1 eye diagram" style="zoom:100%;" width="250"/> 
</center

<center style="color:;text-decoration:underline">&#945=1</center>

​	The following pictures are the plots of the bandwidth of signal under different factors of square root Raised-cosine function in real environment.

<img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\bandwidth afa=0.png" alt="bandwidth afa=0" style="zoom:60%;" />

<center style="color:;text-decoration:underline">&#945=0</center>

<img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\bandwidth afa=0.5.png" alt="bandwidth afa=0.5" style="zoom:60%;" />

<center style="color:;text-decoration:underline">&#945=0.5</center>

<img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\bandwidth afa=1.png" alt="bandwidth afa=1" style="zoom:60%;" />

<center style="color:;text-decoration:underline">&#945=1</center>

​	From  the above pictures and the theory learnt in the lab, what can be predicted is that the performance of transmission become better and better with $\alpha$ (the factor of square root Raised-cosine function) increasing. However, improvement of performance of transmission need to sacrifice bandwidth, which is the same as the idea of improving performance of transmission in $NBFM$ and $WBFM$. 

​	It is obvious to observe the performance of transmission under $\alpha=1$  is worse than that under  $\alpha=0.5$, which seems to violate the theory. In fact, the theory is correct. The reason of this phenomenon is that the noise power is time-varying, which means the noise power is different under the two situation. At that period, the noise power under  $\alpha=1$ is greater than that under $\alpha = 0.5$, which causes the phenomenon to happen.

###  Bandwidth with different SRRC factor

​	In USRP Verification part, the bandwidth with different SRRC factor in real environment is shown. So, the bandwidth with different SRRC factor in ideal environment will be talked in this part.

​	The formula of square root Raised-cosine function is :
$$
g_{sqrc}(t)=\frac{4\alpha}{\pi\sqrt{T}} \frac{cos[\frac{(1+\alpha)\pi t}{T}]+\frac{sin[\frac{(1-\alpha)\pi t}{T}]}{4\alpha t/T}}{1-(\frac{4\alpha t}{T})^2}
$$
​	The waveform of it in time domain and frequency domain is:

<center class="half">    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221014175203015.png" alt="image-20221014175203015" style="zoom:100%;"width="350"/>    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221014175227723.png" alt="image-20221014175227723" style="zoom:70%;" width="350"/> 
</center

​	From the time domain and frequency domain, what is obvious is that trailing attenuation becomes faster and faster and bandwidth needed becomes larger and larger with $\alpha$ increasing. Speed of trailing attenuation implies performance of transmission and bandwidth needed implies costs.  

### SINR under different SRRC factor 

​	The formula of SINR is:
$$
SINR=\frac{E_x|g(0)|^2}{N_0\int|G_{rx}(f)|^2df + E_x\sum_{m\neq0}|g(mT_s)|^2}
$$
​	
$$
g(0)=\int g_{rx}^*(-t)g_{tx}(t)dt
$$

$$
|g(0)|^2=|\int g_{rx}(-t)g_{tx}(t)dt|^2
$$


$$
\begin{align*}
|g(0)|^2&=|\int g_{rx}(-t)g_{tx}(t)dt|^2
\\
&\leqslant\int|g_{tx}(t)|^2dt\int|g_{rx}(t)|^2dt
\\
&=\int|g_{rx}(t)|^2dt

\end{align*}
$$
​	So the following condition can be gotten:
$$
g_{rx}(t)=g_{tx}^*(-t)
$$
​	In denominator, the first term means the noise power and the second term means the power caused by inter symbol interference. To minimize the second term and maximize the numerator, the function of pulse shaping and match filtering should meet $Nyquist $  $ISI$  $criterion$  and $g_{tx}(t)=g_{rx}(-t)$ . That is the reason why square root Raised-cosine is chosen.

​	So:
$$
g_{tx}(t)=g_{rx}(t)=g_{sqrc}(t)=\frac{4\alpha}{\pi\sqrt{T}} \frac{cos[\frac{(1+\alpha)\pi t}{T}]+\frac{sin[\frac{(1-\alpha)\pi t}{T}]}{4\alpha t/T}}{1-(\frac{4\alpha t}{T})^2}
$$
​	According to the theory,   when other conditions don't change, SINR  can reach its maximum when $\alpha$ of $g_{tx}(t)$ is equal to $\alpha$ of $g_{rx}(t)$.  The following picture is used to verify it.

​	SINR under different SRRC factor is shown as below:

<center class="half">    
    <img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\SNR（2）.png" alt="SNR（2）" style="zoom:100%;"width="400"/>    
    <img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\SNR（1）.png" alt="SNR（1）" style="zoom:100%;"width="400"/> 
</center

​	The above two lines are in ideal environment. When $\alpha$ of $g_{tx}(t)$ is equal to 0.5, SINR reach its maximum when $\alpha$ of $g_{rx}(t)$ is equal to 0.5, too. When $\alpha$ of $g_{tx}$ is equal to 1, SINR reach its maximum when  $\alpha$ of $g_{rx}(t)$ is equal to 1. 

​	The below two lines are in real environment using USRP. However, SINR don't reach its maximum when $\alpha$ of $g_{tx}(t)$ is equal to $\alpha$ of $g_{rx}(t)$ . The reason is that $N_0\int|G_{rx}(f)|^2df$  is time-varying and is different under different $\alpha$ of $g_{rx}(t)$ 

## Experience

1. I discovered how pulse shaping and matched filtering works.
2. I learnt who to reduce the inter symbol interference to improve SINR.
3. I learnt how to balance the performance and the consumption of resource.
4. I learnt the request of the best receiver.

### In class submission

汪海玉：

Week4:

![928_1](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/928_1.png)

![928_2](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/928_2.png)

![928_3](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/928_3.png)

Week5:

![1012](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/1012.png)

张旭东：

week4:

![submission](D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\submission.png)

![submission](D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\submission.png)

![submission3](D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\submission3.png)

![submission4](D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\submission4.png)

week5:

![submission5](D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab2\Report\submission5.png)