# Perceptron Learning Algorithm

先初始化$w_{(0)}$

for $t=0,1,...$

找到$w_{(t)}$错误的数据$(x_{i(t)},y_{i(t)})$：$sign(\tilde{w}_{(t)}^T\tilde{x}_{i(t)})\neq y_{i(t)}$

修正：$\tilde{w}_{(t+1)}\leftarrow\tilde{w}_{(t)}+y_{i(t)}\tilde{x}_{i(t)}$

直到没有错误

返回最终的$W$：$W_{PLA}$

![image-20211110194414111](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691078847.png)