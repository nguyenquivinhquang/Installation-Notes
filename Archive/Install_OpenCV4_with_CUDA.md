# How to install OpenCV 4.2.0 with CUDA 10.0 in Ubuntu distro 18.04

First of all install update and upgrade your system:
    
        $ sudo apt update
        $ sudo apt upgrade
   
    
Then, install required libraries:

* Generic tools:

        $ sudo apt install build-essential cmake pkg-config unzip yasm git checkinstall
    
* Image I/O libs
    ``` 
    $ sudo apt install libjpeg-dev libpng-dev libtiff-dev
    ``` 
* Video/Audio Libs - FFMPEG, GSTREAMER, x264 and so on.
    ```
    $ sudo apt install libavcodec-dev libavformat-dev libswscale-dev libavresample-dev
    $ sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
    $ sudo apt install libxvidcore-dev x264 libx264-dev libfaac-dev libmp3lame-dev libtheora-dev 
    $ sudo apt install libfaac-dev libmp3lame-dev libvorbis-dev
    ```
* OpenCore - Adaptive Multi Rate Narrow Band (AMRNB) and Wide Band (AMRWB) speech codec
    ```
    $ sudo apt install libopencore-amrnb-dev libopencore-amrwb-dev
    ```
    
* Cameras programming interface libs
    ```
    $ sudo apt-get install libdc1394-22 libdc1394-22-dev libxine2-dev libv4l-dev v4l-utils
    $ cd /usr/include/linux
    $ sudo ln -s -f ../libv4l1-videodev.h videodev.h
    $ cd ~
    ```

* GTK lib for the graphical user functionalites coming from OpenCV highghui module 
    ```
    $ sudo apt-get install libgtk-3-dev
    ```
* Python libraries for python3:
    ```
    $ sudo apt-get install python3-dev python3-pip
    $ sudo -H pip3 install -U pip numpy
    $ sudo apt install python3-testresources
    ```
* Parallelism library C++ for CPU
    ```
    $ sudo apt-get install libtbb-dev
    ```
* Optimization libraries for OpenCV
    ```
    $ sudo apt-get install libatlas-base-dev gfortran
    ```
* Optional libraries:
    ```
    $ sudo apt-get install libprotobuf-dev protobuf-compiler
    $ sudo apt-get install libgoogle-glog-dev libgflags-dev
    $ sudo apt-get install libgphoto2-dev libeigen3-dev libhdf5-dev doxygen
    ```

We will now proceed with the installation (see the Qt flag that is disabled to do not have conflicts with Qt5.0).

    $ cd ~
    $ wget -O opencv.zip https://github.com/opencv/opencv/archive/4.2.0.zip
    $ wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.2.0.zip
    $ unzip opencv.zip
    $ unzip opencv_contrib.zip
    
    $ echo "Create a virtual environtment for the python binding module"
    $ sudo pip install virtualenv virtualenvwrapper
    $ sudo rm -rf ~/.cache/pip
    $ echo "Edit ~/.bashrc"
    $ export WORKON_HOME=$HOME/.virtualenvs
    $ export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
    $ source /usr/local/bin/virtualenvwrapper.sh
    $ mkvirtualenv cv -p python3
    $ pip install numpy
    
    $ echo "Procced with the installation"
    $ cd opencv-4.2.0
    $ mkdir build
    $ cd build
    
    $ cmake -D CMAKE_BUILD_TYPE=RELEASE \
    	-D CMAKE_C_COMPILER=/usr/bin/gcc-6 \
	-D CMAKE_INSTALL_PREFIX=/usr/local \
	-D INSTALL_PYTHON_EXAMPLES=ON \
	-D INSTALL_C_EXAMPLES=OFF \
	-D WITH_TBB=ON \
	-D WITH_CUDA=ON \
	-D BUILD_opencv_cudacodec=OFF \
	-D ENABLE_FAST_MATH=1 \
	-D CUDA_FAST_MATH=1 \
	-D WITH_CUBLAS=1 \
	-D WITH_V4L=ON \
	-D WITH_QT=OFF \
	-D WITH_OPENGL=ON \
	-D WITH_GSTREAMER=ON \
	-D OPENCV_GENERATE_PKGCONFIG=ON \
	-D OPENCV_PC_FILE_NAME=opencv.pc \
	-D OPENCV_ENABLE_NONFREE=ON \
	-D OPENCV_PYTHON3_INSTALL_PATH=~/.virtualenvs/cv/lib/python3.6/site-packages \
	-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-4.2.0/modules \
	-D PYTHON_EXECUTABLE=~/.virtualenvs/cv/bin/python \
	-D BUILD_EXAMPLES=ON ..
	

If you want to build the libraries statically you only have to include the *-D  BUILD_SHARED_LIBS=OFF*

    $ cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_C_COMPILER=/usr/bin/gcc-6 -D CMAKE_INSTALL_PREFIX=/usr/local -D INSTALL_PYTHON_EXAMPLES=ON -D INSTALL_C_EXAMPLES=OFF -D WITH_TBB=ON -D WITH_CUDA=ON -D BUILD_opencv_cudacodec=OFF -D ENABLE_FAST_MATH=1 -D CUDA_FAST_MATH=1 -D WITH_CUBLAS=1 -D WITH_V4L=ON -D WITH_QT=OFF -D WITH_OPENGL=ON -D WITH_GSTREAMER=ON -D OPENCV_GENERATE_PKGCONFIG=ON -D OPENCV_PC_FILE_NAME=opencv.pc -D OPENCV_ENABLE_NONFREE=ON -D OPENCV_PYTHON3_INSTALL_PATH=~/.virtualenvs/cv/lib/python3.6/site-packages -D OPENCV_EXTRA_MODULES_PATH=~/downloads/opencv/opencv_contrib-4.2.0/modules -D PYTHON_EXECUTABLE=~/.virtualenvs/cv/bin/python -D BUILD_EXAMPLES=ON -D BUILD_SHARED_LIBS=OFF ..
    
In case you do not want to include include CUDA set *-D WITH_CUDA=OFF*     

    $ cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_C_COMPILER=/usr/bin/gcc-6 -D CMAKE_INSTALL_PREFIX=/usr/local -D INSTALL_PYTHON_EXAMPLES=ON -D INSTALL_C_EXAMPLES=OFF -D WITH_TBB=ON -D WITH_CUDA=OFF -D BUILD_opencv_cudacodec=OFF -D ENABLE_FAST_MATH=1 -D CUDA_FAST_MATH=1 -D WITH_CUBLAS=1 -D WITH_V4L=ON -D WITH_QT=OFF -D WITH_OPENGL=ON -D WITH_GSTREAMER=ON -D OPENCV_GENERATE_PKGCONFIG=ON -D OPENCV_PC_FILE_NAME=opencv.pc -D OPENCV_ENABLE_NONFREE=ON -D OPENCV_PYTHON3_INSTALL_PATH=~/.virtualenvs/cv/lib/python3.6/site-packages -D OPENCV_EXTRA_MODULES_PATH=~/downloads/opencv/opencv_contrib-4.2.0/modules -D PYTHON_EXECUTABLE=~/.virtualenvs/cv/bin/python -D BUILD_EXAMPLES=ON ..
    
If you want also to use CUDNN you must include those flags (to set the correct value of CUDA_ARCH_BIN you must visit https://developer.nvidia.com/cuda-gpus and find the Compute Capability CC of your graphic card). If you have problems with the setting up of CUDDN check the *List of documented problems*:

	-D WITH_CUDNN=ON \
	-D OPENCV_DNN_CUDA=ON \
	-D CUDA_ARCH_BIN=7.5 \

Before the compilation you must check that CUDA has been enabled in the configuration summary printed on the screen. (If you have problems with the CUDA Architecture go to the end of the document).

```
--   NVIDIA CUDA:                   YES (ver 10.0, CUFFT CUBLAS NVCUVID FAST_MATH)
--     NVIDIA GPU arch:             30 35 37 50 52 60 61 70 75
--     NVIDIA PTX archs:

```


If it is fine proceed with the compilation (Use nproc to know the number of cpu cores):
    
    $ nproc
    $ make -j8
    $ sudo make install

Include the libs in your environment    
    
    $ sudo /bin/bash -c 'echo "/usr/local/lib" >> /etc/ld.so.conf.d/opencv.conf'
    $ sudo ldconfig
    
If you want to have available opencv python bindings in the system environment you should copy the created folder during the installation of OpenCV (* -D OPENCV_PYTHON3_INSTALL_PATH=~/.virtualenvs/cv/lib/python3.6/site-packages *) into the *dist-packages* folder of the target python interpreter:

    $ sudo cp -r ~/.virtualenvs/cv/lib/python3.6/site-packages/cv2 /usr/local/lib/python3.6/dist-packages
    
    $ echo "Modify config-3.6.py to point to the target directory" 
    $ sudo nano /usr/local/lib/python3.6/dist-packages/cv2/config-3.6.py 
    
    ``` 
	    PYTHON_EXTENSIONS_PATHS = [
	    os.path.join('/usr/local/lib/python3.6/dist-packages/cv2', 'python-3.6')
	    ] + PYTHON_EXTENSIONS_PATHS
    ``` 

### EXAMPLE TO TEST OPENCV 4.2.0 with GPU in C++

Verify the installation by compiling and executing the following example:
```
#include <iostream>
#include <ctime>
#include <cmath>
#include "bits/time.h"

#include <opencv2/core.hpp>
#include <opencv2/highgui.hpp>
#include <opencv2/imgproc.hpp>
#include <opencv2/imgcodecs.hpp>

#include <opencv2/core/cuda.hpp>
#include <opencv2/cudaarithm.hpp>
#include <opencv2/cudaimgproc.hpp>

#define TestCUDA true

int main() {
    std::clock_t begin = std::clock();

        try {
            cv::String filename = "/home/raul/Pictures/Screenshot_20170317_105454.png";
            cv::Mat srcHost = cv::imread(filename, cv::IMREAD_GRAYSCALE);

            for(int i=0; i<1000; i++) {
                if(TestCUDA) {
                    cv::cuda::GpuMat dst, src;
                    src.upload(srcHost);

                    //cv::cuda::threshold(src,dst,128.0,255.0, CV_THRESH_BINARY);
                    cv::cuda::bilateralFilter(src,dst,3,1,1);

                    cv::Mat resultHost;
                    dst.download(resultHost);
                } else {
                    cv::Mat dst;
                    cv::bilateralFilter(srcHost,dst,3,1,1);
                }
            }

            //cv::imshow("Result",resultHost);
            //cv::waitKey();

        } catch(const cv::Exception& ex) {
            std::cout << "Error: " << ex.what() << std::endl;
        }

    std::clock_t end = std::clock();
    std::cout << double(end-begin) / CLOCKS_PER_SEC  << std::endl;
}
```
Compile and execute:

    $ g++ test.cpp `pkg-config opencv --cflags --libs` -o test
    $ ./test


### List of documented problems

If you have problems with unsupported architectures of your graphic card with the minimum requirements from Opencv, you will get the following error:

```
CUDA backend for DNN module requires CC 5.3 or higher.  Please remove unsupported architectures from CUDA_ARCH_BIN option.
```
It means that the DNN module needs that your graphic card supports the 5.3 Compute Capability (CC) version; in this [link](https://developer.nvidia.com/cuda-gpus) you can fint the CC of your card. Some opencv versions have fixed the minimum version to 3.0 but there is a clear move to filter above 5.3 since the half-precision precision operations are available from 5.3 version. To fix this problem you can modify the *CMakeList.txt* file located in *opencv > modules > dnn > CMakeList.txt* and set the minimum version to the one you have, but bear in mind that the correct functioning of this module will be compromised. However, if you only want GPU for the rest of modules, it could work.

You can also select the target `CUDA_ARCH_BIN` option in the command to generate the makefile for your current target or modify the list of supported architectures:

	$ grep -r 'CUDA_ARCH_BIN' .  //That prompts ./CMakeCache.txt


The restriction is to have a higher version than 5.3, so you can modify the file by removing all the inferior arch to 5.3

```
CUDA_ARCH_BIN:STRING=6.0 6.1 7.0 7.5
```
Now, the makefile was created succesfully. Before the compilation you must check that CUDA has been enabled in the configuration summary printed on the screen.

```
--   NVIDIA CUDA:                   YES (ver 10.0, CUFFT CUBLAS NVCUVID FAST_MATH)
--     NVIDIA GPU arch:             60 61 70 75
--     NVIDIA PTX archs:

```

Some users as TAF2 had problems when configuring CUDNN libraries but it was solved and here is the TAF2's proposal, you can also find it in the comments:

```
sudo apt install libcudnn7-dev  libcudnn7-doc  libcudnn7 nvidia-container-csv-cudnn
```

```
 -D CUDNN_INCLUDE_DIR=/usr/include \
-D CUDNN_LIBRARY=/usr/lib64/libcudnn_static_v7.a \
-D CUDNN_VERSION=7.6.3
```

*If you have any other problem try updating the nvidia drivers.*

### Source
- [pyimagesearch](https://www.pyimagesearch.com/2018/08/15/how-to-install-opencv-4-on-ubuntu/)
- [learnopencv](https://www.learnopencv.com/install-opencv-4-on-ubuntu-18-04/)
- [Tzu-cheng](https://chuangtc.com/ParallelComputing/OpenCV_Nvidia_CUDA_Setup.php)
- [Medium](https://medium.com/@debugvn/installing-opencv-3-3-0-on-ubuntu-16-04-lts-7db376f93961)
- [Previous Gist](https://gist.github.com/raulqf/a3caa97db3f8760af33266a1475d0e5e)
