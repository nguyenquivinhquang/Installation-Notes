## First you need to install cuda and cudnn. Must install cuda version less than 9.1


# Install gcc6
```
sudo apt install gcc-6 g++-6

sudo ln -s /usr/bin/gcc-6 /usr/local/cuda/bin/gcc

sudo ln -s /usr/bin/g++-6 /usr/local/cuda/bin/g++
  
```

# Installing Torch
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
