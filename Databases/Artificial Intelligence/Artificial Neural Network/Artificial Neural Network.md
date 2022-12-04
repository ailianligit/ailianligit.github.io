# Artificial Neural Network

## Forward Network Activation

$I_j=\sum_iw_{ij}O_i+\theta_j$

$O_j=\frac{1}{1+e^{-I_j}}$

## Backward Error Propagation

$Err_k=O_k(1-O_k)(T_k-O_k)$

$Err_j=O_j(1-O_j)\sum_kErr_kw_{jk}$

$w_{jk}=w_{jk}+\eta Err_kO_j$

$\theta_k=\theta_k+\eta Err_k$