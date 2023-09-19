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

$$ {begin{align*}
|g(0)|^2=|\int g_{rx}(-t)g_{tx}(t)dt|^2
$$ {end{align*}


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

​	The below two lines are in real environment using USRP. However, SINR don't reach its maximum when $\alpha$ of $g_{tx}(t)$ is equal to $\alpha$ of $g_{rx}(t)$ . The reason is that $N_0\int|G_{rx}(f)|^2df$  is time-varying and is different under different $\alpha$ of $g_{rx}(t)$ .

## Experience