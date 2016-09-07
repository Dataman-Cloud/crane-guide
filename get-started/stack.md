# 应用

应用或者应用编排(Application Stack)是 Docker 1.12 引入的概念，目前还是 experimental 的功能，必须得安装 experimental 的包才可以尝试。除去编排(stack), Docker 1.12 还引入了服务(service)和任务(task) 的概念，Docker 借此重新阐述了应用与容器(container)之间的关系。

一个应用编排代表一组有依赖关系的服务(譬如 wordpress 服务和 mysql 服务)，服务之间可以相互发现，每个服务由多个任务组成，任务的数量可以扩缩(scale)，而任务则物化为一个具体的 Docker 容器及其配置。

目前可以通过以下三种方式发布应用：

**DAB 创建**:
我们可以通过一个 JSON 文件快速发布一个应用，JSON 文件的格式如下：

  ```  		
  json
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
如果还来不及对 JSON 文件的格式有更多了解，可以通过向导方式逐配置应用参数。

* 点击创建应用--向导创建--填写应用名称－－填写服务信息－－部署
* 服务信息：
   * 服务名称: 为服务命名，必须为英文或数字。
   * 镜像名称: 服务使用的镜像，主机已存在的镜像或者为集群可以pull的镜像。
   * 服务模式: 设定服务模式类型，分为固定实例数和一节点一任务模式。
   * 选择网络:  服务使用的网络，需要在网络里预先创建。
   * 认证标识: 预先设置的仓库认证信息，如果使用的镜像仓库需要登录验证，可以预先在`仓库认证`处设置登录信息的用户名、密码，设置不同的标签；此处选择相应的认证标签。
   * 端口映射:建立宿主机的端口与容器端口的映射关系，支持`tcp`和`udp`协议。
   * 环境变量：设置容器启动时的环境变量。
   * 启动参数：
      * 容器工作目录：容器内工作路径，如设置`/root`，容器内工作路径在`/root`，如需执行`／root／test.sh`，此处设置`/root`，命令行设置`sh test.sh`即可;
      * 命令行：容器启动的运行命令。
      * 参数：为命令行提供参数。   
  * 标签： 可以分别为服务和服务添加标签。 
  * 资源限制：设定服务每个容器的限制资源及预留资源。
  * 容错策略：针对容器健康状态设置的重启策略，分别为`退出`、`失败`，`退出`指只要容器`退出`了,重启策略即生效，`失败`指容器非正常退出，重启策略才生效，若不设置重启策略，可以选择`从不`。
    *  评估间隔：设定的时间用来评估重启政策。
    *  重启间隔：容器重启的时间间隔。
    *  尝试次数：尝试次数后，不再重启。
  * 更新策略：服务更新时执行的策略
    * 间隔：更新间隔，通常指更新的并行数<现有实例数；二次更新时的间隔时间。
    * 并行数：一次更新的最大实例数
    * 失败后策略，如果更新失败执行的策略，分`继续尝试`和`立即停止`两种。
  * 调度策略：指服务发布的容器的调度分布，可以将服务的容器发布到指定的`node`节点或者指定`label`的节点,`label`需要预先编辑，label为`key`：`value`格式，
     * 限定label输入方式`node.labels.yourkey`:`yourvalue`
     * 限定节点id输入方式`node.id`:`yournodeid`
     * 限定hostname的输入方式`node.hostname`:`yourhostname`
     
  * 文件挂载：挂载宿主机路径到容器内，可以选择该路径到属性，如`只读`，`读写`。`source`为主机路径,`taget`为容器内路径。
  * 部署：发布应用
  * 部署并导出：发布应用的同时，将配置信息的json文件导出。
  * 增加服务：在该应用下增加新服务。如果增加的新服务在编辑时可以删除。
  
  
  


**快捷创建**:
除去向导创建，用户也可以从应用目录一键下发已经设置好的应用。目前设置应用目录的接口暂为开放。
