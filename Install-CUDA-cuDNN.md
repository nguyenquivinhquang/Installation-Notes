# Ubuntu-18.04 Install Nvidia driver and CUDA and CUDNN and build Tensorflow for gpu
Ubuntu 18.04 Tutorial : How to install Nvidia driver + CUDA + CUDNN +  build tensorflow for gpu step by step command line

Thoses steps allowed me to build tensorflow for gpu with a comptute capabilities of 3.0 on a laptop with a GeForce 740m and Ubuntu 18.04.

Install neccesary library :

```
   sudo apt-get install openjdk-8-jdk git python-dev python3-dev python-numpy python3-numpy python-six python3-six build-essential python-pip python3-pip python-virtualenv swig python-wheel python3-wheel libcurl3-dev libcupti-dev
    
```
If libcurl3-dev package is not found use:

```
   sudo apt-get install libcurl4-openssl-dev
    
```

# Install nvidia driver #

Add graphics drivers to your source list :
```
    sudo add-apt-repository ppa:graphics-drivers/ppa
    sudo apt update
    sudo apt upgrade
   
```
 Check what driver will be installed :
```
    ubuntu-drivers devices
   ```
 Auto install latest driver (it will do everything blacklist drivers nouveau , create nvidia daemon  , ect ...) :
   ```
    sudo ubuntu-drivers autoinstall
   ```
   Then reboot your machine :
   ```
    sudo reboot
   ```
   If you boot without any kernel crash you're ok but you can check the correct install of the driver :
   ```
   lsmod | grep nvidia
   
```
  or

```
   nvidia-smi

```
# Install cuda # 

Download cuda_your_cuda_version.run on https://developer.nvidia.com/cuda-toolkit and install it: 

```
     cd Downloads/
     sudo sh cuda_9.0.176_384.81_linux.run --override --silent --toolkit
   
```
If everything is ok you should see a cuda folder in /usr/local/ .

Download linux cudnn_your_version on https://developer.nvidia.com/cudnn and install it:

```
     tar -xzvf cudnn-9.0-linux-x64-v7.1.tgz 
     sudo cp cuda/include/cudnn.h /usr/local/cuda/include
     sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
     sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
   
   
```
Check if you have correctly copied cudnn in /usr/local/cuda/lib64/.

Now you must add some path to your  /.bashrc :

```
     gedit ~/.bashrc
   
 ```
 Add those line at the end of your /.bashrc :
 
  ```
 export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64"
 export CUDA_HOME=/usr/local/cuda
  ```
  Now reload your terminal config :
 ```
     source ~/.bashrc
     sudo ldconfig
  ```
Check if the path are correctly installed :
     
 ```
     echo $CUDA_HOME
 ```
 
## Change version of CUDA with multiple version
We can swich among CUDA version via using following commands:

```
$ sudo rm /usr/local/cuda
$ sudo ln -s /usr/local/cuda-<version> /usr/local/cuda
```



# Build tensorflow with Bazel #
 
 Install gcc 4.8 (only version of gcc that can currently compile tensorflow) :

```
    sudo apt-get install gcc-4.8 g++-4.8
    sudo apt-get update
   
```
 If gcc-4.8 package is not found you can try to add :

```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get install gcc-4.8 g++-4.8
  
```
Install bazel :

```
 sudo apt install curl
 echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
 curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -
 sudo apt-get update
 sudo apt-get install bazel
 sudo apt-get upgrade bazel
   
```
Download tensorflow and choose what branch you want :

```
     cd ~
     git clone https://github.com/tensorflow/tensorflow
     cd ~/tensorflow
     git checkout r1.8
     cd ~/tensorflow
  
```
Create configuration file for tensorflow build :

```
    ./configure
  
```
Say no to most query just specify the python version you want , yes to jemalloc and specify correct path to gcc-4.8.
```
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3
Do you wish to build TensorFlow with jemalloc as malloc support? [Y/n]: Y
Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: N
Do you wish to build TensorFlow with Hadoop File System support? [Y/n]: N
Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: N
Do you wish to build TensorFlow with Apache Kafka Platform support? [y/N]: N
Do you wish to build TensorFlow with XLA JIT support? [y/N]: N
Do you wish to build TensorFlow with GDR support? [y/N]: N
Do you wish to build TensorFlow with VERBS support? [y/N]: N
Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: N
Do you wish to build TensorFlow with CUDA support? [y/N]: Y
Please specify the CUDA SDK version you want to use, e.g. 7.0. [Leave empty to default to CUDA 9.0]: 9.0
Please specify the location where CUDA 9.1 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: /usr/local/cuda
Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]: 7.1
Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: /usr/local/cuda
Do you wish to build TensorFlow with TensorRT support? [y/N]: N
Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 5.0] 3.0
Do you want to use clang as CUDA compiler? [y/N]: N
Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]: /usr/bin/gcc-4.8
Do you wish to build TensorFlow with MPI support? [y/N]: N
Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: -march=native
Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]:N

````
Build tensorflow with bazel :

```
sudo bazel build --config=opt --config=cuda --action_env="/usr/local/cuda/lib64" //tensorflow/tools/pip_package:build_pip_package
    
```
Create .whl for pip install :

```
   bazel-bin/tensorflow/tools/pip_package/build_pip_package tensorflow_pkg
   cd tensorflow_pkg/
   sudo pip3 install tensorflow-<name_of_generated_file>.whl 
  
```
Let me know if you find some quicker way to build tensorflow or if you found some mistakes.
