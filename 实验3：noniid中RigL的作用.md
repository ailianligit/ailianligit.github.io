消融实验

超参数变化

噪声数据对超参数xuan'ze

--distribution iid dirichlet
--beta 0.1 0.5

--sparsity 0.5

--need-readjust
--readjustment-ratio 0.0 0.1 0.3 0.5 0.7 0.9

--outfile
--device



python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.5 --need-readjust --readjustment-ratio 0.7 --outfile cifar10_05_dirichlet_01_readjust_07_c.csv --device

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.5 --need-readjust --readjustment-ratio 0.01 --outfile cifar10_05_dirichlet_01_readjust_001_b.csv --device



python fedavg.py --distribution iid --sparsity 0.5 --need-readjust --readjustment-ratio 0.3 --outfile cifar10_05_iid_readjust_03_a.csv --device

python fedavg.py --distribution iid --sparsity 0.5 --need-readjust --readjustment-ratio 0.5 --outfile cifar10_05_iid_readjust_05_a.csv --device

python fedavg.py --distribution iid --sparsity 0.5 --need-readjust --readjustment-ratio 0.7 --outfile cifar10_05_iid_readjust_07_a.csv --device

python fedavg.py --distribution iid --sparsity 0.5 --need-readjust --readjustment-ratio 0.9 --outfile cifar10_05_iid_readjust_09_a.csv --device

| 调整率 | 0.0           | 0.01          | 0.1           | 0.3       | 0.5           | 0.7       | 0.9           |
| ------ | ------------- | ------------- | ------------- | --------- | ------------- | --------- | ------------- |
| 0.1    | **0.52653** a | 0.139580001 a | **0.42905** a | 0.3969 b  | 0.41441 a     | 0.29787 b | 0.427239993 a |
| 0.5    | **0.61985** a | 0.61612 a     | **0.61665** a | 0.59643 a | 0.600710005 a | 0.60968 a | **0.6187** a  |
| iid    | **0.66816** a | **0.6625** a  | 0.655079997 a |           |               |           |               |

