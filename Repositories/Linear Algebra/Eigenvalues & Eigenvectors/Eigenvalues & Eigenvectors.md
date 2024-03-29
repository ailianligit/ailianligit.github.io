特征向量：在线性变换过程中方向保持不变的向量

如果基向量都是特征向量，代表基向量在线性变换过程中方向不变，由于线性变换后的基向量构成矩阵的列向量，因此（线性变换）矩阵是对角矩阵，列向量为特征向量，矩阵对角线上的值为特征值。

$Q$的列向量是$A$的特征向量，线性变换的顺序从右到左。首先，特征向量基上的某向量$v$经$Q$转换为原基上的向量，在原基上做线性变换$A$，最后原基上的向量经$Q^{-1}$转换为特征向量基上的向量。总体上看是在特征向量基上作线性变换。
$$
Q^{-1}AQ=\Lambda
$$
如果写成下式的形式，则表示矩阵的特征分解。可用于求逆矩阵和矩阵的幂，都只需要对对角矩阵求逆或者求幂。
$$
A=Q\Lambda Q^{-1}
$$
任意的N*N实对称矩阵的特征值都是实数且都有N个线性无关的特征向量（正交）。其中Q是正交矩阵，对角矩阵为实对角矩阵。
$$
A=Q\Lambda Q^{-1}=Q\Lambda Q^{T}
$$
线性变换的线性性质（加法和数乘性质）保证了矩阵乘法能够代表向量的线性变换，因为线性变换后的向量是对新基向量（矩阵的列向量）做加法和数乘运算。

| Linear Algebra         | Functions        |
| ---------------------- | ---------------- |
| Linear Transformations | Linear Operators |
| Dot Products           | Inner Products   |
| Eigenvectors           | Eigenfunctions   |

列空间（秩）+零空间
