# Deepstream installation on dGPU Setup for Ubuntu
> For Jetson Setup see the [official documentation](https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_Quickstart.html).

# dGPU Setup for Ubuntu
Before installing the DeepStream SDK, this section provides instructions on preparing an `Ubuntu x86_64` system that includes NVIDIA dGPU devices.
> Note:
> This document uses the term dGPU (“discrete GPU”) to refer to NVIDIA GPU expansion card products such as NVIDIA Tesla® T4 , NVIDIA GeForce® GTX 1080, NVIDIA GeForce® RTX 2080 and NVIDIA GeForce® RTX 3080. This version of DeepStream SDK runs on specific dGPU products on x86_64 platforms supported by NVIDIA driver 525.85.12 and NVIDIA TensorRT™ 8.5.2.2 and later versions.


**You must install the following components:**
* Ubuntu 20.04
* GStreamer 1.16.2
* NVIDIA driver 515.85.12
* CUDA 11.8
* TensorRT 8.5.2.2

### Steps to Install Deepstream
1. Install Dependencies
2. Install CUDA Toolkit 11.8
3. Install NVIDIA driver 525.85.12
4. Install TensorRT 8.5.2.2
5. Install librdkafka (to enable Kafka protocol adaptor for message broker)
6. Install the DeepStream SDK
7. Installing binding for python

> For installing the steps mentioned above, you can refer to the [official documentation](https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_Quickstart.html) for detailed instructions.

# 7. Install Python bindings
## 1. Prerequisites
The following dependencies need to be met in order to compile bindings:


### 1.1 Deepstream SDK
Go to https://developer.nvidia.com/deepstream-sdk, download and install Deepstream SDK and its dependencies


### 1.2 Base dependencies
To compile bindings on Ubuntu - 20.04 [use python-3.8, python-3.6 will not work] :
```
sudo apt install python3-gi python3-dev python3-gst-1.0 python-gi-dev git python-dev \
    python3 python3-pip python3.8-dev cmake g++ build-essential libglib2.0-dev \
    libglib2.0-dev-bin libgstreamer1.0-dev libtool m4 autoconf automake libgirepository1.0-dev libcairo2-dev
```

### 1.3 Initialization of submodules
Make sure you clone the deepstream_python_apps repo under <DeepStream 6.2 ROOT>/sources: git clone https://github.com/NVIDIA-AI-IOT/deepstream_python_apps

This will create the following directory:
```
<DeepStream 6.2 ROOT>/sources/deepstream_python_apps
```

The repository utilizes gst-python and pybind11 submodules. To initializes them, run the following command:
```
cd /opt/nvidia/deepstream/deepstream-6.2/sources/deepstream_python_apps/
sudo git submodule update --init
```
### 1.4 Installing Gst-python
Following commands ensure we add the new certificates that gst-python git server now uses:
```
sudo apt-get install --reinstall ca-certificates
```

Build and install gst-python:
```
cd 3rdparty/gst-python/
sudo ./autogen.sh
sudo make
sudo make install
```


## 2 - Building the bindings
Python bindings are compiled using CMake. Following commands provide quick cmake configurations for common compilation options:

### 2.1 Quick build (x86-ubuntu-18.04 | python 3.6 | Deepstream 6.0.1)
```
cd deepstream_python_apps/bindings
sudo mkdir build
cd build
sudo cmake .. -DPYTHON_MAJOR_VERSION=3 -DPYTHON_MINOR_VERSION=6
make
```

### 2.1.1 Quick build (x86-ubuntu-20.04 | python 3.8 | Deepstream 6.1)

```
cd deepstream_python_apps/bindings
sudo mkdir build
cd build
sudo cmake ..
sudo make
```

### 2.2 Advanced build
#### 2.2.1 Using Cmake options
Multiple options can be used with cmake as follows:
```
cmake .. [-D<var>=<value> [-D<var>=<value> [-D<var>=<value> ... ]]]
```
#### 2.2.2 Available cmake options
| Variable                | Default value | Purpose                                               | Available values                                                                                          |
|-------------------------|---------------|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| DS_VERSION              | 6.2           | Used to determine default deepstream library path      | should match to the deepstream version installed on your computer                                          |
| PYTHON_MAJOR_VERSION    | 3             | Used to set the python version used for the bindings  | 3                                                                                                         |
| PYTHON_MINOR_VERSION    | 8             | Used to set the python version used for the bindings  | 6, 8                                                                                                      |
| PIP_PLATFORM            | linux_x86_64  | Used to select the target architecture to compile the bindings | linux_x86_64, linux_aarch64                                                                           |
| DS_PATH                 | /opt/nvidia/deepstream/deepstream-${DS_VERSION}     | Path where deepstream libraries are available          | Should match the existing deepstream library folder                                                     |

#### 2.2.3 Example
Following commands can be used to compile the bindings natively on Jetson devices
```
cd deepstream_python_apps/bindings
sudo mkdir build
cd build
sudo cmake ..  -DPYTHON_MAJOR_VERSION=3 -DPYTHON_MINOR_VERSION=8 \
    -DPIP_PLATFORM=linux_aarch64 -DDS_PATH=/opt/nvidia/deepstream/deepstream/
sudo make
```
## 3. Using the generated pip wheel
Following commands can be used to install the generated pip wheel.

### 3.1 Installing the pip wheel
The latest prebuilt release package complete with python bindings and sample applications can be downloaded from the [release section](https://github.com/NVIDIA-AI-IOT/deepstream_python_apps/releases) for both x86 and Jetson platforms.
```
sudo apt install libgirepository1.0-dev libcairo2-dev -y
```
```
sudo pip3 install ./pyds-1.1.6-py3-none*.whl
```
### 3.1.1 pip wheel troubleshooting
If the wheel installation fails, upgrade the pip using the following command:
```
sudo apt update
sudo apt upgrade
```


### 3.2 Running sample with deepstream-app
Once you have successfully completed the installation of the Python bindings, you can deactivate your conda environment if you are using one. Then, you can run the `deepstream_test1.py` script using the following command:
```
cd
cd /opt/nvidia/deepstream/deepstream-6.2/sources/deepstream_python_apps/apps/deepstream-test1
sudo python3 deepstream_test_1.py ../../../../samples/streams/sample_qHD.h264
```
This command executes the `deepstream_test_1.py` script and passes the path to a sample video file (sample_qHD.h264) as an argument.

Make sure to replace `../../../../samples/streams/sample_qHD.h264` with the actual path to your video file if it is located elsewhere.

Running the script with this command will initiate the DeepStream test application and process the specified video file according to the logic defined in the deepstream_test_1.py script.

> The scripts mentioned above have been sourced from this [repository](https://github.com/NVIDIA-AI-IOT/deepstream_python_apps/tree/master/bindings), and we express our gratitude to the author for their dedicated efforts in creating and maintaining them.




