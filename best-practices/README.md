###快速发布zookeeper集群


ZooKeeper是一个分布式的，开放源码的分布式应用程序协调服务，是Google的Chubby一个开源的实现，是Hadoop和Hbase的重要组件。它是一个为分布式应用提供一致性服务的软件，提供的功能包括：配置维护、域名服务、分布式同步、组服务等。

####准备dab文件


		{
    "Version":"",
    "Services":{
        "node1":{
            "Name":"node1",
            "RegistryAuth":"",
            "Labels":{

            },
            "TaskTemplate":{
                "ContainerSpec":{
                    "Image":"zookeeper",
                    "Labels":{

                    },
                    "Command":[

                    ],
                    "Env":[
                        "ZOO_SERVERS=server.1=node1:2888:2888 server.2=node2:2889:3889 server.3=node3:2890:3890",
                        "ZOO_MY_ID=1"
                    ],
                    "Dir":"",
                    "User":"",
                    "Mounts":[

                    ],
                    "StopGracePeriod":null,
                    "Args":[

                    ]
                },
                "Resources":{
                    "Limits":{
                        "NanoCPUs":null,
                        "MemoryBytes":null
                    },
                    "Reservations":{
                        "NanoCPUs":null,
                        "MemoryBytes":null
                    }
                },
                "RestartPolicy":{
                    "Condition":"any",
                    "Delay":null,
                    "MaxAttempts":null,
                    "Window":null
                },
                "Placement":{
                    "Constraints":[

                    ]
                },
                "LogDriver":{

                }
            },
            "Mode":{
                "Replicated":{
                    "Replicas":1
                }
            },
            "UpdateConfig":{
                "Parallelism":null,
                "Delay":null,
                "FailureAction":"continue"
            },
            "Networks":[
                "ingress"
            ],
            "EndpointSpec":{
                "Mode":"vip",
                "Ports":[
                    {
                        "Name":"",
                        "Protocol":"tcp",
                        "TargetPort":2181,
                        "PublishedPort":2181
                    },
                    {
                        "Name":"",
                        "Protocol":"tcp",
                        "TargetPort":2888,
                        "PublishedPort":2888
                    },
                    {
                        "Name":"",
                        "Protocol":"tcp",
                        "TargetPort":3888,
                        "PublishedPort":3888
                    }
                ]
            }
        },
        "node2":{
            "Name":"node2",
            "RegistryAuth":"",
            "Labels":{

            },
            "TaskTemplate":{
                "ContainerSpec":{
                    "Image":"zookeeper",
                    "Labels":{

                    },
                    "Command":[

                    ],
                    "Env":[
                        "ZOO_SERVERS=server.1=node1:2888:2888 server.2=node2:2889:3889 server.3=node3:2890:3890",
                        "ZOO_MY_ID=2"
                    ],
                    "Dir":"",
                    "User":"",
                    "Mounts":[

                    ],
                    "StopGracePeriod":null,
                    "Args":[

                    ]
                },
                "Resources":{
                    "Limits":{
                        "NanoCPUs":null,
                        "MemoryBytes":null
                    },
                    "Reservations":{
                        "NanoCPUs":null,
                        "MemoryBytes":null
                    }
                },
                "RestartPolicy":{
                    "Condition":"any",
                    "Delay":null,
                    "MaxAttempts":null,
                    "Window":null
                },
                "Placement":{
                    "Constraints":[

                    ]
                },
                "LogDriver":{

                }
            },
            "Mode":{
                "Replicated":{
                    "Replicas":1
                }
            },
            "UpdateConfig":{
                "Parallelism":null,
                "Delay":null,
                "FailureAction":"continue"
            },
            "Networks":[
                "ingress"
            ],
            "EndpointSpec":{
                "Mode":"vip",
                "Ports":[
                    {
                        "Name":"",
                        "Protocol":"tcp",
                        "TargetPort":2181,
                        "PublishedPort":2182
                    },
                    {
                        "Name":"",
                        "Protocol":"tcp",
                        "TargetPort":2888,
                        "PublishedPort":2889
                    },
                    {
                        "Name":"",
                        "Protocol":"tcp",
                        "TargetPort":3888,
                        "PublishedPort":3889
                    }
                ]
            }
        },
        "node3":{
            "Name":"node3",
            "RegistryAuth":"",
            "Labels":{

            },
            "TaskTemplate":{
                "ContainerSpec":{
                    "Image":"zookeeper",
                    "Labels":{

                    },
                    "Command":[

                    ],
                    "Env":[
                        "ZOO_SERVERS=server.1=node1:2888:2888 server.2=node2:2889:3889 server.3=node3:2890:3890",
                        "ZOO_MY_ID=3"
                    ],
                    "Dir":"",
                    "User":"",
                    "Mounts":[

                    ],
                    "StopGracePeriod":null,
                    "Args":[

                    ]
                },
                "Resources":{
                    "Limits":{
                        "NanoCPUs":null,
                        "MemoryBytes":null
                    },
                    "Reservations":{
                        "NanoCPUs":null,
                        "MemoryBytes":null
                    }
                },
                "RestartPolicy":{
                    "Condition":"any",
                    "Delay":null,
                    "MaxAttempts":null,
                    "Window":null
                },
                "Placement":{
                    "Constraints":[

                    ]
                },
                "LogDriver":{

                }
            },
            "Mode":{
                "Replicated":{
                    "Replicas":1
                }
            },
            "UpdateConfig":{
                "Parallelism":null,
                "Delay":null,
                "FailureAction":"continue"
            },
            "Networks":[
                "ingress"
            ],
            "EndpointSpec":{
                "Mode":"vip",
                "Ports":[
                    {
                        "Name":"",
                        "Protocol":"tcp",
                        "TargetPort":2181,
                        "PublishedPort":2190
                    },
                    {
                        "Name":"",
                        "Protocol":"tcp",
                        "TargetPort":2888,
                        "PublishedPort":2890
                    },
                    {
                        "Name":"",
                        "Protocol":"tcp",
                        "TargetPort":3888,
                        "PublishedPort":3890
                    }
                ]
            }
        }
     }
    }
    
    
   
  
  
####创建应用

点击创建`创建应用`--`选择DAB创建`--`填写应用名称`--`选择dab文件`--`部署`。

等待系统拉取镜像成功，应用即可运行。

**注**  示例文件许多非必要参数未赋值，可以根据实际情况填写。如果涉及端口改变，需要修改对应的环境变量值。

应用发布成功后，可以通过进入容器内部，直接通过进入容器内部，（点击`容器id`--`终端`）执行`./bin/zsServer.sh status` 查看该容器的节点属性`leader`or`follow`.

