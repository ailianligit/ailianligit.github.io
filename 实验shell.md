## test1.sh

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.8 --outfile cifar10_08_dirichlet_01_b.csv --device 0

## test2.sh

python fedavg.py --distribution iid --sparsity 0.85 --outfile cifar10_085_iid_a.csv --device 2

## test3.sh

python fedavg.py --distribution dirichlet --beta 0.5 --sparsity 0.85 --outfile cifar10_085_dirichlet_05_a.csv --device 3

## test4.sh

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.85 --outfile cifar10_085_dirichlet_01_a.csv --device 0

## test5.sh

python fedavg.py --distribution iid --sparsity 0.95 --outfile cifar10_095_iid_a.csv --device 2

## test6.sh

python fedavg.py --distribution dirichlet --beta 0.5 --sparsity 0.95 --outfile cifar10_095_dirichlet_05_a.csv --device 3

## test7.sh

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.95 --outfile cifar10_095_dirichlet_01_a.csv --device 0

## test8.sh

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.5 --need-readjust --readjustment-ratio 0.3 --outfile cifar10_05_dirichlet_01_readjust_03_a.csv --device 2

## test9.sh

python fedavg.py --distribution dirichlet --beta 0.1 --sparsity 0.5 --need-readjust --readjustment-ratio 0.7 --outfile cifar10_05_dirichlet_01_readjust_07_a.csv --device 3