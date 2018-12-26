---
title: Centos 7 root automatic login
author: wanghuagang
date: 2018-12-26 19:02:00
---

# Centos 7 root automatic login

## Usage

**Docker file**

```bash
FROM centos:7
ENV container docker
VOLUME [ "/sys/fs/cgroup" ]
RUN mkdir /etc/systemd/system/console-getty.service.d/;\
cp /lib/systemd/system/console-getty.service /etc/systemd/system/;\
sed -i '/ExecStart/ s/noclear/noclear --autologin root/' /etc/systemd/system/console-getty.service;\
ln -s ../console-getty.service /etc/systemd/system/console-getty.service.d/console-getty.service
CMD ["/usr/sbin/init"]
```

**Build**

```bash
docker build --rm -t local/c7_nologin .
```

**Run**
```
docker run --privileged -it local/c7_nologin
```

## Description

使用 [Centos7](https://hub.docker.com/_/centos) 官方 Docker 镜像制作。


由于默认镜像中没有启用用户，虽然使用命令  `docker run --privileged  -it -e "container=docker" centos:7  /usr/sbin/init` 也可以启动 Centos，但是会提示登录。而且系统默认没有启用任何用户。所以是无法登录到系统中的。具体可以通过 `docker run -it centos:7` 查看 `/etc/shadow` 文件中 `root` 用户。

### 启用 `console-getty` autologin

根据大家在使用 Centos 时，配置自动登录的思路，和 Linux 文档，同时结合手动更改 root 密码后进入系统查看进程得出的结论。[Centos7 官方 Docker 镜像](https://hub.docker.com/_/centos) 在登录进系统时使用的是 `Nspawn console` 方式，而不是 `Virtual console`。因此在构建过程中通过修改 `console-getty.service` 完成对 root 用户的无密码登录

参考博文：

[Autologin in CentOS 7 for TTY and GDM](http://xinerama.blogspot.com/2016/03/autologin-in-centos-7.html?tdsourcetag=s_pctim_aiomsg)

[getty - ArchWiki](https://wiki.archlinux.org/index.php/Getty#Automatic_login_to_virtual_console)

**注意：**

在 [Login to CentOS 7 without Login Prompt](http://mazzakolinux.com/login-to-centos-7-without-login-prompt/?tdsourcetag=s_pctim_aiomsg) 中对配置 TTY 自动登录操作相比 [Autologin in CentOS 7 for TTY and GDM](http://xinerama.blogspot.com/2016/03/autologin-in-centos-7.html?tdsourcetag=s_pctim_aiomsg) 中的操作更加简便，而且是没有问题的。我已经使用 VMware 虚拟机试验过了。但是在 Docker 中直接将 `console-getty.service` 文件放到 `/etc/systemd/system/console-getty.service.d/` 中，然后修改后，是不起作用的。唯有在上一级目录也存在这个文件，原理不是太明白。为了使两个文件配置的达到一致，我采用了软链接的方式。

## Other

### 更改 Docker 容器中的 root 密码

如果有要更改 root 密码的需求，可以使用如下方法

上述 Dockerfile 在构建过程中使用 `echo "000000" | passwd --stdin root` 更改密码。参考 [Root password inside a Docker container](https://stackoverflow.com/questions/28721699/root-password-inside-a-docker-container)

当然也可以配置其他用户，同时搭配默认登录其他用户

----

### Other reference

[Login to CentOS 7 without Login Prompt](http://www.voidcn.com/article/p-xfhbbwyp-bps.html)

[配置centos7解决 docker Failed to get D-Bus connection 报错](https://blog.csdn.net/xiaochonghao/article/details/64438246)