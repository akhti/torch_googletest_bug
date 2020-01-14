Demo of the linking bug between libtorch.so and googletest.

To reproduce:
```
conda create --yes -n test_env123 python=3.7
source activate test_env123
conda install --yes pytorch=1.3 torchvision cudatoolkit=9.2 -c pytorch
git clone https://github.com/akhti/torch_googletest_bug.git --recursive
cd torch_googletest_bug
mkdir build -p
cd build
cmake .. && make
```

`simple_test.cc` successfully compiles on its one, but failes at linking stage with torch.
