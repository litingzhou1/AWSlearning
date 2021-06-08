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
    + 建立互联网网关: *deeplearnaws-igw*
    + 绑定到VPC *deeplearnaws-vpc* 上

### 建立公网路由表 - 让我们的子网连接到互联网
+ RTB: deeplearnaws-web-rtb
  * VCP: deeplearnaws-vpc
  * Rule: 加入 deeplearnaws-igw
+ 把新路由绑定到 deeplearnaws-web-1a 子网

### 公私网


### 区域（Regions）和可用区（Availability Zones）
+ 区域（Regions）
+ 可用区（Availability Zones
    - 一个区分成多个可用区
    - 保险起见，企业设计系统要多个区和多个可用区

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