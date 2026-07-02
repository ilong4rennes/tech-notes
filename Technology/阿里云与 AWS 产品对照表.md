
| 阿里云产品                     | AWS 对应产品                                               | 它们是干什么的                                  | 在计算机里的领域                   |
| ------------------------- | ------------------------------------------------------ | ---------------------------------------- | -------------------------- |
| **云服务器 ECS**              | **Amazon EC2**                                         | 在云上租一台虚拟电脑，可以安装 Linux、部署网站、运行程序          | 计算机硬件、操作系统、虚拟化、IaaS        |
| **GPU 云服务器**              | **EC2 GPU Instances**                                  | 租带 GPU 的服务器，用于大模型训练、推理、图形计算和科学计算         | GPU、并行计算、AI 基础设施           |
| **弹性裸金属服务器 EBM**          | **EC2 Bare Metal**                                     | 直接租用物理服务器，不经过普通虚拟机隔离层                    | 计算机体系结构、服务器硬件、IaaS         |
| **弹性伸缩 ESS**              | **EC2 Auto Scaling**                                   | 用户增加时自动增加服务器，用户减少时自动缩减                   | 分布式系统、自动化运维、弹性计算           |
| **镜像服务 ECS Image**        | **Amazon Machine Images，AMI**                          | 保存已经装好系统和软件的服务器模板                        | 操作系统、虚拟化、环境管理              |
| **对象存储 OSS**              | **Amazon S3**                                          | 存放图片、视频、文档、备份文件等海量文件                     | 存储系统、对象存储、分布式系统            |
| **云盘 ESSD**               | **Amazon EBS**                                         | 给云服务器挂一块可以长期保存数据的虚拟硬盘                    | 块存储、磁盘、文件系统                |
| **文件存储 NAS**              | **Amazon EFS**                                         | 多台服务器共同访问同一个网络文件夹                        | 文件系统、网络存储、分布式存储            |
| **云备份 Cloud Backup**      | **AWS Backup**                                         | 对服务器、数据库和文件进行统一备份与恢复                     | 数据备份、容灾、数据管理               |
| **关系型数据库 RDS**            | **Amazon RDS**                                         | 云厂商帮助管理 MySQL、PostgreSQL、SQL Server 等数据库 | 关系型数据库、SQL、PaaS            |
| **PolarDB**               | **Amazon Aurora**                                      | 云原生关系型数据库，强调高性能、扩展和高可用                   | 分布式数据库、云原生数据库              |
| **云数据库 Tair/Redis**       | **Amazon ElastiCache**                                 | 把常用数据放进内存，提高访问速度                         | 缓存、内存数据库、NoSQL             |
| **云数据库 MongoDB**          | **Amazon DocumentDB**                                  | 保存类似 JSON 的文档型数据                         | NoSQL、文档数据库                |
| **表格存储 Tablestore**       | **Amazon DynamoDB**                                    | 保存超大规模、结构比较灵活的键值或表格数据                    | NoSQL、键值数据库、分布式数据库         |
| **专有网络 VPC**              | **Amazon VPC**                                         | 在云上划出一个属于自己的私有网络                         | 计算机网络、网络隔离、IaaS            |
| **交换机 vSwitch**           | **VPC Subnet**                                         | 把一个 VPC 进一步划分成若干子网                       | 子网、IP 地址、路由                |
| **弹性公网 IP，EIP**           | **Elastic IP**                                         | 给云服务器分配固定公网 IP                           | IP 地址、公网通信                 |
| **NAT 网关**                | **NAT Gateway**                                        | 让没有公网 IP 的服务器访问互联网，或进行地址转换               | NAT、网络地址转换、路由              |
| **云企业网 CEN**              | **AWS Transit Gateway**                                | 连接多个 VPC、分支机构和不同区域的网络                    | 广域网、网络互联、企业网络              |
| **负载均衡 SLB**              | **Elastic Load Balancing，ELB**                         | 把用户请求分发给多台服务器                            | 网络、负载均衡、分布式系统              |
| **应用型负载均衡 ALB**           | **Application Load Balancer**                          | 按域名、URL、HTTP 内容分发请求                      | HTTP、七层网络、Web 架构           |
| **网络型负载均衡 NLB**           | **Network Load Balancer**                              | 按 TCP、UDP 等网络连接分发流量                      | TCP/UDP、四层网络               |
| **云解析 DNS**               | **Amazon Route 53**                                    | 把域名转换成服务器 IP，并进行流量调度                     | DNS、域名系统、互联网基础设施           |
| **CDN**                   | **Amazon CloudFront**                                  | 把图片、视频和网页缓存到距离用户更近的节点                    | CDN、缓存、边缘计算、网络             |
| **容器服务 ACK**              | **Amazon EKS**                                         | 托管 Kubernetes 集群，管理大量容器                  | Docker、Kubernetes、云原生、PaaS |
| **容器镜像服务 ACR**            | **Amazon ECR**                                         | 保存和管理 Docker 镜像                          | 容器、镜像仓库、DevOps             |
| **弹性容器实例 ECI**            | **AWS Fargate**                                        | 不管理服务器，直接运行容器                            | Serverless 容器、云原生          |
| **函数计算 Function Compute** | **AWS Lambda**                                         | 上传一段代码，有请求或事件时才运行                        | Serverless、事件驱动、PaaS       |
| **API 网关**                | **Amazon API Gateway**                                 | 对外发布、认证、限流和管理 API                        | Web、HTTP、API、微服务           |
| **Serverless 应用引擎 SAE**   | **AWS App Runner / Elastic Beanstalk**                 | 上传应用，平台帮助部署、扩容和运行                        | PaaS、应用托管、Web 开发           |
| **消息队列 RocketMQ**         | **Amazon MQ / Amazon MSK**                             | 在不同程序之间异步传递消息                            | 消息队列、分布式系统、微服务             |
| **消息服务 MNS**              | **Amazon SQS / SNS**                                   | 实现消息队列和发布订阅                              | 异步通信、事件驱动架构                |
| **Elasticsearch 服务**      | **Amazon OpenSearch Service**                          | 全文搜索、日志检索和数据分析                           | 搜索引擎、大数据、日志分析              |
| **日志服务 SLS**              | **Amazon CloudWatch Logs**                             | 集中收集、查询和分析程序日志                           | 日志系统、可观测性、运维               |
| **云监控 CloudMonitor**      | **Amazon CloudWatch**                                  | 观察 CPU、内存、请求量、错误率等指标                     | 系统监控、可观测性、SRE              |
| **操作审计 ActionTrail**      | **AWS CloudTrail**                                     | 记录谁在什么时候修改了哪些云资源                         | 审计、安全、运维管理                 |
| **资源编排 ROS**              | **AWS CloudFormation**                                 | 用代码批量创建服务器、网络和数据库                        | 基础设施即代码、IaC、DevOps         |
| **云效 DevOps**             | **AWS CodePipeline / CodeBuild**                       | 自动测试、构建和发布代码                             | 软件工程、CI/CD、DevOps          |
| **访问控制 RAM**              | **AWS IAM**                                            | 管理用户、角色和访问权限                             | 身份认证、权限控制、云安全              |
| **密钥管理服务 KMS**            | **AWS KMS**                                            | 创建和管理数据加密密钥                              | 密码学、数据加密、安全                |
| **Web 应用防火墙 WAF**         | **AWS WAF**                                            | 防御 SQL 注入、恶意请求等 Web 攻击                   | Web 安全、应用安全                |
| **DDoS 高防**               | **AWS Shield**                                         | 防御大规模恶意流量攻击                              | 网络安全、DDoS 防护               |
| **云防火墙**                  | **AWS Network Firewall**                               | 控制不同网络和服务器之间的访问流量                        | 防火墙、网络安全、访问控制              |
| **安全中心**                  | **AWS Security Hub / GuardDuty**                       | 检测漏洞、异常行为和安全风险                           | 主机安全、威胁检测、安全运营             |
| **数据传输服务 DTS**            | **AWS Database Migration Service**                     | 在不同数据库之间迁移和同步数据                          | 数据库迁移、数据集成                 |
| **DataWorks**             | **AWS Glue**                                           | 清洗、处理、调度和整合数据                            | 数据工程、ETL、大数据               |
| **MaxCompute**            | **Amazon Redshift / EMR**                              | 对海量数据进行离线计算和数据仓库分析                       | 大数据、数据仓库、分布式计算             |
| **实时计算 Flink 版**          | **Amazon Managed Service for Apache Flink**            | 实时处理日志、交易和用户行为数据                         | 流计算、实时数据处理                 |
| **机器学习平台 PAI**            | **Amazon SageMaker AI**                                | 训练、部署和管理机器学习模型                           | 机器学习、MLOps、AI 平台           |
| **百炼 Model Studio**       | **Amazon Bedrock**                                     | 调用和管理大模型，构建 RAG、Agent 等应用                | 大模型、生成式 AI、MaaS            |
| **向量检索服务**                | **OpenSearch Vector Engine / Bedrock Knowledge Bases** | 保存向量并检索语义相似的文档                           | 向量数据库、RAG、语义搜索             |

AWS 官方把 EC2 定义为按需、可扩展的云计算能力；S3 是对象存储，RDS 是托管关系型数据库。阿里云函数计算和 AWS Lambda 则属于事件驱动的 Serverless 服务，用户不需要直接管理服务器。

## 按之前的课程模块重新归类

|你学习的模块|对应阿里云产品|对应 AWS 产品|
|---|---|---|
|**网络基础**|VPC、vSwitch、EIP、NAT 网关、SLB、云解析 DNS、CDN|VPC、Subnet、Elastic IP、NAT Gateway、ELB、Route 53、CloudFront|
|**硬件与 Linux**|ECS、GPU ECS、裸金属、弹性伸缩|EC2、GPU Instances、Bare Metal、Auto Scaling|
|**存储与数据库**|OSS、云盘、NAS、RDS、PolarDB、Tair、Tablestore|S3、EBS、EFS、RDS、Aurora、ElastiCache、DynamoDB|
|**虚拟化与容器**|ECS、ACK、ACR、ECI|EC2、EKS、ECR、Fargate|
|**Web 与 API**|SLB、ALB、CDN、API 网关、函数计算、SAE|ELB、ALB、CloudFront、API Gateway、Lambda、App Runner|
|**安全与权限**|RAM、KMS、WAF、云防火墙、DDoS 高防|IAM、KMS、WAF、Network Firewall、Shield|
|**运维与 DevOps**|SLS、云监控、ActionTrail、ROS、云效|CloudWatch、CloudTrail、CloudFormation、CodePipeline|
|**大数据**|DataWorks、MaxCompute、实时计算 Flink|Glue、Redshift、EMR、Managed Flink|
|**AI 与大模型**|PAI、百炼、GPU 云服务器、向量检索|SageMaker AI、Bedrock、EC2 GPU、Knowledge Bases|

## 最重要的记忆方式

不要孤立地背产品名，而是先记住计算机需要解决的几件事：

> **计算、存储、网络、数据库、安全、运维、大数据、AI。**

然后再往下面挂产品：

- 要一台电脑：**ECS / EC2**
    
- 要存文件：**OSS / S3**
    
- 要虚拟硬盘：**云盘 / EBS**
    
- 要数据库：**RDS / RDS**
    
- 要私有网络：**VPC / VPC**
    
- 要分发流量：**SLB / ELB**
    
- 要运行 Kubernetes：**ACK / EKS**
    
- 要不管服务器直接跑代码：**函数计算 / Lambda**
    
- 要管理权限：**RAM / IAM**
    
- 要训练机器学习模型：**PAI / SageMaker**
    
- 要调用大模型：**百炼 / Bedrock**
    

因此，你看到任何云产品时，都可以先问：

> **它是在帮我管理计算机的 CPU、存储、网络、数据、程序、安全，还是 AI？**

一旦确定了它属于哪个领域，产品名称就不再是需要死记的缩写，而只是不同云厂商给同一种计算机能力起的商品名。