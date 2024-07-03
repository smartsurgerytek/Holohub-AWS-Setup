# Holohub-AWS-Setup
This repo contains every thing we need to setup the Holohub environment on AWS.
## Step 1
Follow this tutorial: https://github.com/nvidia-holoscan/holohub/tree/main/tutorials/holoscan-playground-on-aws

until "Setting up Display Forwarding from EC2 Instance".
## Remove Original NVIDIA Driver
```bash
sudo apt purge nvidia-*
```
```bash
sudo apt autoremove --purge
```
```bash
sudo dpkg-reconfigure xserver-xorg
```
## Install NVIDIA DIRVER 535
```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
```
```bash
sudo apt install nvidia-driver-535
```
## Reboot and Test
```bash
sudo reboot
```
Wait for a couple of minutes and then reconnect.
```bash
nvidia-smi
```
You should see the NVIDIA driver information below:
```
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.183.01             Driver Version: 535.183.01   CUDA Version: 12.2     |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |         Memory-Usage | GPU-Util  Compute M. |
|                                         |                      |               MIG M. |
|=========================================+======================+======================|
|   0  NVIDIA A10G                    On  | 00000000:00:1E.0 Off |                    0 |
|  0%   25C    P8               9W / 300W |      0MiB / 23028MiB |      0%      Default |
|                                         |                      |                  N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:                                                                            |
|  GPU   GI   CI        PID   Type   Process name                            GPU Memory |
|        ID   ID                                                             Usage      |
|=======================================================================================|
|  No running processes found                                                           |
+---------------------------------------------------------------------------------------+
```
```bash
sudo apt install -y x11-apps
```
```bash
xeyes
```
You should see an X11 window popup on your local machine.
## Set up ~/.bashrc
```bash
vim ~/.bashrc
```
Add these lines to the end of the file:
```bash
nvidia_icd_json=$(find /usr/share /etc -path '*/vulkan/icd.d/nvidia_icd.json' -type f,l -print -quit 2>/dev/null | grep .) || (echo "nvidia_icd.json not found" >&2 && false)
NGC_CONTAINER_IMAGE_PATH="nvcr.io/nvidia/clara-holoscan/holoscan:v2.2.0-dgpu"
XSOCK=/tmp/.X11-unix
XAUTH=/tmp/.docker.xauth
# the error “file does not exist” is expected at the next command
xauth nlist $DISPLAY | sed -e 's/^..../ffff/' | sudo xauth -f $XAUTH nmerge -
```
## Clone Holohub and Build Image
```bash
cd ~
git clone https://github.com/nvidia-holoscan/holohub.git
cd holohub
```
```bash
sudo apt install docker-buildx-plugin
./dev_container build
```
## Install the NVIDIA Container Toolkit
Follow this instruction:
https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html
## Set up ~/.bashrc
```bash
vim ~/.bashrc
```
Add these lines to the end of the file:
```bash
HOLOHUB_IMAGE=holohub:ngc-v2.1.0-dgpu
cd holohub
```
Restart bash
## Run Container
```bash
docker run -it --net host \
--gpus all \
-v $XAUTH:$XAUTH \
-v $XSOCK:$XSOCK \
-v $nvidia_icd_json:$nvidia_icd_json:ro \
-e XAUTHORITY=$XAUTH \
-e NVIDIA_DRIVER_CAPABILITIES=graphics,video,compute,utility,display \
-e DISPLAY \
--ipc=host \
--cap-add=CAP_SYS_PTRACE \
--runtime=nvidia \
--ulimit memlock=-1 \
-v /etc/group:/etc/group:ro \
-v /etc/passwd:/etc/passwd:ro \
-v $PWD:/workspace/holohub \
-w /workspace/holohub \
--group-add video \
$HOLOHUB_IMAGE
```
## Build Holohub Multi-AI Endoscopy Example
Inside the Container, do this:
```bash
. run build multiai_endoscopy cpp
. run launch multiai_endoscopy cpp
```
This may take a while, you will see a X window popup and the application running.

## Setup Development Enviroment
1. Install VSCode Remote Extension on local machine
2. SSH into the AWS EC2 instance
3. Install VSCode Docker extension on AWS EC2 instance
4. Attach EC2 to the running container

# Congradulation
