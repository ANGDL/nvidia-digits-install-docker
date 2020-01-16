# nvidia-digits-install-docker
## 准备
安装前需要保证已经安装Nviida GPU驱动程序和docker
## 1. Installing Nvidia-Docker

```shell
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.repo | sudo tee /etc/yum.repos.d/nvidia-docker.repo

$ sudo yum install -y nvidia-container-toolkit
$ sudo systemctl restart docker
```
## 2. Install nvidia-container-runtime
```shell
sudo yum install nvidia-container-runtime

sudo mkdir -p /etc/systemd/system/docker.service.d
sudo tee /etc/systemd/system/docker.service.d/override.conf <<EOF
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --host=fd:// --add-runtime=nvidia=/usr/bin/nvidia-container-runtime
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
## 3. Install DIGITS with Docker
```shell
docker pull nvidia/digits:latest
```
## 4. Running DIGITS on Nvidia Docker
```shell
# Run DIGITS with the mnist dataset stored on the host under /home/crscic/data
# -v 将/home/crscic/data 挂载到docker /data下
docker run --runtime=nvidia --name digits -d -p 5000:5000 -v /home/crscic/data:/data/ nvidia/digits
```
## 5. 在终端浏览器中打开
192.168.7.64:5000
