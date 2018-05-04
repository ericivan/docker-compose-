### centos yum安装docker

1.删除系统的docker,可以先执行 yum remove docker

```shell
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```


2.安装一些必须的包
```shell
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

3.设置yum仓库
```shell
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

4.启用test还有edge的仓库

```shell
 sudo yum-config-manager --enable docker-ce-edge
```

```shell
sudo yum-config-manager --enable docker-ce-test
```


5.利用yum安装docker

```shell
sudo yum install docker-ce
```


6.如果你要安装其他版本的docker,可以先执行以下指令查看repo源里面docker安装包 

```shell
 yum list docker-ce --showduplicates | sort -r
```

docker-ce.x86_64       18.03.0.ce-1.el7.centos             docker-ce-stable

7.然后执行安装指令加版本信息

```shell
sudo yum install docker-ce-<VERSION STRING>
```


<font color="red">特别注意的一步,一定要开启docker的服务</font>

```shell
systemctl start docker
```
