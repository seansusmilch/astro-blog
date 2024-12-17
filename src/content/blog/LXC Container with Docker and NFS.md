---
author: Sean
pubDatetime: 2024-12-16T22:32:00Z
slug: lxc-docker-nfs-proxmox
title: Setting up an LXC Container with Docker and NFS on Proxmox
description: Info on how to set up a Proxmox LXC container with NFS and Docker ready to go
featured: true
draft: false
tags:
  - docker
  - lxc
  - proxmox
  - nfs
  - linux
  - vms
ogImage: "@assets/blog/futuristic-container.png"
---

![futuristic-container.png](@assets/blog/futuristic-container.png)
# Set up LXC container with Docker
1. Create LXC container with helper scripts.
```bash
bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/docker.sh)"
```
>See script page: https://community-scripts.github.io/ProxmoxVE/scripts?id=docker
2. Make new user "sean"
```bash
adduser sean
```
3. Add user to `docker` and `sudo` groups
```shell
usermod -aG docker sean
usermod -aG sudo sean
```
4. Make docker folder and link it to root directory 
```bash
mkdir /home/sean/docker
ln -s /home/sean/docker /docker
```
5. Add ssh key to `~/.ssh/authorized_keys`
6. In Proxmox, set static IP and DNS
# Setting up NFS
>In order to use NFS inside LXC, you need to have a privileged container. Follow these steps to convert your container to privileged in Proxmox.
1. Shut down the container if running
2. Backup container
3. Restore container but change Privilege Level to `Privileged`
4. Under Options > Features, check the NFS feature
5. Boot it up
6. Install nfs-common
```bash
apt update && apt install -y nfs-common
```
7. Update `/etc/fstab`
8. Create mountpoints
```bash
mkdir /mnt/share1 /mnt/share2 /mnt/share3
```
9. Mount all mountpoints
```bash
sudo mount -va
```