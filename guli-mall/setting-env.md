1. #### 安装虚拟机

------

- **安装Virtual Box**

下载地址：https://www.virtualbox.org/

- **安装vagrant**

下载地址：https://www.vagrantup.com/downloads

- **vagrant安装centos7**

  ```bash
  vagrant init centos7 https://mirrors.ustc.edu.cn/centos-cloud/centos/7/vagrant/x86_64/images/CentOS-7.box
  启动：vagrant up
  ```

- **设置网络**

​	找到C:\Users\yangxu\VagrantFile，设置固定ip。

​	`config.vm.network "private_network", ip: "192.168.56.10"`