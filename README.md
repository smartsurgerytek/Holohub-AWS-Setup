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
