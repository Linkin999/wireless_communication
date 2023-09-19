<h1 align = "center">Lab3 Symbol Synchronization</h1>

<center>汪海玉 12011331 &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp 张旭东 12011923 </center>

## Introduction

​	In lab2, we ignore the propagation delay to do pulse shaping and matched filtering. However, there are propagation delay in the real channel when the signal transmits. So, we will talk about the basic principle of symbol synchronization and two methods of calculating time delay to get the optimal sampling times.

### Basic principle of symbol synchronization

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221029234139265.png" alt="image-20221029234139265" style="zoom:67%;" />

​	In the real channel, there are propagation delay so the expression of the receive signal at the receiver can be expressed as following:
$$
z(t)=\alpha e^{j \phi}x(t-\tau_{d})+v(t)
$$
​	The diagram of symbol synchronization is shown below:

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221029235719186.png" alt="image-20221029235719186" style="zoom:67%;" />

​	In the previous experience, we use square-root raised cosine function  as the pulse function and matched filtering function, which makes the effect of inter symbol interference to reduce to the lowest level. And we do sampling at the maximum point. However, due to the time delay in communication system, the maximum point is shifted. For this, what is needed to do is symbol synchronization. In the next , two methods of symbol synchronization will be introduced, which are $Maximum$ $Energy$ $Algorithm$ and  $Early-Late$ $Gate$ $Algorithm$

### Maximum Energy Algorithm

​	The nature of $Maximum$ $Energy$ $Algorithm$ is to find the most appropriate $\tau_{d}$ to make the energy of received signal maximum. The received signal can be expressed as follow: 
$$
z(t)=\alpha e^{j \phi}\sqrt{E_{x}} \sum_{m}s[m]g_{tx}(t-mT-\tau_{d})+v(t)
$$


​	So the detected signal can be expressed as follow:
$$
\begin{align*}
y[n]&=h \sum_{m}s[m]g((n-m)T-\tau_{d})+v[n]
\\
&=hs[n]g(\tau_{d})+h\sum_{m\neq n}s[m]g((n-m)T-\tau_{d})+v[n]
\end{align*}
$$
​	The first term denotes the idea value, the second term denotes the effect of inter symbol interference and the third denotes the noise. So the expression of $y(t)$ is :
$$
y(t)=h \sum_{m=-\infty}^{+\infty}s[m]g(t-mT-\tau_{d})+v(t)
$$
​	The energy of sampled signal can be expressed as below:
$$
\begin{align*}
J(\tau)&=E[|y(nT+\tau)|^2]
\\
&=|h|^2 \sum_{m=-\infty}^{+\infty}|g(mT+\tau-\tau_{d})|^2+N_{0}
\\
&=|h|^2 \sum_{m=-\infty}^{+\infty}|g(mT+\tau_{frac}-\widehat\tau_{frac})|^2+N_{0}
\end{align*}
$$
​	Then let its value be maximum to find the most appropriate $\tau_{d}$, which makes the energy maximum.
$$
\begin{align*}
\widehat\tau_{d}&= arg\,\,\max_{\tau\in[0,T)}J(\tau)
\\
&=arg\,\,\max\sum_{i}r^2(kT+\tau_{d})
\end{align*}
$$

$$
E[|y(nT+\tau)|^2]\leqslant|h|^2|g(0)|^2+N_{0}
$$

​	The flowchart of finding the most appropriate $T_{d}$ is showing below:

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030005233403.png" alt="image-20221030005233403" style="zoom:67%;" />

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030005323994.png" alt="image-20221030005323994" style="zoom:67%;" />

​	The train of thought to find the most appropriate $\tau_{d}$ is to sample the received signal at the original point, which is the maximum point ignoring propagation delay with using square-root raised cosine function. Then do sample in the interval $\tau\in[0,T)$ and calculate energy of each. Then, the maximum point  is the point whose calculated energy is highest. 

### Early-Late Gate Algorithm

​	Another important method for finding the maximum point in the real channel with propagation delay is   $Early-Late$ $Gate$ $Algorithm$. From `Maximum Energy Algorithm`, the expression of sampled signal is known:
$$
J(\tau)=E[|y(nT+\tau)|^2]
=|h|^2 \sum_{m=-\infty}^{+\infty}|g(mT+\tau-\tau_{d})|^2+N_{0}
$$
​	The changing rate for it can be expressed as follow:
$$
\frac{\delta}{\delta k}J_{\delta}[k]\simeq \frac{1}{P}\sum^{P-1}_{n=1}2Re\left\{ \widetilde y[nP+k][\widetilde y^*[nP+k+\delta]-\widetilde y ^*[nP+k-\delta]]\right\}
$$
​	When the changing rate equals to $0$ ($\frac{\delta}{\delta k} J_{\delta}[k]=0$), the energy is maximum, which means the point is the maximum point.

​	The flowchart of finding the point where the changing rate equals to $0$ is showing below:

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030012304722.png" alt="image-20221030012304722" style="zoom:67%;" />

## Result and analysis

### Analysis of Maximum Energy Algorithm

#### Program

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030100602776.png" alt="image-20221030100602776" style="zoom: 50%;" />

​	In this part, the first thing is to set the initial time and do the down sampling. Then calculate the sum of square of the amplitude. After that, shift register is used to keep the maximum point. The value in the right shift register is always the maximum. The next structure will control the opening of symbol synchronization.

#### Result

<center class="half">    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030101523856.png" alt="image-20221030101523856" style="zoom:100%;" width="300"/>    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030101545168.png" alt="image-20221030101545168" style="zoom:100%;" width="300"/> 
</center

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030101623639.png" alt="image-20221030101623639" style="zoom: 25%;" />

​	From the above picture, what can be known is that the constellation result will diverge firstly and concentrate then with propagation delay increasing in the model where symbol synchronization closed. This phenomenon is caused by periods of samples.

<center class="half">    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030101725899.png" alt="image-20221030101725899" style="zoom:100%;" width="300"/>    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030101725899.png" alt="image-20221030101725899" style="zoom:100%;" width="300"/> 
</center

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030101818443.png" alt="image-20221030101818443" style="zoom: 25%;" />

​	The three pictures are totally different from the last three pictures. It is obvious that the constellation always concentrate with propagation delay increasing in the model where symbol synchronization is open, which means `Maximum Energy Algorithm` has a very good effect on symbol synchronization.

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030103224680.png" alt="image-20221030103224680" style="zoom:67%;" />

​	What's more, obviously, with $\alpha$ increasing, the proportion of total energy the energy of middle point occupies becomes larger and larger.

### Accuracy analysis

​	In this part, the relationship between oversample factor, delay and the sample rate will be discussed.

<center class="half">    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030104125987.png" alt="image-20221030104125987" style="zoom:100%;" width="300"/>    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030104150085.png" alt="image-20221030104150085" style="zoom:100%;" width="300"/> 
</center

​	What is easy to find is that the signal is just the same sample signal with propagation delay $0$ and $25μs$, which is correct according to the theory.

<center class="half">    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030104837878.png" alt="image-20221030104837878" style="zoom:100%;" width="300"/>    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030104919127.png" alt="image-20221030104919127" style="zoom:100%;" width="300"/> 
</center

​	The period of symbol synchronization is:
$$
T=\frac{4}{4M}=\frac{10}{10M}=1μs
$$
​	Next, trend of symbol synchronization accuracy with the change of oversample factor will be discussed when  the period of sampled signal is fixed.

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221030110138563.png" alt="image-20221030110138563" style="zoom:67%;" />

​	With the oversample factor increasing, the error statistic become smaller and smaller, which is correct according to the analysis.

## Experience

1. Understanding of  basic principle of symbol synchronization
2. Master of `Maximum Energy Algorithm` and understanding of `Early-Late Gate Algorithm`
3. Master of programming of `Maximum Energy Algorithm`  

### In-class submission

张旭东：

<center class="half">    
    <img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab3\report\submission1.JPG" alt="submission1" style="zoom:100%;" width="300"/>    
    <img src="D:\Study in SUSTech\First semester of junior year\wireless communication\lab\lab3\report\submission2.JPG" alt="submission2" style="zoom:100%;" width="300"/> 
</center

