## 1. 背景

Docker Desktop 开始收费以来，[Colima](https://github.com/abiosoft/colima) 就成为了它的一个不错的替代方案, colima 安装简单, 而且使用 docker client 作为 Docker 运行时以后, 与之前安装 Docker Desktop 之后, 在命令行下使用几乎无差. 

macOS 系统下只需要下面几条命令即可完成替代:
```shell
brew install docker  # 使用 docker client 作为 docker 运行时, 还可以使用 containerd 作 为docker 运行时.
brew install colima
colima start
```

结合 macOS 系统用户的使用习惯（下班后直接合上笔记本就走）, colima 在后台稳定运行, 几乎感受不到它的存在.

使用 colima 一段儿时间之后, 可能需要从本地登录一些不安全 Docker 镜像仓库站点（使用非权威 CA 颁发的 SSL 证书）, 但由于本地已经没有了 docker server 的守护进程, 所以不能修改 docker 守护进程的配置文件 /etc/docker/daemon.json 来添加不安全的 Docker 镜像仓库站点.

经过搜索 colima 相关文档, 找到下面的修改方法.

## 2.修改 colima 配置文件添加不安全的 docker 镜像仓库站点

```shell
vi ~/.colima/default/colima.yaml

docker:
  insecure-registries:
    - harbor.poc.cnstack.cloud:5000
    - harbor.six.cnstack.cloud
    ......
```

修改完成后，重启 colima 后端的虚拟机即可:
```shell
colima stop
colima start
docker login harbor.six.cnstack.cloud

Username: harbor
Password:
WARNING! Your password will be stored unencrypted in /Users/tdy218/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```
