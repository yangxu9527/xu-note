### 安装虚拟机

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

  ​	找到`C:\Users\yangxu\Vagrantfile`，设置固定ip。

  ​	`config.vm.network "private_network", ip: "192.168.56.10"`

  ​	通过cmd重启vagrant：`vagrant reload`

  ​	cmd通过vagrant进入虚拟机：`vagrant ssh`

  ​	默认账号名和密码：vagrant/vagragnt root/vagrant

### 安装Docker

------

-   **官方文档**


  ​	`https://docs.docker.com/engine/install/centos/`

  ```shell
  # 删除旧版本docker
  sudo yum remove docker \
                    docker-client \
                    docker-client-latest \
                    docker-common \
                    docker-latest \
                    docker-latest-logrotate \
                    docker-logrotate \
                    docker-engine
  # 安装依赖包
  sudo yum install -y yum-utils
  # 设置镜像地址
  sudo yum-config-manager \
      --add-repo \
      https://download.docker.com/linux/centos/docker-ce.repo
  ```

-   **配置阿里镜像**


  ​	`https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors`

-   **安装docker**


  ```shell
  # 安装
  sudo yum install docker-ce docker-ce-cli containerd.io
  # 启动docker
  sudo systemctl start docker
  # 设置为开机启动
  sudo systemctl enable docker
  ```
### 安装Mysql
------

​	

```shell
# 拉取镜像
docker pull mysql:5.7

# 启动mysql并挂载目录
# -v表示把docker内部的目录挂载到linux的目录文件下
docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.7

# 配置mysql的编码
vi /mydata/mysql/conf/my.cnf

    [client]
    default-character-set=utf8

    [mysql]
    default-character-set=utf8
    [mysqld]
    init_connect='SET colletion_connection = utf8_unicode_ci'
    init_connect='SET NAMES utf8'
    character-set-server=utf8
    collection-server=utf8_unicode_ci
    skip-character-set-client-handshake
    skip-name-resolve
    
# 重启mysql
docker restart mysql
```

