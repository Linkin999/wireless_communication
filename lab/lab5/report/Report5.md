<h1 align = "center">Lab5 Channel Estimation and Equalization</h1>

<center>汪海玉 12011331 &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp 张旭东 12011923 </center>

## Introduction

In the past experiment, we simply use AWGN channel to simulate our communication channel, which means the impulse response of our channel is $\delta(t)$, which is impossible. Now we are going to discuss about wide-band channel and the  impulse response of the channel can be write as:
$$
h\left( t \right) =\alpha _0e^{j\varphi _0}\delta \left( t-\tau _0 \right) +\alpha _1e^{j\varphi _1}\delta \left( t-\tau _1 \right)
$$
And the received signal is:
$$
z\left( t \right) =\alpha _0e^{j\varphi _0}x\left( t-\tau _0 \right) +\alpha _1e^{j\varphi _1}x\left( t-\tau _1 \right) +v\left( t \right) 
$$
This time, our received signal may suffer inter-symbol interference(ISI).

![image-20221123201448926](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221123201448926.png)

So we hope to use some mathematic method to resolve the signal. That is channel estimation and equalization. 

### Channel estimation

Let's recall the structure of the packet.

| Packet Head | Training sequences | Key bits sequences | Guard band |
| ----------- | ------------------ | ------------------ | ---------- |

Before doing channel estimation, let's recall some knowledge of  linear algebra. For an matrix equation $Ax = b$($A$,$x$ and $b$ are matrixes): when the inverse of $A$ exists, the equation can be solved.
$$
x = A^{-1}b
$$
According to this, the matrix which can make the value of $min||Ax-b||^{2}$ minimize can be expressed as follows:
$$
x_{LS}=(A^{*}A)^{-1}A^{*}b
$$
In the channel estimation, the transmitted signal can be expressed as follows:
$$
s[n]=t[n], n=0,1,..,N_{t}-1
$$
The received signal can be expressed as follows:
$$
y[n]=\sum_{l=0}^{L}s[l]h[n-l]+v[n]
$$
where $h[n]$ means the impulse response of the channel and $v[n]$ means the noise.

The purpose of channel estimation is to make the influence of noise minimize, so what needed to do is to estimate the impulse response of channel, denoted by $\widehat{h}(t)$,which satisfy the following expression:
$$
{\widehat{h}[0],\widehat{h}[1],...,\widehat{h}[L]}\\
=arg\,\,\min_{h[0],h[1],...,h[L]} \sum_{n=L}^{N_{t}-1}||y[n]-\sum_{l=0}^{L}h[l]t[n-l]||^2
$$
The matrix form of $y[n]$ ($n=L,L+1,...,N_{t}-1$) is as below:
$$
\left[ \begin{matrix}
	{y}\left[ L\right]\\
	{y}\left[ L+1 \right]\\
	\vdots\\
	{y}\left[ N_{t}-1 \right]\\
	
\end{matrix} \right] =
\left[ \begin{matrix}
	{t}\left[ L \right]&				\cdots&		t\left[ 0 \right]\\
	{t}\left[ L+1 \right]&				\cdots&		t\left[ 1 \right]\\
	\vdots&		\ddots&		\ddots&		\vdots\\
	{t}\left[ N_{t}-1 \right]&				\cdots&		t\left[ N_{t}-1-L \right]\\			
\end{matrix} \right]
\left[ \begin{matrix}
	{h}\left[ 0\right]\\
	{h}\left[ 1 \right]\\
	\vdots\\
	{h}\left[ L \right]\\
\end{matrix} \right]+
\left[ \begin{matrix}
	{v}\left[ L\right]\\
	{v}\left[ L+1 \right]\\
	\vdots\\
	{v}\left[ N_{t}-1 \right]\\
	
\end{matrix} \right]
$$
Further, the expression can be written in this form:
$$
y=Th+v
$$
where $y$, $T$, $h$ and $v$ is the representation of corresponding matrix. If the inverse of $T$ exists, then $h_{LS}$ which satisfy formula $(7)$ can be expressed as follows:
$$
h_{LS} =(T^*T)^{-1}T^*y
$$
  According to linear algebra: if a matrix is a full rank matrix, then its inverse must exist. The condition which makes the inverse of $T$ exist is :
$$
N_{t}-L>=L+1\\
N_{t}>=2L+1
$$

### Channel equalization

After channel estimation, we get the estimated channel $\widehat{h}\left( t \right) $. Suppose the system function of equalizer is $f_{n_d}$, and after the equalizer we will get $ \widehat{s}$ the process can be expressed as:
$$
z\left[ n \right] =\sum_{l=0}^{L_{\mathrm{f}}}{f_{\mathrm{n}_{\mathrm{d}}}\left[ l \right] y\left[ n-l \right] \approx \widehat{s}\left[ n-n_{\mathrm{d}} \right]}
$$
Applying the equation we get in the channel estimation, we will find:
$$
\sum_{l=0}^{L_{\mathrm{f}}}{f_{\mathrm{n}_{\mathrm{d}}}\left[ l \right] \widehat{h}\left[ n-l \right] \approx \widehat{\delta }\left[ n-n_{\mathrm{d}} \right]}
$$
So the main problem is to use matrix to represent convolution, and the matrix is known as Toeplitz matrix.

#### Toeplitz matrix

The structure of Toeplitz matrix is shown as below:
$$
\left[ \begin{matrix}
	\widehat{h}\left[ 0 \right]&		0&		\cdots&		\cdots\\
	\widehat{h}\left[ 1 \right]&		\widehat{h}\left[ 0 \right]&		0&		\cdots\\
	\vdots&		\ddots&		\ddots&		\vdots\\
	\widehat{h}\left[ L \right]&		\widehat{h}\left[ L-1 \right]&		\cdots&		\cdots\\
	\vdots&		\widehat{h}\left[ L \right]&		\vdots&		\vdots\\
\end{matrix} \right]
$$
By using this matrix we can use some techniques in linear algebra to solve $f_{n_d}$. The process is shown as below:

First we can rewrite the equation above in a matrix way:
$$
\left[ \begin{matrix}{}
	\widehat{h}\left[ 0 \right]&		0&		\cdots&		\cdots\\
	\widehat{h}\left[ 1 \right]&		\widehat{h}\left[ 0 \right]&		0&		\cdots\\
	\vdots&		\ddots&		\ddots&		\vdots\\
	\widehat{h}\left[ L \right]&		\widehat{h}\left[ L-1 \right]&		\cdots&		\cdots\\
	\vdots&		\widehat{h}\left[ L \right]&		\vdots&		\vdots\\
\end{matrix} \right] \left[ \begin{array}{l}
	f\left[ 0 \right]\\
	f\left[ 1 \right]\\
	\vdots\\
	\vdots\\
	f\left[ L_{\mathrm{f}} \right]\\
\end{array} \right] =\left[ \begin{array}{l}
	0\\
	\vdots\\
	1\\
	\vdots\\
	0\\
\end{array} \right]
$$
Then we successfully change it into a solvable problem, the best estimation of $f_{n_d}$ can be solved by $\widehat{f}_{\mathrm{n}_{\mathrm{d}}}=\left( \widehat{H}^*\widehat{H} \right) ^{-1}\widehat{H}^*e_{\mathrm{n}_{\mathrm{d}}}$.

We can find the element of every column is just the same, but it appears in different position. So I think we can use **shift register** to implement this part, and the programming diagram will be shown in the following part.

## Experiment and Analysis

### Program

#### Channel estimation

The program is shown below.

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127165029639.png" alt="image-20221127165029639" style="zoom:67%;" />

First, the transpose of $A$ ,$A^*$ is gotten. Then, compute $(A^*A)^{-1}$ and $A^*b$. Last, compute $(A^*A)^{-1}A^*b$ and $x_{LS}$ is gotten.

#### Channel equalization

Different from the method shown in the class, I choose to use `shift register` and `Rotate 1D Array` to implement `Toeplitz`. 

![image-20221123211728995](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221123211728995.png)

The basic idea is that we can right shift the column step by step, and store each step in a new matrix, after serval round, which is determined by the length of row, we will get the Toeplitz matrix.

### Analysis

The configuration information is shown as below:

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127171430947.png" alt="image-20221127171430947" style="zoom:67%;" />

<center class="half">    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127171057466.png" alt="image-20221127171057466" style="zoom:67%;" width="300"/>    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127171249700.png" alt="image-20221127171249700" style="zoom:67%;" width="300"/> 
</center

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127171401185.png" alt="image-20221127171401185" style="zoom:67%;" />

The equalizer length is increased from $1$ to $6$. The constellation is shown below:

<center class="half">    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127171807593.png" alt="image-20221127171807593" style="zoom:100%;" width="300"/>    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127171906937.png" alt="image-20221127171906937" style="zoom:100%;" width="300"/> 
</center

<center>length=1 &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp length=2 </center>

<center class="half">    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127171953634.png" alt="image-20221127171953634" style="zoom:100%;" width="300"/>    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127172138861.png" alt="image-20221127172138861" style="zoom:100%;" width="300"/> 
</center

<center>length=3 &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp length=4 </center>

<center class="half">    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127172538935.png" alt="image-20221127172538935" style="zoom:100%;" width="300"/>    
    <img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221127172619855.png" alt="image-20221127172619855" style="zoom:100%;" width="300"/> 
</center

<center>length=5 &nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp length=6 </center>

From the above pictures, what is obvious is that under the same other conditions, the longer the equalizer length is , more concentrated points on the constellation, which means the transmission quality of the system is becoming better. Another point need to be considered is that when the equalizer length is less than the value of channel estimate length, the points on constellation map are more concentrated with the equalizer length increasing. However, when the equalizer length isn't less than the value of channel estimate length, the points on constellation map do not change significantly with the equalizer length increasing. In other words, channel estimate length is the critical length of the equalizer length, beyond which the sign of the constellation does not change significantly, and at this time the sign is basically at the position of the emission point without any dispersion.

According to the theory, when the equalizer length is less than the channel estimate length, the effect of noise is becoming less and $SNR$ is becoming larger with equalizer length increasing. When the equalizer length isn't less than the channel estimate length, the effect of noise is minimized and $SNR$ is almost unchanged. What confused me is that with increasing the equalizer length, $SNR$ is almost unchanged, which is not in line with theory. In my opinion, the predictions from theory is correct and the occurrence of this phenomenon is related to the fluctuations of $SNR$. When all the conditions are fixed, the value of $SNR$ fluctuates within a certain range.



## USRP Verification





## Experience

1. Least mean square algorithm for channel estimation
2. The matrix form of convolution
3. The construction of  Toeplitz matrix
4. Indirect channel equalization algorithm
5. The effect of equalizer length on the performance of channel equalization
6. The relation between equalizer length and SNR  