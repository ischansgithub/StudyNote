1. nvidia driver

2. cuda

   ```
   # Add NVIDIA package repositories
       # Add HTTPS support for apt-key
       sudo apt-get install gnupg-curl
       wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_10.0.130-1_amd64.deb
       sudo dpkg -i cuda-repo-ubuntu1604_10.0.130-1_amd64.deb
       sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
       sudo apt-get update
       wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/nvidia-machine-learning-repo-ubuntu1604_1.0.0-1_amd64.deb
       sudo apt install ./nvidia-machine-learning-repo-ubuntu1604_1.0.0-1_amd64.deb
       sudo apt-get update
   
       # 第一步已经安装好，可跳过
       # Install NVIDIA Driver
       # Issue with driver install requires creating /usr/lib/nvidia
       sudo mkdir /usr/lib/nvidia
       sudo apt-get install --no-install-recommends nvidia-410
       # Reboot. Check that GPUs are visible using the command: nvidia-smi
   
       # Install development and runtime libraries (~4GB)
       sudo apt-get install --no-install-recommends \
           cuda-10-0 \
           libcudnn7=7.4.1.5-1+cuda10.0  \
           libcudnn7-dev=7.4.1.5-1+cuda10.0
       
   
       # Install TensorRT. Requires that libcudnn7 is installed above.
       sudo apt-get update && \
               sudo apt-get install nvinfer-runtime-trt-repo-ubuntu1604-5.0.2-ga-cuda10.0 \
               && sudo apt-get update \
               && sudo apt-get install -y --no-install-recommends libnvinfer-dev=5.0.2-1+cuda10.0
   ```

   

3. 下载[cuDNN Library for Linux](https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.6.0.64/prod/10.0_20190516/cudnn-10.0-linux-x64-v7.6.0.64.tgz)，注意要与 CUDA版本匹配：

   <https://blog.csdn.net/DarrenXf/article/details/82182449>

4. ```
   $ pip install virtualenv
   $ cd my_project_dir
   $ virtualenv python3.5_env　　#venv为虚拟环境目录名，目录名自定义
   $ virtualenv -p /usr/bin/python3.5 python3.5_env　　　　# -p参数指定Python解释器程序路径
   $ source python3.5_env/bin/activate  
   $ pip install tensorflow-gpu
   $ . python3.5_env/bin/deactivate
   ```

   

5  cmake3.8 <https://cmake.org/files/v3.8/>