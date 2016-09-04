# 应用

应用或者应用编排(Application Stack)是 Docker 1.12 引入的概念，目前还是 experimental 的功能，必须得安装 experimental 的包才可以尝试。除去编排(stack), Docker 1.12 还引入了服务(service)和任务(task) 的概念，Docker 借此重新阐述了应用与容器(container)之间的关系。

一个应用编排代表一组有依赖关系的服务(譬如 wordpress 服务和 mysql 服务)，服务之间可以相互发现，每个服务由多个任务组成，任务的数量可以扩缩(scale)，而任务则物化为一个具体的 Docker 容器及其配置。

目前可以通过以下三种方式发布应用：

**DAB 创建**:
我们可以通过一个 JSON 文件快速发布一个应用，JSON 文件的格式如下：

  ```json
  {
    "Services": {
      "mysql": {
        "Name": "mysql",
        "Labels": {
          "name": "mysql"
        },
        "TaskTemplate": {
          "ContainerSpec": {
            "Image": "mysql",
            "Labels": {
              "name": "mysql"
            },
            "Env": [
              "MYSQL_ROOT_PASSWORD=wordpress",
              "MYSQL_PASSWORD=wordpress",
              "MYSQL_USER=wordpress",
              "MYSQL_DATABASE=wordpress"
            ],
            "User": "root"
          },
          "RestartPolicy": {
            "Condition": "none",
            "Delay": 3000000000,
            "MaxAttempts": 1,
            "Window": 3000000000
          },
          "LogDriver": {
            "Name": "json-file"
          }
        },
        "Mode": {
          "Replicated": {
            "Replicas": 1
          }
        },
        "UpdateConfig": null,
        "Networks": [
          "ingress"
        ],
        "EndpointSpec": {
          "Mode": "vip",
          "Ports": [
            {
              "Name": "testport",
              "Protocol": "tcp",
              "TargetPort": 3306,
              "PublishedPort": 3306
            }
          ]
        }
      },
      "wordpress": {
        "Name": "wordpress",
        "Labels": {
          "wordpress": "wordpress"
        },
        "TaskTemplate": {
          "ContainerSpec": {
            "Image": "wordpress",
            "Labels": {
              "name": "wordpress"
            },
            "Env": [
              "WORDPRESS_DB_HOST=mysql:3306",
              "WORDPRESS_DB_PASSWORD=wordpress"
            ],
            "User": "root"
          },
          "RestartPolicy": {
            "Condition": "none",
            "Delay": 3000000000,
            "MaxAttempts": 1,
            "Window": 3000000000
          },
          "LogDriver": {
            "Name": "json-file"
          }
        },
        "Mode": {
          "Replicated": {
            "Replicas": 1
          }
        },
        "UpdateConfig": null,
        "Networks": [
          "ingress"
        ],
        "EndpointSpec": {
          "Mode": "vip",
          "Ports": [
            {
              "Name": "testport",
              "Protocol": "tcp",
              "TargetPort": 80,
              "PublishedPort": 8000
            }
          ]
        }
      }
    },
    "Version": "1"
  }
  ```

[了解更多](https://github.com/docker/docker/blob/master/experimental/docker-stacks-and-bundles.md)

**向导创建**:
如果还来不及对 JSON 文件的格式有更多了解，可以通过向导方式逐步配置应用参数。

**快捷创建**:
除去向导创建，用户也可以从应用目录一键下发已经设置好的应用。
