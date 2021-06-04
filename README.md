# AWSlearning

## 基础知识

+ Linux 基础知识
+ 一种编程语言(Node.js, Python, GO, Java, ect.)
+ 网络基础知识(TCP/IP, HTTP/HTTPS, SSH/SCP, Network, ect.)
+ 数据库基础知识 (RDB, SQL, NOSQL, etc.)
+ 容器基础知识 (Docker, K8S)

## 课件内容
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

## 课程目标：
01. 系统架构能力
    + 实现弹性架构
    + 实现高可用架构
    + 实现安全架构
    + 实现成本优化


# 系统架构

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