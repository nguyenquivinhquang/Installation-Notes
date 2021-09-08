## First you need to install cuda and cudnn. Must install cuda version less than 9.1


# Install gcc6
```
sudo apt install gcc-6 g++-6

sudo ln -s /usr/bin/gcc-6 /usr/local/cuda/bin/gcc

sudo ln -s /usr/bin/g++-6 /usr/local/cuda/bin/g++
  
```

# Installing Torch
## First:
Follow this instruction to install torch7: [link](http://torch.ch/docs/getting-started.html#installing-torch)

If you see the error like below: 
```
Package python-software-properties is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source
However the following packages replace it:
  software-properties-common
E: Package 'python-software-properties' has no installation candidate
```

The way to fix:

  + Go to folder which you clone Torch.
  + Open file install-deps.
  + Find the line **sudo apt-get install -y python-software-properties** and comment this line.
  + Install torch again.


## Second: 
Run install.sh. 
Some errors and the way to fix:

### Error: Unsupported gpu architecture 'compute_75'
Fix: run the line below in the folder torch and run install.sh again.
```
export TORCH_CUDA_ARCH_LIST="7.0"
```

### Error: "THCTensorMathPairwise.cu(66): error: more than one operator "!=" matches
Fix: run the line below in the folder torch and run install.sh again.
```
export TORCH_NVCC_FLAGS="-D__CUDA_NO_HALF_OPERATORS__" 
```
