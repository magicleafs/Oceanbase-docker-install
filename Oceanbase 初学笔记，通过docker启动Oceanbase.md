# Oceanbase 初学笔记，通过docker启动Oceanbase

环境准备：

* 一台不少于10G运行内存的物理机（或者公有云）
* Docker 平台
* ssh连接工具(推荐Finalshell)
* 国外网站加速（否则你访问国外网站会很痛苦）

需求知识:

* 基本的linux命令

* 会注册公有云账号

  

## 教程开始

*  购买vps(或者虚拟机)

  这里我用的是[Vultr](https://www.vultr.com/) ,国外商家不限制带宽，价格也便宜，我选的这家是按小时付费。

  配置我们推荐16G内存，系统centos7,8都可以,选中好vps购买开机后就可以连接了。

* 连接主机

  使用Finalshell连接主机

* 安装Docker

  我们需要在一台新开的机子安装Docker,使用如下命令：

  ```shell
  yum install -y docker
  ```

  

* 启动Docker

  ```shell
  systemctl start docker
  ```

  

* 拉取镜像仓库

  ```shell
  docker pull oceanbase/obce-mini
  ```

  

* 运行该镜像:

  ```shell
  docker run -p 2881:2881 --name some-obce -d oceanbase/obce-mini
  ```

  

* 查看是否运行成功

  ```
  docker logs some-obce | tail -1
  ```

  返回boot success!则为成功

  ## 连接 OceanBase 实例

  obce-mini 镜像安装了 OceanBase 数据库客户端 obclient，并提供了默认连接脚本 ob-mysql。

  ```bash
  docker exec -it some-obce ob-mysql sys # 连接 sys 租户
  docker exec -it some-obce ob-mysql root # 连接用户租户的 root 账户
  docker exec -it some-obce ob-mysql test # 连接用户租户的 test 账户
  ```

  您也可以运行以下命令，使用您本机的 obclient 或者 MySQL 客户端连接实例。

  ```bash
  $mysql -uroot -h127.1 -P2881
  ```

  连接成功后，终端将显示如下内容：

  ```mysql
  Welcome to the MySQL monitor.  Commands end with ; or \g.
  Your MySQL connection id is 167310
  Server version: 5.7.25 OceanBase 3.1.0 (r-00672c3c730c3df6eef3b359eae548d8c2db5ea2) (Built Jun 22 2021 12:46:28)
  
  Copyright (c) 2000, 2021, Oracle and/or its affiliates.
  
  Oracle is a registered trademark of Oracle Corporation and/or its
  affiliates. Other names may be trademarks of their respective
  owners.
  
  Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
  
  mysql>
  ```

  

  

