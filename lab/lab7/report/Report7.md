<h1 align = "center">Lab7 LPDC Code</h1>

## Introduction

In previous lab, we are focusing on how to reduce the error rate during the transmission. In this lab, we are going to introduce a new way to reduce the error rate with proper coding and encoding. 

### Shannon–Hartley theorem

For any given degree of [noise contamination of a communication channel, it is possible to communicate discrete data digital information nearly error-free up to a computable maximum rate through the channel. 

And the way to achieve that is to apply Shannon coding theorem:

1. The code should be random.
2. The coding word should be infinite long.
3. Use maximal-likelihood Decode.

But in the real world, we can let the coding word to be infinite long, so we need to do a degree of approximation.

### Hamming encoding

(7,4) Hamming encoding is a typical kind of encoding, which encodes 4 bits to 7 bits. The mapping table is as shown below:

![image-20221223201513983](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221223201513983.png)

And the redundant bits are generated by the equation:
$$
\begin{cases}
	C_2=C_6+C_5+C_4\\
	C_1=C_5+C_4+C_3\\
	C_0=C_6+C_5+C_3\\
\end{cases}
$$
And this process can be expressed by linear algebra, and it is  so called "Generator Matrix".

### Generator matrix and check matrix

Now, if we use a bit vector to represent the transmitted bits, and a matrix G to represent the transform, the result will be the encoded bits, and the matrix is generator matrix. It can be expressed as:
$$
m_{\mathrm{transmitted}\  \mathrm{bits}}\times G=C_{\mathrm{word}}
$$
The generator matrix for (7,4) Hamming encoding is:
$$
G=\left[ \begin{matrix}
	1&		0&		0&		0&		1&		0&		1\\
	0&		1&		0&		0&		1&		1&		1\\
	0&		0&		1&		0&		1&		1&		0\\
	0&		0&		0&		1&		0&		1&		1\\
\end{matrix} \right]
$$
Now suppose the transmitted bits are "0101", the process can be expressed as:
$$
\left[ \begin{matrix}
	0&		1&		0&		1\\
\end{matrix} \right] \times \left[ \begin{matrix}
	1&		0&		0&		0&		1&		0&		1\\
	0&		1&		0&		0&		1&		1&		1\\
	0&		0&		1&		0&		1&		1&		0\\
	0&		0&		0&		1&		0&		1&		1\\
\end{matrix} \right] =\left[ \begin{matrix}
	0&		1&		0&		1&		1&		0&		0\\
\end{matrix} \right]
$$
And if we want to check the correctness of the entire bit stream. We need to use use another matrix so called check matrix. It can be regard as kind of "reverse" of the generator matrix. The generator matrix is not full rank, but it has full row rank. So it is "left reverse" to be more exactly.
$$
S^T=H\times R^T=\left[ \begin{array}{c}
	s_2\\
	s_1\\
	s_0\\
\end{array} \right] =\left[ \begin{array}{c}
	0\\
	0\\
	0\\
\end{array} \right]
$$

### Tanner Graph

Tanner graph can be used as a tool to see the check process more directly. If the syndrome values are related to the coding word, we will draw a line to attach them, the final result may show as below:

![image-20221224193309184](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221224193309184.png)

By looking at this graph we can clearly find the relation between coding word and syndrome values. And if syndrome values are not all zeros, we can use it to detect the error bit.

### Low-density parity-check (LDPC)

LDPC is another kind of encode method. In short, it is to maintain a certain parity of the entire bit flow through control bits. 

Regular LDPC has the following properties:

+ As for a $m\times n$ matrix of a LDPC:
  1. The number of 1’s in any column (the row weight 𝑤𝑟), is much less than
     row-length (𝑤𝑟 << 𝑚).
  2. The number of 1’s in any row (the column weight 𝑤𝑐), is much less than
     column-length (𝑤𝑐 << 𝑛).
  3. 𝑤𝑐 is constant for every column, 𝑤𝑟 is constant for every row and $\frac{w_r}{m}=\frac{w_c}{n}$.

Here is an example of LDPC matrix (8,2,4)

 $H=\left[ \begin{matrix}
	0&		1&		0&		1&		1&		0&		0&		1\\
	1&		1&		1&		0&		0&		1&		0&		0\\
	0&		0&		1&		0&		0&		1&		1&		1\\
	1&		0&		0&		1&		1&		0&		1&		0\\
\end{matrix} \right] $

#### Loops in LDPC

If we draw the tanner graph of LDPC matrix (8,2,4), we will find that there is a lot of "loops" in the graph:

![image-20221224194834043](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221224194834043.png)

As the image shown above, if c~3~ and c~6~ shift at the same time, we will not be able to detect this problem. In other word, the tolerable bit error rate is not more than 1/8 for this case. If the matrix is bigger, we will find more word bit will be involved in that bigger loop, which means it has better error tolerance.

## Experiment And Analysis

### Hamming Code

In the class, we only handle with LDPC code, so we need to add two `vi` to make it support Hamming code. The two `vi` are `MT Hamming Encoder VI` and `MT Hamming Decoder VI`. As for (7,4) Hamming, the `hamming order` is 3.

So The whole programming diagram is as shown below:

![image-20221224202220303](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221224202220303.png)

To evaluate the performance of hamming code, we can compare it with the one that do not use any code methods. The first graph is do not use any code method, and the second one use (7,4) hamming code:

![image-20221224202445091](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221224202445091.png)


![image-20221224202514103](https://smangic-markdown-image.oss-cn-shenzhen.aliyuncs.com/img/image-20221224202514103.png)

Actually, the error rate of Hamming code is zero most of time. Sometimes, it will jump to a certain value, but, it is still much smaller than the one do not use any code technique. We can say that (7,4) hamming code can increase the quality of communication.

### LDPC code

In this part, we will use the Hamming Code method to achieve a simulate simple LDPC AWGN channel demo.

So The whole programming diagram is as shown below:

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221229164028280.png" alt="image-20221229164028280" style="zoom:67%;" />

To evaluate the performance of hamming code, we can compare it with the one that do not use any code methods. 

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221229164325889.png" alt="image-20221229164325889" style="zoom:67%;" />

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221229164405501.png" alt="image-20221229164405501" style="zoom:67%;" />

![image-20221229165048606](C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221229165048606.png)

<img src="C:\Users\胡晨\AppData\Roaming\Typora\typora-user-images\image-20221229165120993.png" alt="image-20221229165120993" style="zoom:67%;" />

From the four pictures, what is obvious is that the result using LDPC coding technique is much better than that using no coding technique for the average bit-error rate of using LDPC coding is less than that of using no code technique and received symbols of using LDPC coding  is much more concentrated than that of using no code technique.

## Experience

1. Shannon–Hartley theorem
2. the principle of generator matrix, check matrix, tanner graph and loops in LDPC
3. the principle of the Hamming coding together with their mathematical analysis
4. the principle of the LDPC coding together with their mathematical analysis