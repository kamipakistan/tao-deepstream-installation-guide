# TAO Toolkit Quick Start
The NVIDIA TAO Toolkit is a software tool that helps you train machine learning models without needing to be an expert in AI or having a lot of data. It makes the process easier and faster by using pre-existing models and allowing you to use your own data to improve them. This can help you make better predictions and decisions based on your specific needs.

[Build Status](https://drive.google.com/file/d/1SdBShKWcp1rNUi89ca7_kyrzWDrug0Uh/view?usp=sharing)

# Requirements
## Minimum Hardware requirements
For optimal training performance using TAO Toolkit and its supported models, it is recommended to use the following system configuration:

> 32 GB system RAM
> 32 GB of GPU RAM
> 8 core CPU
> 1 NVIDIA GPU
> 100 GB of SSD space

TAO Toolkit is supported on discrete GPUs, such as H100, A100, A40, A30, A2, A16, A100x, A30x, V100, T4, Titan-RTX and Quadro-RTX.

> Note: TAO Toolkit is not supported on GPU's before the Pascal generation

## Software requirements
| Software | Version | Comment |
| --- | --- | --- |
| Ubuntu LTS | 20.04 | |
| python | >=3.6.9<3.7 | Not needed if you are using TAO API (See #3 below) |
| docker-ce | >19.03.5 | Not needed if you are using TAO API (See #3 below) |
| docker-API | 1.40 | Not needed if you are using TAO API (See #3 below) |
| nvidia-container-toolkit | >1.3.0-1 | Not needed if you are using TAO API (See #3 below) |
| nvidia-container-runtime | 3.4.0-1 | Not needed if you are using TAO API (See #3 below) |
| nvidia-docker2 | 2.5.0-1 | Not needed if you are using TAO API (See #3 below) |
| nvidia-driver | >520 | Not needed if you are using TAO API (See #3 below) |
| python-pip | >21.06 | Not needed if you are using TAO API (See #3 below) |

# Running TAO Toolkit
TAO toolkit is available as a docker container or a collection of python wheels. There are 4 ways to run TAO Toolkit depending on your preference and setup, through
1. the launcher CLI
2. the containers directly
3. the tao toolkit apis
4. python wheels

> In this particular case, we will be following the first option which is using the Launcher CLI to run TAO Toolkit. However, if you prefer to use TAO Toolkit in any of the other ways mentioned, you can refer to the official documentation for more information on how to do so.

## 1. Launcher CLI

The TAO Toolkit launcher is a simple command-line interface that is based on Python. It acts as a front-end for the TAO Toolkit containers, which are built on top of PyTorch and TensorFlow. The launcher makes it easier to use TAO Toolkit by abstracting away the details of which network is implemented in which container. When you select a particular model to use, the corresponding container is automatically launched by the CLI.

> To get started with the launcher, follow the instructions below to install the required pre-requisite software. 

## Installing the Pre-requisites.
>The TAO Toolkit launcher is strictly a python3 only package, capable of running on python versions >= 3.6.9.

##### 1. Docker Installation. 
The first step involves installing Docker, which is a platform for creating and running software in containers. To install Docker, you can follow the instructions provided in the link given. This will allow you to download Docker-CE, which is the community edition of Docker, and install it on your computer. Once Docker is installed, you will be able to use it to run TAO Toolkit in a container
> Docker Installation [Link]().

Once you have installed `docker-ce`, follow the [post-installation]() steps to ensure that the docker can be run without `sudo`.

##### 2. Install nvidia-container-toolkit
To install the nvidia-container-toolkit, you can follow the [installation guide]() provided. This toolkit is a set of tools and extensions that enable Docker containers to access the GPU on the host machine. By installing the nvidia-container-toolkit, you will be able to run TAO Toolkit in a Docker container with access to the GPU, which can greatly improve the performance of machine learning tasks.

##### 3. Get an [NGC](https://catalog.ngc.nvidia.com/?filters=&orderBy=weightPopularASC&query=) account and API key:
a.  Go to NGC and click the `TAO Toolkit` container in the `Catalog` tab. This message is displayed: “Sign in to access the PULL feature of this repository”.
b. Enter your Email address and click `Next`, or click `Create an Account`.
c. Choose your organization when prompted for `Organization/Team`.
d.  Click `Sign In`.

##### 4. Log in to the NGC docker registry (nvcr.io) using the following command.

```
docker login nvcr.io
```
and enter the following credentials:

```
a. Username: "$oauthtoken"
b. Password: "YOUR_NGC_API_KEY"
```
where `YOUR_NGC_API_KEY` corresponds to the key you generated from step 3.
##### 5. Setup python environment
NVIDIA recommends setting up a python environment using `miniconda`. The following instructions show how to setup a python `conda` environment.

1. Follow the instructions in this [link](https://docs.conda.io/en/latest/miniconda.html) to set up a conda environment using a miniconda.

2. Once you have installed `miniconda`, create a new environment by setting the Python version to 3.6.
    ```
    conda create -n launcher python=3.
    ```
3. Activate the conda environment that you have just created.
    ```
    conda activate launcher
    ```
4. Once you have activated your conda environment, the command prompt should show the name of your conda environment.
    ```
    (launcher) py-3.6.9 desktop:
    ```
5. When you are done with you session, you may deactivate your conda environment using the deactivate command:
    ```
    conda deactivate
    ```
6. You may re-instantiate this created conda environment using the following command
    ```
    conda activate launcher
    ```
# 2. Installing TAO Launcher

1. Download the TAO package
To download the `TAO` package, you can execute a command that will retrieve a collection of files containing startup scripts, Jupyter notebooks, and configuration files necessary for running `TAO` software. This command will allow you to obtain all the required files in a convenient and organized package for your usage.

    ```
    wget --content-disposition https://api.ngc.nvidia.com/v2/resources/nvidia/tao/tao-getting-started/versions/4.0.1/zip -O getting_started_v4.0.1.zip
    unzip -u getting_started_v4.0.1.zip  -d ./getting_started_v4.0.1 && rm -rf getting_started_v4.0.1.zip && cd ./getting_started_v4.0.1
    ```
    or
    ```
    bash setup/quickstart_launcher.sh --install
    ```
    **File Hierarchy**
    ```
    setup
        |--> quickstart_launcher.sh
        |--> quickstart_api_bare_metal
        |--> quickstart_api_aws_eks
    notebooks
        |--> tao_api_starter_kit
            |--> api
                |--> automl
                |--> end2end
                |--> dataset_prepare
            |--> client
                |--> automl
                |--> end2end
                |--> dataset_prepare
        |--> tao_launcher_starter_kit
            |--> yolov4_tiny
            |--> yolov4
            |--> yolov3
            |-->  ...
    ```

2. You can also use this script to update the launcher to the latest version of TAO Toolkit by running the following command.

    ```
    bash setup/quickstart_launcher.sh --upgrade
    ```
3. Invoke the entrypoints using the `tao` command.
    ```
    tao --help
    ```
    The sample output of the above command is:
    ```
    usage: tao [-h]
             {list,stop,info,augment,bpnet,classification,detectnet_v2,dssd,emotionnet,faster_rcnn,fpenet,gazenet,gesturenet,
             heartratenet,intent_slot_classification,lprnet,mask_rcnn,punctuation_and_capitalization,question_answering,
             retinanet,speech_to_text,ssd,text_classification,converter,token_classification,unet,yolo_v3,yolo_v4,yolo_v4_tiny}
             ...
    
    Launcher for TAO

    optional arguments:
    -h, --help            show this help message and exit
    
    tasks:
          {list,stop,info,augment,bpnet,classification,detectnet_v2,dssd,emotionnet,faster_rcnn,fpenet,gazenet,gesturenet,heartratenet
          ,intent_slot_classification,lprnet,mask_rcnn,punctuation_and_capitalization,question_answering,retinanet,speech_to_text,
          ssd,text_classification,converter,token_classification,unet,yolo_v3,yolo_v4,yolo_v4_tiny}
    ```