消融实验

超参数变化

--distribution iid dirichlet
--beta 0.1 0.5

--sparsity 0.5

--need-readjust
--readjustment-ratio 0.0 0.1 0.3 0.5 0.7 0.9

--outfile
--device

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.5 --need-readjust --readjustment-ratio 0.1 --outfile cifar10_05_dirichlet_01_readjust_01_a.csv --device 2

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.5 --need-readjust --readjustment-ratio 0.3 --outfile cifar10_05_dirichlet_01_readjust_03_a.csv --device 3

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.5 --need-readjust --readjustment-ratio 0.5 --outfile cifar10_05_dirichlet_01_readjust_05_a.csv --device 0

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.5 --need-readjust --readjustment-ratio 0.7 --outfile cifar10_05_dirichlet_01_readjust_07_a.csv --device 2

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.5 --need-readjust --readjustment-ratio 0.9 --outfile cifar10_05_dirichlet_01_readjust_09_a.csv --device 3





cifar10_05_iid_read_03.csv

0.64：python fedavg.py --dataset cifar10 --eta 0.01 --clients 10 --total-clients 10 --rounds 200 --distribution iid --sparsity 0.5 --Gamma 1 --tradeoff 0.8 --need-readjust --readjustment-ratio 0.3 --outfile test5.csv --device

cifar10_05_dirichlet_01_read_03.csv

0.44：python fedavg.py --dataset cifar10 --eta 0.01 --clients 10 --total-clients 10 --rounds 200 --distribution dirichlet --sparsity 0.5 --beta 0.1 --Gamma 1 --tradeoff 0.8 --need-readjust --readjustment-ratio 0.3 --outfile test6.csv --device

cifar10_05_dirichlet_05_read_03.csv

0.59：python fedavg.py --dataset cifar10 --eta 0.01 --clients 10 --total-clients 10 --rounds 200 --distribution dirichlet --sparsity 0.5 --beta 0.5 --Gamma 1 --tradeoff 0.8 --need-readjust --readjustment-ratio 0.3 --outfile test7.csv --device
