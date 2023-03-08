超参数变化

cifar10_05_iid_read_03.csv

0.64：python fedavg.py --dataset cifar10 --eta 0.01 --clients 10 --total-clients 10 --rounds 200 --distribution iid --sparsity 0.5 --Gamma 1 --tradeoff 0.8 --need-readjust --readjustment-ratio 0.3 --outfile test5.csv --device

cifar10_05_dirichlet_01_read_03.csv

0.44：python fedavg.py --dataset cifar10 --eta 0.01 --clients 10 --total-clients 10 --rounds 200 --distribution dirichlet --sparsity 0.5 --beta 0.1 --Gamma 1 --tradeoff 0.8 --need-readjust --readjustment-ratio 0.3 --outfile test6.csv --device

cifar10_05_dirichlet_05_read_03.csv

0.59：python fedavg.py --dataset cifar10 --eta 0.01 --clients 10 --total-clients 10 --rounds 200 --distribution dirichlet --sparsity 0.5 --beta 0.5 --Gamma 1 --tradeoff 0.8 --need-readjust --readjustment-ratio 0.3 --outfile test7.csv --device
