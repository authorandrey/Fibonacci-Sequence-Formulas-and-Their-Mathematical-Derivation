# Table of Contents
- [Intuition](#intuition)
- [Complexity](#complexity)
- [Approach](#approach)
  - [Problem](#statement-of-the-problem)
  - [Solving the problem](#solving-the-problem)
    - [Diagonalization](#diagonalization)
    - [Final Formula](#final-formula)
  - [Optimization](#optimization)
    - [More reduction](#more-reduction)
  - [Easiest solution](#easiest-solution)
 - [Conclusion](#conclusion)
# Intuition
The Fibonacci sequence can be computed without recursion or iteration. Using a direct mathematical relationship between the sequence index and its value, allowing for constant-time computation. 

# Complexity
- Time complexity: $$O(1)$$

# Approach
## Statement of the problem
We have the problem:
Given fibonacci recurence with starting values
$$
\begin{cases}
\varphi_n = \varphi_{n-1} + \varphi_{n-2}\\
\varphi_0 = 0\\
\varphi_1 = 1
\end{cases}
$$
The goal is to compute $\varphi_n$ by given $n\in\mathbb{N}_0$.
There is a well-known [matrix form](https://en.wikipedia.org/wiki/Fibonacci_sequence#Matrix_form) for this problem:
$$
\begin{pmatrix}
\varphi_n \\ \varphi_{n-1}
\end{pmatrix} = 
\begin{pmatrix}
1 & 1 \\ 1 & 0
\end{pmatrix}
\begin{pmatrix}
\varphi_{n-1} \\ \varphi_{n-2}
\end{pmatrix}
$$
Let:
$$
A := \begin{pmatrix}
1 & 1 \\ 1 & 0
\end{pmatrix}
$$
Then:
$$
\begin{pmatrix}
\varphi_n \\ \varphi_{n-1}
\end{pmatrix} = 
A^{n-1}
\begin{pmatrix}
1 \\ 0
\end{pmatrix}
$$

## Solving the problem
### Diagonalization
The problem reduces to diagonalizing matrix $A$ to easily compute $A^n$:
1) Characteristic polynomial: $\chi_A = \lambda^2 - \lambda - 1 \quad (I_1 = \operatorname{tr}A = 1, I_2 = \operatorname{det}A=-1, \chi_A = (-1)^n\lambda^n+(-1)^{n-1}I_1\lambda^{n-1} + \dots + I_n)$, where $I_i$ are principal invariants of matrix.


2) Eigenvalues($\chi_A=0$): $$\lambda_1 = \dfrac{1+\sqrt 5}{2}, \lambda_2 = \dfrac{1-\sqrt 5}{2}$$


3) Eigenvectors($$(A-\lambda E)v=0$$): 
$$
\begin{pmatrix}
1-\lambda & 1 \\ 1 & -\lambda
\end{pmatrix}
\begin{pmatrix}
x \\ y
\end{pmatrix} = 
\begin{pmatrix}
0 \\ 0
\end{pmatrix}\Rightarrow
\begin{cases}
x - \lambda x + y = 0\\
x - \lambda y = 0
\end{cases} \Rightarrow
\begin{cases}
x = \lambda x + y\\
\lambda^2y-\lambda y - y = 0
\end{cases}
$$
If $y=0$ then $x=0$ which is not a valid eigenvector. Also $\lambda^2-\lambda - 1 = 0$ we got from eigenvalues. So:
$
\begin{pmatrix}
x \\ y
\end{pmatrix} = 
\begin{pmatrix}
\lambda \\ 1
\end{pmatrix}
$ are eigenvectors for $\lambda \in\{\lambda_1, \lambda_2\}$


4) Diagonalization:
$A = P\Lambda P^{-1}$, where 
$$
\Lambda = 
\begin{pmatrix}
\lambda_1 & 1 \\ 1 & \lambda_2
\end{pmatrix},
P = 
\begin{pmatrix}
\lambda_1 & \lambda_2 \\ 1 & 1
\end{pmatrix}
$$
Eigenvectors are not orthogonal so we can't use $$P^{-1} = P^T$$. So let's calculate $$P^{-1}$$:
$$\operatorname{det}P = \lambda_1 - \lambda_2 = \dfrac{1+\sqrt 5}{2} - \dfrac{1-\sqrt 5}{2} = \sqrt5$$
$$
P^{-1} = \dfrac{1}{\sqrt5}
\begin{pmatrix}
1 & -\lambda_2 \\ -1 & \lambda_1
\end{pmatrix}
$$


5) $A^n = P\Lambda P^{-1}P\Lambda P^{-1}\dots P\Lambda P^{-1} = P\Lambda^nP^{-1}$

### Final formula
As result we can derive $\varphi_n$:
$$
\begin{pmatrix}
\varphi_n \\ \varphi_{n-1}
\end{pmatrix} = A^{n-1}
\begin{pmatrix}
1 \\ 0
\end{pmatrix} = \dfrac{1}{\sqrt5}
\begin{pmatrix}
\lambda_1 & \lambda_2 \\ 1 & 1
\end{pmatrix}
\begin{pmatrix}
\lambda_1^{n-1} & 1 \\ 1 & \lambda_2^{n-1}
\end{pmatrix}
\begin{pmatrix}
1 & -\lambda_2 \\ -1 & \lambda_1
\end{pmatrix}
\begin{pmatrix}
1 \\ 0
\end{pmatrix}
$$
Simplifying:
$$ = \dfrac{1}{\sqrt5}
\begin{pmatrix}
\lambda_1 & \lambda_2 \\ 1 & 1
\end{pmatrix}
\begin{pmatrix}
\lambda_1^{n-1} & 1 \\ 1 & \lambda_2^{n-1}
\end{pmatrix}
\begin{pmatrix}
1 \\ -1
\end{pmatrix} = \dfrac{1}{\sqrt5}
\begin{pmatrix}
\lambda_1 & \lambda_2 \\ 1 & 1
\end{pmatrix}
\begin{pmatrix}
\lambda_1^{n-1} \\ -\lambda_2^{n-1}
\end{pmatrix} = \dfrac{1}{\sqrt5}
\begin{pmatrix}
\lambda_1^n - \lambda_2^n \\ \lambda_1^{n-1} - \lambda_2^{n-1}
\end{pmatrix}
$$
Therefore:
$$
\boxed{\varphi_n = \dfrac{1}{\sqrt5}(\lambda_1^n - \lambda_2^n)}
$$
We can reduce $$\lambda_2$$:
$$\lambda_2 = \dfrac{1 - \sqrt5}{2} = \dfrac{1 - \sqrt5}{2}\dfrac{1 + \sqrt5}{1 + \sqrt5} = \dfrac{-4}{2(1+\sqrt5)} = -\dfrac{1}{\lambda_1}$$
As result we get:
$$
\boxed{\varphi_n = \dfrac{1}{\sqrt5}(\lambda_1^n+(-1)^{n+1}\lambda_1^{-n})}
$$
This leads to the first version of program:
```cpp []
#include <cmath>

class Solution {
public:
    double sqrt_5 = sqrt(5);
    double fib_const = (1 + sqrt_5) / 2;

    int fib(int n) {
        int sign = (n%2==0 ? -1 : 1);
        return (1 / sqrt_5) * (pow(fib_const, n) + sign * pow(fib_const, -n));
    }
};
```
It already runs in ```0 ms``` on LeetCode, but memory usage is relatively high (beats only ```21%```of solutions for memory usage)

## Optimization
Let $\varphi := \lambda_1 = \dfrac{1+\sqrt5}{2}$ (the golder ratio).
1) There is an interesting fact that $\varphi^2 = \dfrac{(1+\sqrt5)^2}{4} = \dfrac{1+2\sqrt5+5}{4} = \varphi+1$
As result by induction we can prove that $\varphi^n=\varphi_n\varphi+\varphi_{n-1}$:
$$\varphi^1=\varphi_1\varphi+\varphi_0=1\cdot\varphi+0=\varphi$$
$$\varphi^{n+1}=\varphi\varphi^n=\varphi_n\varphi^2+\varphi_{n-1}\varphi=(\varphi_n+\varphi_{n-1})\varphi+\varphi_n=\varphi_{n+1}\varphi+\varphi_n$$
But this lead to self-reference: to compute $\varphi_n$ we must compute $\varphi_n$.
2) You might ask: "Why not use `static` or `constexpr`?". The answer is that `sqrt` becomes `constexpr` only in `C++26` and for `static` we need `constexpr`.
3) There are some algorithms that I don't cover here. For example  optimizing `pow` and $\dfrac{1}{\sqrt5}$. One of some ways to optimize $\dfrac{1}{\sqrt5}$ is to use well-known [Quake III inverse square root algorithm](https://en.wikipedia.org/wiki/Fast_inverse_square_root#Overview_of_the_code), though it would need adaptation. For `pow` optimization you can use the fact that $$\varphi^2=\varphi+1$$. But this text is made to cover mathematical aspects so such optimizations are left to the reader.

### More reduction
We can go even further by eliminating $\lambda_2$.
Let $\psi := \lambda_2 = \dfrac{1-\sqrt5}{2}$; $\xi_n := \varphi_n - \dfrac{\varphi^n}{\sqrt5}$ - solution error.
Therefore $\xi_n = -\dfrac{\psi^n}{\sqrt5} = (-1)^{n+1}\dfrac{\varphi^{-n}}{\sqrt5}$
Notice that $\varphi>1$ and $\varphi^{-n}\xrightarrow {n\to\infty} 0$
So $||\xi_n||_\infty = \max_n |\xi_n| = \max_n \dfrac{\varphi^{-n}}{\sqrt5} = |\xi_0| = \dfrac{1}{\sqrt5} < 0.5$
We are planning to reduce $$\psi$$. To do this we need to prove that it doesn't have any impact on some transition of the previous solution. We have the fact that error is $<0.5$. And the ```round``` function approximates it to $0$.
Therefore we have $\operatorname{round}\left|\varphi_n-\dfrac{\varphi^n}{\sqrt5}\right|=\operatorname{round\left|\dfrac{\psi^n}{\sqrt5}\right|}=0$. And from it follows that with ```round``` $\dfrac{\psi^n}{\sqrt5}$ doesn't give any impact.
As result we get $$\boxed{\varphi_n=\operatorname{round}\left(\dfrac{\varphi^n}{\sqrt5}\right)}$$
```cpp []
#include <cmath>

class Solution {
public:
    const double sqrt_5 = sqrt(5);
    const double fib_const = (1 + sqrt_5) / 2;

    int fib(int n) {
        return round(pow(fib_const, n) / sqrt_5);
    }
};
```
As previous solution it runs in `0 ms` on LeetCode, but it's theoretically faster. On the other hand the memory consumption remains similar.

## Easiest solution
Finally, here's the simplest possible solution. No advanced math or complex programming is needed.
LeetCode gives a function with signature:
```cpp []
int fib(int n)
```
So we can use bounds of type $$int$$. It's maximum value is `2147483647`. We can precompute all Fibonacci numbers that fit within this limit. The values can be looked up[here](https://www.math.net/list-of-fibonacci-numbers)):
```cpp []
#include <cmath>

constexpr int fibonacci[47] = {
    0, 1, 1, 2, 3, 5, 8, 13, 21, 34,
    55, 89, 144, 233, 377, 610, 987, 
    1597, 2584, 4181, 6765, 10946, 17711,
    28657, 46368, 75025, 121393, 196418,
    317811, 514229, 832040, 1346269, 2178309,
    9227465, 14930352, 24157817, 39088169,
    63245986, 102334155, 165580141, 267914296,
    433494437, 701408733, 1134903170, 1836311903
};

class Solution {
public:
    int fib(int n) {
        return fibonacci[n];
    }
};
```
This solution completes in ```0 ms``` and uses ```~7.74
MB``` (that beats ```64%``` of solutions). While it may seem like cheating, simplicity often leads to the best results.

# Conclusion
Is summary, we've explored some methods to compute Fibonacci numbers. The mathematical derivation demonstrates the beautiful connection between linear algebra and number theory. For LeetCode-style problems where n ≤ 46, the lookup table offers the best combination of speed and simplicity. However, understanding the underlying mathematics provides the knowledge about how the whole foundation is structured.
