# Holohub-AWS-Setup
This repo contains every thing we need to setup the Holohub environment on AWS.
## Step 1
Follow this tutorial: https://github.com/nvidia-holoscan/holohub/tree/main/tutorials/holoscan-playground-on-aws

until "Setting up Display Forwarding from EC2 Instance".
## Step 2
```bash
sudo apt purge nvidia-*
```
```bash
sudo apt autoremove --purge
```
```bash
sudo dpkg-reconfigure xserver-xorg
```
```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
```
```bash
sudo apt install nvidia-driver-535
```
```bash
sudo reboot
```
```bash
nvidia-smi
```
```bash
sudo apt install -y x11-apps
```
```bash
xeyes
```
