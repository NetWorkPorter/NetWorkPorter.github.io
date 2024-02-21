## 安装 Docker,docker-compose

~~~ shell
yum install docker docker-compose
~~~

> 这样安装的是老版本, pull 镜像时会报错

~~~ shell
/usr/bin/docker-current: missing signature key.
See '/usr/bin/docker-current run --help'.
~~~

> 更新版本:
> ~~~ shell
> 卸载旧版本: yum remove docker docker-compose docker-common
> 
> 安装yum管理工具: yum install -y yum-utils
> 
> 添加软件源: yum-config-manager --add-repo https://mirrors.ustc.edu.cn/docker-ce/linux/centos/docker-ce.repo
> 
> 安装新版本: yum install docker-ce
> 
~~~shell
=====================================================================================================================================================
 Package                                      Arch                      Version                            Repository                           Size
=====================================================================================================================================================
Installing:
 docker-ce                                    x86_64                    3:25.0.3-1.el7                     docker-ce-stable                     26 M
Installing for dependencies:
 containerd.io                                x86_64                    1.6.28-3.1.el7                     docker-ce-stable                     35 M
 docker-buildx-plugin                         x86_64                    0.12.1-1.el7                       docker-ce-stable                     13 M
 docker-ce-cli                                x86_64                    1:25.0.3-1.el7                     docker-ce-stable                     14 M
 docker-ce-rootless-extras                    x86_64                    25.0.3-1.el7                       docker-ce-stable                    9.4 M
 docker-compose-plugin                        x86_64                    2.24.5-1.el7                       docker-ce-stable                     13 M

Transaction Summary
=====================================================================================================================================================
Install  1 Package (+5 Dependent packages)
~~~

## 启动 Docker

~~~ shell
systemctl start docker.service
~~~

## 设置开机自启

~~~ shell
systemctl enable docker.service
~~~

## 测试 docker docker-compose

~~~ shell
docker -v
docker compose version
~~~

## 非root用户使用docker命令

> 创建用户组,将当前用户加入到docker组,并重启docker服务,以使当前用户可以使用docker命令

~~~ shell
sudo groupadd docker
sudo usermod -aG docker $USER
sudo systemctl restart docker.service
~~~
