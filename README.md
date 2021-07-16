# Step 0: Pre-installation actions
## Remove existing NVIDIA/cuda drivers
`watch "dpkg -l | grep nvidia"
`
`sudo dpkg --force-all -P <PACKAGE>`
## Verify CUDA-capable GPUs, gcc installation, Linux versions
`lspci | grep -i nvidia`

`gcc --version`

`uname -m && cat /etc/*release`
## Install NVIDIA drivers
`sudo apt-get install linux-headers-$(uname -r)`

`distribution=$(. /etc/os-release;echo $ID$VERSION_ID | sed -e 's/\.//g') \
   && wget https://developer.download.nvidia.com/compute/cuda/repos/$distribution/x86_64/cuda-$distribution.pin \
   && sudo mv cuda-$distribution.pin /etc/apt/preferences.d/cuda-repository-pin-600`
   
`sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/$distribution/x86_64/7fa2af80.pub \
   && echo "deb http://developer.download.nvidia.com/compute/cuda/repos/$distribution/x86_64 /" | sudo tee /etc/apt/sources.list.d/cuda.list`
   
`sudo apt-get update \
   && sudo apt-get -y install cuda-drivers`
   


If something is wrong refer [here](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
# Step 1: Install microk8s
## Install microk8s with a fixed channel
`snap install microk8s --classic --channel=1.21/stable`
## Add user groups
```
sudo usermod -a -G microk8s user
sudo chown -f -R user ~/.kube
newgrp microk8s
```
## Check if k8s is ready
`microk8s status --wait-ready`

microk8s enable kubeflow --bundle=cs:kubeflow
