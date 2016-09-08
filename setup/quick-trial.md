# 单机试用

## 一. 环境检查

1. 请确保 docker 安装版本 >=1.12 并正常运行, 我们可以通过命令 `docker version` 或者 `docker info` 来确认 docker 的运行状态(如何安装和配置 docker 请参考 https://docs.docker.com/engine/installation/)。
2. 请确保 docker-compose 已经正确安装(如何安装 docker-compose 请参考 https://docs.docker.com/compose/install/)。
3. docker，swarm 本身对操作系统还有一些设置需求，譬如主机时钟同步，selinux，docker daemon tcp 服务开启等。这些会在程序安装时自动进行检查, 请在安装时按提醒进行相应设置。

## 二. 安装

4. 执行内测邮件中的安装命令，在安装过程中会提醒你输入当前主机的 IP 。
5. 安装成功后通过浏览器访问 http://$IP 即可，默认用户名：admin@admin.com 密码：adminadmin
