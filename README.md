# AWSlearning

## Class1
### 基础知识
+ Linux 基础知识
+ 一种编程语言(Node.js, Python, GO, Java, ect.)
+ 网络基础知识(TCP/IP, HTTP/HTTPS, SSH/SCP, Network, ect.)
+ 数据库基础知识 (RDB, SQL, NOSQL, etc.)
+ 容器基础知识 (Docker, K8S)
### 课件内容
01. 组建规划网络
    + VPC - 虚拟网络
    + Subnet - 子网络
    + ACL - 访问控制列表，分配存取列表  * 有何异同
    + SecurityGroup - 分配安全组 * 有何异同
    + RouteTable - 路由表
02. 设计网络应用
    + EC2 - 搭建虚拟服务器
    + ELB - 负载均衡
    + Database  
    + Auto Scaling - 利用弹性伸缩，几乎适合所有集群式部署的网站/APP
    + MultiAZ - 高可用
    + MultiRegion
03. AWS服务
    + VPC - 虚拟网络
    + EC2 - 服务器
    + S3 - 文件存取 ***
    + IAM - 控制用户权限
    + RDS - 关系型数据库
    + DynamoDB - 非关系型数据库
    + Lambda - 是一种无服务器的计算服务*** 
    + API Gateway
    + Route 53 - 路由
    + CloudTrail - 跟踪用户行为
    + CloudWatch - 监控系统行为
    + Elastic Beanstalk - 自动部署代码
    + SQS - 消息队列服务
    + ECR - 存储docker images(够将Docker容器映像和相关的OCI构件存储到您的存储库）
    + ECS - 高度可扩展的软件容器管理服务
04. 开发流程
    + CodeCommit
    + CodeBuild
    + CodeDeploy  
    + CodePipeline
04. 进阶应用
    + Config
    + System Manager
    + CloudFormation - 一项典型的(IAC)基础架构即代码服务
    + OpsWorks
    + IoT
    + 机器学习
    + 工作流
    + 移动开发 - AWS Amplify
    + CLI
    + SDK（Node.js，Python，Go）

## Class 2
### 用户
避免使用超级管理员root根账号，应该分配适当有权账号进行工作管理
01. root 根账户
    + 控制其他管理账户时
    + 账号契约解约时
    + 账号付款时
02. 普通管理员
    + 管理一般用户（User，Group，Role）
    + 管理AWS资源（EC2，RDB，...）

IAM(identity and Acess Management)
+ 简化用户登录URL - deeplearnaws
    - 自定义URL里的账户ID
+ 建立管理员组 - lcadmin-group
    - 组 -> AdminstationAcess
+ 建立普通管理员 - lcadmin
    - 用户 - 添加到组 - 创建用户
登陆时用IAM用户

### 设置CloudWatch - 监控成本

### 设置CloudTrail - 记录系统中用户或API的活动行为（Who，When，What）- 用于企业

注意点
+ CloudTrail 本体免费(真实的谎言)，如果记录特殊的分析事件需要另行付费
+ CloudTrail 默认免费记录90天操作日志
+ 开启系统跟踪服务后，日志会写入S3存储桶，S3存储桶另行收费

### 区域（Regions）和可用区（Availability Zones）
+ 区域（Regions）
+ 可用区（Availability Zones
    - 一个区分成多个可用区
    - 保险起见，企业设计系统要多个区和多个可用区

### 设计一个Web应用的网络结构 
知识点：
+ 设计简单的网络，部署简单的web应用


### 建立VPC - 建立自己的IDC（互联网数据中心）
+ 知识点
    + 建立VPC(Virtual Private Cloud)
        + 可以把VPC理解为我们自己私有的IDC(互联网数据中心)
+ 网段规划：
    + CIDR（无类别域间路由：是一个用于给用户分配IP地址以及在互联网上有效地路由IP数据包的对IP地址进行归类的方法）
        * 172.16.0.0/16 <- 看图说话
    + 标签
        * Name:deeplearnaws-vpc

### 网络IP范围设计 - IP Network
设计系统分成两个子网
+ 公开网络 - 用于对外公开的服务器 - Web， API服务器
    - 网段决定:
        - 172.16.10.0/24
        - 172.16.11.0/24
        - ap-northeast-web-1a
    - 标签
        - Name：ap-northeast-web-1a （AR）
+ 私有网络 - 用于内部使用的服务器， 如：DB， Redis等
    - 网段决定:
        - 172.16.20.0/24
        - 172.16.21.0/24
        - ap-northeast-web-db-1a （AR）
+ 区别：公有网络的服务器外部可以通过IP地址访问，容易被黑。私有网络，外部无法通过IP访问，可以通过公有网络服务器访问
+ 扩展性考虑
    - 考虑到系统整体可用性，今后需要将服务部署到多个AZ区，所以应该提前做好IP地址的规划

子网所属VPC
+  deeplearnaws-vpc 

### 建立互联网网关 - 公开网络和私有网络的主要区别，上面创建的两个subnet现在都不是公开的，现在要创建网关
建立互联网网关，绑定到VPC，使VPC子网可以连上互联网
+ 绑定到VPC(deeplearnaws-vpc)上
    + 建立互联网网关: *deeplearnaws-igw*    （igw - internet gateway）
    + 绑定到VPC *deeplearnaws-vpc* 上

### 建立公网路由表 - 让我们的子网连接到互联网

+ RTB: deeplearnaws-web-rtb 
  * VCP: deeplearnaws-vpc
  * Rule: 加入 deeplearnaws-igw
+ 把新路由绑定到 deeplearnaws-web-1a 子网

### 两个防火墙 - 网络ACL和安全组SG （两层数据过滤）
* 网络存取控制列表 - ACL（侦听所有的数据流量，针对整个VPC生效）
* 安全组 - Security Group（针对AWS资源生效）
* 任务：
    + Name: deeplearnaws-web-sg
    + Port: SSH(22)

### 公私网

## Class 3
### 建立EC2 - 云端服务器
建立一个公网可访问的EC2服务器实例
+ EC2实例具体设置
    + Amazon Linux 2 AMI - Amazon Machine Image
    + t2.micro 
    + deeplearnaws-vpc - EC2 装在哪个VPC
    + deeplearnaws-web-1a - EC2 装在哪个子网
    + 自动分配公有 IP
    + 内网IP: 172.16.10.10/32
    + Name: deeplearnaws-web1a - 服务器名
    + 密钥对: deeplearnaws-ssh-key - 
+ SSH连接
    ```bash
    $ cp ~/Downloads/deeplearnaws-ssh-key.pem ./
    $ chmod 400 deeplearnaws-ssh-key.pem - 设置本地权限
    # Amazon Linux 2
    $ ssh -i ./deeplearnaws-ssh-key.pem ec2-user@13.115.167.6
    >
    ```
### 建立EC2 - 云端服务器
建立AMI - 克隆自己的系统
+ 知识点：
    * 在Amazon Linux 2上安装必要的软件包
    * 打包Amazon Linux 2成AMI(Amazon Machine Image)
    * 从AMI启动我们的EC2实例
+ 在Amazon Linux 2上安装必要的软件包
    + Docker
    + Node.js
    + Git

    ```bash
    ##### Amazon Linux 2
    $ ssh -i ./deeplearnaws-ssh-key.pem ec2-user@54.173.158.89
    ##### 登陆安全信息
    $ touch 
    ##### 系统升级
    $ sudo yum update -y
    ##### 确认linux 系统版本
    $ cat /etc/os-release
    ############### DOCKER ####################################
    ##### Docker: amazon-linux-extras 扩展包安装docker
    $ sudo amazon-linux-extras install -y docker
    # 修改docker 附加组给 ec2-user
    $ sudo usermod -a -G docker ec2-user
    # 启动docker服务 
    $ sudo systemctl start docker 
    # 确认 docker 运行
    $ ps aux|grep docker
    # 注册 docker 服务
    $ sudo systemctl enable docker
    # 安装 docker-compose
    $ sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
    $ sudo chmod +x /usr/bin/docker-compose
    $ docker-compose version
    ############### Node.js ####################################
    $ curl -sL https://rpm.nodesource.com/setup_12.x | sudo bash -
    $ sudo yum install -y gcc-c++ make
    $ sudo yum install -y nodejs
    $ node -v
    ############### Git ####################################
    $ sudo yum install git -y
    $ git version
    # 可选安装nmap， 扫描端口
    $ sudo yum install -y nmap
    # 重启
    $ sudo init 0
   ```

+ 将系统打包成AMI
    - Name: deeplearnaws-ami

### 部署一个Web应用 - 建立Node.js+Docker的应用程序
+ 知识点
    * 部署一个 Node.js 的 Web 应用
    * 把应用通过 Docker(Hub) 方式进行部署
    * 修改安全组，追加开放 80 端口
+  今后预定
    + 部署到 ECR + ECS
+ 实战：
    + Docker Hub：deeplearnaws-web
    + 修改安全组， 追加开放80端口
        ```bash
        # 从DockerHub上获取deeplearnaws-web镜像
        $ docker pull komavideo/deeplearnaws-web:latest
        $ docker run --name deeplearnaws-web -p 80:3000 -d komavideo/deeplearnaws-web:latest
        $ docker container ls -a
        $ docker logs -f deeplearnaws-web
        $ docker container stop deeplearnaws-web
        $ docker container rm deeplearnaws-web
        $ docker ps
        # 编辑调整
        $ docker exec -it deeplearnaws-web sh
        >root $ vi app.js
        $ docker container restart deeplearnaws-web
        ```
 ### 部署一个DB应用
+ 知识点： 
    * 在公开网络中建立NAT网关
    * 建立DB私有网络的路由表, 设置NAT网关
    * 建立DB私有网络的安全组, 开放 3306 - MySQL端口
+ 实战：
    + NAT网关
        + Name: deeplearnaws-web-nat
        + SubNet: deeplearnaws-web-1a
        + Elastic IP
    + 路由表
        + Name: deeplearnaws-db-rtb
        + Target: deeplearnaws-web-nat -> 0.0.0.0/0
    + 安全组
        + Name: deeplearnaws-db-sg
        + Port: 3306
连接到公网的两个条件：
    公网IP
    Internet网关

### 建立私有网段的服务器 - For MySQL
+ 知识点：
    * 修改私有网段安全组，增加 Port:22 支持
    * 在私有网段建立 EC2 实例
    * 通过公有网段 web-ec2 实例连接到 db-ec2 实例
+ 实战：
    * 修改私有网段安全组，增加 Port:22 支持
        + deeplearnaws-db-sg
        + Port: 22

    * 在私有网段建立 EC2 实例
        + Name: deeplearnaws-db1
    * 通过公有网段 web-ec2 实例连接到 db-ec2 实例
        + 本地连接到 deeplearnaws-web1
        + 拷贝本地连接私钥到 deeplearnaws-web1
        + 通过 deeplearnaws-web1 连接到 deeplearnaws-db1
        + deeplearnaws-db1测试
        ```bash
        # @deeplearnaws-web1
        $ ssh -i ./deeplearnaws-ssh-key.pem 172.16.20.10
        # @deeplearnaws-db1
        $ sudo yum update -y
        $ curl https://komavideo.com
        ```
### 建立私有网段的服务器 - For MySQL
+ 知识点：
    * 在私有网段实例中安装 MySQL@Docker
    * 在公有网段中去连接 MySQL 数据库实例
+ 实战：
    - 数据库端(deeplearnaws-db1)
    https://hub.docker.com/r/komavideo/deeplearnaws-mysql

    ```bash
    # 应用
    $ docker pull komavideo/deeplearnaws-mysql:latest
    $ docker run --name deeplearnaws-mysql -e MYSQL_ROOT_PASSWORD=12345678 -p 3306:3306 -d --restart=always komavideo/deeplearnaws-mysql:latest
    $ docker ps
    # 进入容器bash-shell
    $ docker exec -it deeplearnaws-mysql bash -p
    >mysql -u root -p -h 127.0.0.1
    mysql> show databases;
    mysql> use blogdb;
    mysql> select * from user;
    mysql> exit;
    ```
    - Web端(deeplearnaws-web1)
    
    ```bash
    $ ssh -i ./deeplearnaws-ssh-key.pem 172.16.20.10
    # 连接数据库
    $ mkdir myapp
    $ cd myapp
    $ npm init -y
    $ npm install mysql --save
    $ nano main.js
    ...
    $ node main.js
    ```
    - main.js
    
    ```js
    const mysql = require('mysql');
    const connection = mysql.createConnection({
        host: '172.16.20.10',
        user: 'root',
        password: '12345678',
        database: 'blogdb'
    });
    connection.connect((err) => {
        if (err) {
            console.log('error connecting: ' + err.stack);
            return;
        }
        connection.query(
            'SELECT * FROM user', (error, results) => {
                console.log(results);
                process.exit()
            }
        );
    });
    ```

### 通过 Web 应用连接到 MySQL 数据库服务器
+ 知识点：
    * 更新 Web 应用 Docker 版本
    * 确认连接到数据库实例
+ Web镜像(Docker Hub)：https://hub.docker.com/r/komavideo/deeplearnaws-web
+ 实战：
    - 实战练习
    ```bash
    # 从DockerHub上获取deeplearnaws-web镜像
    $ docker pull komavideo/deeplearnaws-web:latest
    $ docker run --name deeplearnaws-web -p 80:3000 -d --restart=always komavideo/deeplearnaws-web:latest
    $ docker container ls -a
    $ docker logs -f deeplearnaws-web
    $ docker container stop deeplearnaws-web
    $ docker container rm deeplearnaws-web
    $ docker ps
    # 编辑调整
    $ docker exec -it deeplearnaws-web sh
    >root $ vi app.js
    $ docker container restart deeplearnaws-web
    ```
    - 打包Web-AMI 
        - Name: deeplearnaws-web-ami

### 负载均衡 - 通过 ELB 设计高可用性服务
+ 知识点：
    * 设计高可用性网络结构
+ 实战演习
    - Web端高可用性设计
    - 操作步骤
        + 建立新的可用区子网 - deeplearnaws-web-1c
        + 在新的子网区添加 web-ec2 实例
        + 通过 ELB 进行多区多实例之间的负载均衡

### 高可用性设计 - 建立新的子网区
+ 知识点：
    * 建立新的可用区子网
    * 在新的子网区添加 web-ec2 实例
+ 实战演习
    - 操作内容
        * 建立新的可用区子网
            + Name: deeplearnaws-web-1c
            + 配置路由表
        * 在新的子网区添加 web-ec2 实例
            + Name: deeplearnaws-web2

### 高可用性设计 - 建立 ALB 负载均衡
+ 知识点：
    * 建立目标组
    * 建立ALB负载均衡器
+ 实战演习
    - 启动Web服务器
        + deeplearnaws-web1(1a)
        + deeplearnaws-web2(1c)
    - 建立目标组
        + Name: deeplearnaws-web-target-group
    - 建立ALB负载均衡器
        + Name: deeplearnaws-web-alb

## Class 4
### Route53 - 为我指引方向
+ 知识点：
    * 使用 Route53 的DNS解析服务
+ 特点：
    + 高度可用且可靠
    + 灵活
    + 专为与其他 Amazon Web Services 配合使用而设计(A记录-ALIAS)
    + 简单
    + 速度快的
    + 经济高效
    + 安全
    + 可扩展
    + 简化混合云
+ 实战演习
    + 申请域名
    + 设置名字服务器NS
    + 使用Route53路由

### 注册域名 - freenom
+ 知识点：
    * 使用免费的域名申请服务 - freenom
    * 注册一个域名
+ 实战演习：
    - 域名注册：deeplearnaws.ml

### 链接Rout53和freenom - 设置托管区(Hosted Zone)和名字服务器(NS)

+ 知识点：
    * 链接Rout53和freenom
    * 设置托管区(Hosted Zone)@Rout53
    * 设置名字服务器(NS)@freenom
    * 域名中的create record which is called an "Alias" record.
+ 实战演习：
    + 在Route53中建立托管区
    + 记录NS服务器地址
    + 在freenom中设置NS

    ```bash
    # check 域名
    nslookup meetingminder.ml
    ```

### Route53 - 解析我的Web服务
+ 知识点：
    * 使用 Route53 解析我的Web服务
+ 采用策略 - 简单路由
    - 对于为您的域执行给定功能的单一资源（例如为 example.com 网站提供内容的 Web 服务器），可以使用该策略。
    - https://docs.aws.amazon.com/zh_cn/Route53/latest/DeveloperGuide/routing-policy.html
+ 实战演习：
    + 实战
    ```bash
    $ dig deeplearnaws.ml @8.8.8.8
    ```
    + 设置DNS解析
        + 启动ec2-web1
        + 设置域名IP解析


### Route53 - 解析我的ELB服务 - 等待域名全球确认
+ 知识点：
    * 使用 Route53 - 解析我的ELB服务
+ 采用策略 - 简单路由
    - 对于为您的域执行给定功能的单一资源（例如为 example.com 网站提供内容的 Web 服务器），可以使用该策略。
    - https://docs.aws.amazon.com/zh_cn/Route53/latest/DeveloperGuide/routing-policy.html
+ 实战演习：
    + 启动ec2-web1, ecs2-web2
    + 确认ELB和目标组状态
    + 设置域名ELB解析
    + 域名访问


### 全球部署 - 新加坡  - Educate 账号无法执行
+ 知识点：
    * 在全球其他区域部署我们的Web服务
+ 实战演习
    + 将东京区的AMI拷贝到新加坡区
    + 从新加坡区启动EC2实例
    + 配置安全组SG，使SSH和HTTP可以访问
    + 配置子网路由表
    + 修改索引显示内容
    ```bash
    $ docker exec -it deeplearnaws-web sh
    >root $ vi app.js
    $ docker container restart deeplearnaws-web
    ```
### Route53 - 分流东京和新加坡区，多值应答路由
+ 知识点：
    + 启动各区的服务器
    + 设置DNS健康检查
        - 东京区IP
        - 新加坡区IP
    + 设置多值应答路由 - global.deeplearnaws.ml
    + 停止东京区服务, 确认DNS解析结果
    + 实战演习

### RDS - 被管理的数据库服务
+ RDS的相关知识
    + RDS是被管理的数据库服务，让开发人员免去系统(OS,DB)管理的麻烦，重点把精力放在业务逻辑的开发
+ RDS 支持的数据引擎：
    + Amazon Aurora(MySQL, PostgreSQL) - scree
    + MySQL
    + MariaDB
    + PostgreSQL
    + Oracle
    + SQL Server
+ 优点：
    + 轻松管理
    + 高度可扩展
    + 可用且持久
    + 速度快
    + 安全
    + 经济实惠(考虑人员维护成本)
+ 结构设计

### MySQL@RDS - 准备工作
+ 知识点
    * 建立RDS MySQL前的准备工作
+ 实战：
    - VPC子网,安全组,DB子网组,参数组,选项组
        + VPC子网
            - Name: deeplearnaws-db-1c
            - CIDR: 172.16.21.0/24
        + 安全组
            - Name: deeplearnaws-db-sg <- 可以直接使用，但生产环境时应只保留3306端口
        + DB子网组
            - Name: deeplearnaws-db-subnet-group
        + 参数组
            - Name: deeplearnaws-db-parameter-group
        + 选项组
            - Name: deeplearnaws-db-option-group

### MySQL@RDS - 建立MySQL数据库服务
+ 知识点
    * 建立 MySQL RDS 数据库服务
+ 实战：
    + 标准创建
    + MySQL
        - 5.7.31
    + 免费套餐
    + 数据库实例标识符
        - komadb
    + 凭证设置
        - root/12345678
    + 数据库实例大小
        - db.t2.micro
    + 存储
        - SSD
        - 20G-1000G
    + 连接
        - deeplearnaws-vpc
    + 子网组
        - deeplearnaws-db-subnet-group
    + 安全组
        - deeplearnaws-db-sg
    + 可用区
        - 任意
    + 密码身份验证
    + 其他配置
        - 初始数据库名称: blogdb

### 连接MySQL - MySQL客户端工具
+ 知识点
    * 安装 MySQL 客户端工具，连接到 MySQL RDS 数据库实例
+ 实战：
    + 安装 MySQL 客户端命令行工具
    + 连接到 MySQL 服务器实例
    + 建立数据表
    + 添加数据
    ```bash
    $ sudo yum -y update
    $ sudo yum -y install mysql
    $ mysql -h mysqldev.cbcbjcjywvwn.ap-northeast-1.rds.amazonaws.com -u root -p
    Enter password:
    mysql> show databases;
    mysql> use blogdb;
    mysql> create table blogdb.user (id int, name varchar(255));
    mysql> show tables;
    mysql> insert into blogdb.user values (1, 'Koma');
    mysql> insert into blogdb.user values (2, 'Xiaoma');
    mysql> insert into blogdb.user values (3, 'MySql');
    mysql> select * from blogdb.user;
    mysql> exit;
    ```
### 连接MySQL - phpMyAdmin管理工具
+ 知识点
    * 使用 phpMyAdmin 管理工具连接 MySQL 数据库实例
+ 实战：
    - docker-compose.yml
    ```yml
    version: '3' 
    services:
        phpmyadmin:
            image: phpmyadmin:latest
            container_name: web_phpmyadmin
            ports:
            - 80:80
            environment:
            - PMA_HOST=mysqldev.cbcbjcjywvwn.ap-northeast-1.rds.amazonaws.com
            - PMA_USER=root
            - PMA_PASSWORD=12345678
    ```

    ```bash
    # 编译服务
    $ sudo docker-compose build
    # 容器启动(精灵线程)
    $ sudo docker-compose up -d
    # 查询容器状态
    $ sudo docker-compose ps
    ```
### 连接MySQL - Node.js Web应用程序
+ 知识点
    * 使用 Node.js Web应用程序连接 MySQL 数据库实例
+ 实战
    * 实战训练
        ```bash
        # 从DockerHub上获取deeplearnaws-web镜像
        $ docker pull komavideo/deeplearnaws-web:latest
        $ docker run --name deeplearnaws-web -p 80:3000 -d --restart=always komavideo/deeplearnaws-web:latest
        $ docker container ls -a
        # 编辑调整
        $ docker exec -it deeplearnaws-web sh
        >root $ vi app.js
        ...
        mysqldev.cbcbjcjywvwn.ap-northeast-1.rds.amazonaws.com
        ...
        $ docker container restart deeplearnaws-web
        ```
### AWS CLI 命令行工具
AWS Command Line Interface (AWS CLI) 是一种开源工具，让您能够在命令行 Shell 中使用命令与 AWS 服务进行交互。仅需最少的配置，即可使用AWS CLI 开始运行命令，以便从终端程序中的命令提示符实现与基于浏览器的 AWS 管理控制台所提供的功能等同的功能
+ AWS操作方法
    + 管理控制台(Browser)
    + AWS CLI
    + AWS SDK
+ 使用方法
    + AWS CLI 本地安装(Windows, Mac, Linux)
    + CloudShell
    + Cloud9
    + EC2(Amazon Linux自带CLI)
        - 演示时间

### AWS CLI 命令行工具
+ 知识点
    * AWS CLI 的安装
+ 实战
    + 安装Mac CLI v2版本： https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/install-cliv2-mac.html
    ```bash
    #################################################
    # 安装版本确认
    $ aws --version
    ```

########################################################################################
# 系统架构
## 目标：
01. 系统架构能力
    + 实现弹性架构
    + 实现高可用架构
    + 实现安全架构
    + 实现成本优化

## 设计系统架构需要考虑的问题
+ 需要几台服务器
+ 每个服务器的功能(Web, Cache, DB,...)
+ 需要什么硬件配置的服务器(CPU, MEM, HDD/SDD,...)
+ 服务器放在哪里(IDC, Cloud,...)
+ 服务器需要什么OS(Linux, Windows,...)
+ 服务器需要什么软件(Apache,Nginx,Tomcat,MySQL,PostgreSQL,...)
+ 网络构成是怎样的(客户端，中间层，服务层，数据层，分析层，AI层，...)
+ 网络IP该如何规划(根据角色分配IP范围)
+ 需要几个子网(对外公开，对内公开)
+ 如何设计网络安全(服务访问授权，跟踪，数据加密，传输加密，WAF，DDos，...)
+ 如果与本地连接(公司内网和云端通信机制)
+ 如何提高服务可用性(负载均衡，多区域部署)
+ 如何节省成本(性价比)

![system architec](/images/basic_system_arcte.png)

# 