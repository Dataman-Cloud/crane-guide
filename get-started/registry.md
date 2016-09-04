# 镜像

镜像或镜像管控不仅可以通过私有/公有镜像来管理、分享 Docker 镜像，而且还可利用应用目录提供一键下发的应用。

**私有镜像**：私有镜像功能实现了用户管理与镜像管控的闭环，通过对接企业内部用户系统，用户可以使用内部的账号/密码无缝推送(docker push)镜像到私有镜像仓库，拉取(docker pull)亦然。我们以系统自带的默认账号 `admin@admin.com` 为例展示下如何向镜像仓库添加私有镜像。

1. 登录到镜像仓库： `docker login $REGISTRY_ADDR -uadmin@admin.com -pYOUR_PASSWORD`
2. 推送镜像到仓库： `docker push 123.59.56.132:5000/admin/IMAGE_NAME`

注意： 由于默认的 registry 没有配置证书，push/pull 镜像之前确保 docker daemon 启动参数 --insecure-registry=REGISTRY_IP:5000

**公有镜像**：结合用户管理功能，用户可以将自己的镜像公开给其他人使用，达到资源共享。

**应用目录**：应用目录可以帮助企业用户打造企业内部的镜像市场，一键下发公共应用，提高运维效率。
