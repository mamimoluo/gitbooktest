# 弹性扩展的云计算-云服务器 ECS-阿里云

云服务器ECS（Elastic Compute Service）是阿里云提供的性能卓越、稳定可靠、弹性扩展的 IaaS（Infrastructure as a Service）级别云计算服务。云服务器ECS免去了您采购IT硬件的前期准备，让您像使用水、电、天然气等公共资源一样便捷、高效地使用服务器，实现计算资源的即开即用和弹性伸缩。阿里云ECS持续提供创新型服务器，解决多种业务需求，助力您的业务发展。

## 什么是云服务器ECS

### 前置概念

阅读本文前，您可能需要了解如下概念：

* [什么是云计算？](https://www.aliyun.com/getting-started/what-is/what-is-cloud-computing)
* [什么是虚拟化？](https://www.aliyun.com/getting-started/what-is/what-is-virtualization)
* [什么是IaaS？](https://www.aliyun.com/getting-started/what-is/what-is-iaas)

### 为什么选择云服务器ECS

* 多样化计算能力：阿里云服务器支持主流的 x86、Arm 处理器架构，覆盖 CPU、GPU、弹性裸金属及超级计算集群等服务器类型，提供了上百种实例规格族，满足不同规模和类型用户的需求。
* 便捷易用：无需自建机房，分钟级交付，并提供行业通用标准 API、性能监控框架和主动运维体系，支持 Terraform、系统运维、资源编排等多种运维能力。
* 成本优化：提供按量付费、包年包月、抢占式实例等多种计费方式，并可结合节省计划、预留实例券等优惠策略，以及弹性伸缩、弹性供应能力，优化资源使用成本。
* 弹性灵活：支持根据业务量增减灵活调整计算、存储和网络带宽配置，并可结合弹性伸缩实现自动扩缩容。
* 稳定可靠：单实例可用性达 99.975%，多可用区多实例可用性达 99.995%；云盘多副本、快照备份、自动告警等保障数据安全与服务稳定。
* 安全保障：提供硬件加密、虚拟防火墙、访问控制、DDoS 防护、漏洞扫描、数据加密等多重安全能力，保护操作系统、访问、网络与应用层的安全。

更多选择理由，请参见 [特性与优势](yun-fu-wu-qi-ecs-de-te-xing-yu-you-shi-ali-yun-bang-zhu-zhong-xin.md) 和 [应用场景](/broken/pages/193cc3d35459889c92e8e3a96ae7201547aadedf#topic-9547)。

### 产品架构

云服务器ECS主要包含实例、镜像、[块存储](https://www.aliyun.com/getting-started/what-is/what-is-block-storage)、快照、[安全组](https://www.aliyun.com/getting-started/what-is/what-is-a-security-group)、网络等功能组件。更多基本概念请参见 [基本概念](/broken/pages/e234f923a147e3095a964280e9113b19c5198a87#concept-2087383)。

![ECS 架构图](../.gitbook/assets/CAEQORiBgIC33tze5xgiIDczMTkzZjMyYTM5YzQ2MjY4YjkxZGYyYTViZjZmMjcx3974248_20230905103052.317.svg)

### 产品计费

ECS 计费涉及计算资源（vCPU 和内存）、镜像、块存储、公网带宽、快照等。常见计费方式包括：

* 包年包月：按一定时长预付并使用资源。
* 按量付费：按需开通和释放，先使用后付费。
* 抢占式实例：竞价模式的折扣实例，但可能被回收。
* 预留实例券：对指定配置的按量付费实例进行账单抵扣。
* 节省计划：按承诺的稳定使用量按折扣价抵扣账单。
* 存储容量单位包：以容量包形式抵扣块存储、NAS、OSS 等存储类账单。
* OSS 存储包：OSS 标准（LRS）存储包可自动按容量抵扣快照费用。

详细计费规则请参见 [计费概述](ji-fei-gui-ze-ding-jia-yu-cheng-ben-guan-li-yun-fu-wu-qi-ecsali-yun.md#concept-isb-scd-5db)，价格信息见 [云产品定价页](https://www.aliyun.com/price/product#/ecs/detail)。产品详情与活动请见 [云服务器ECS产品详情页](https://www.aliyun.com/product/ecs)。

### 如何使用云服务器ECS

通过注册阿里云账号，您可以通过以下途径创建、使用或释放 ECS 实例：

* ECS 管理控制台：交互式 Web 操作界面。参见 [常用操作导航](/broken/pages/5d34a091237abdd142d2f2da96c023bc3ce9817d)。
* ECS [API](https://www.aliyun.com/getting-started/what-is/what-is-api)：RPC 风格 API 支持 GET/POST。API 参考见 [API参考](/broken/pages/544e95094e0ff1eb619562d261227ff8eb99ab9a#EcsApiWelcome)。常用开发者工具：
  * [命令行工具CLI](/broken/pages/b82f2afb74629527cbd6a7758088890bc543e290)
  * [OpenAPI 开发者门户](https://next.api.aliyun.com/api/Ecs/2014-05-26)
  * [阿里云SDK（多语言）](https://next.api.aliyun.com/api-tools/sdk/Ecs?version=2014-05-26)
* [资源编排服务ROS](https://help.aliyun.com/zh/ros/product-overview/what-is-ros#concept-28852-zh)：通过模板自动创建和配置资源。
* [系统运维管理OOS](/broken/pages/3a785f8fd1d0919fcc1658a48e9dd1d89548002a#concept-1181174)：定义执行模板以自动化运维任务。
* [Terraform 参考](/broken/pages/c783b8e1ad39aff03d0d4061bd34502698bad9f2)。
* [阿里云客户端](/broken/pages/decd92994307f70ac9813fd7f6315f6e83e5d02b#concept-2230009)：官方客户端工具，支持资源浏览、远程连接等。
* 阿里云 App：移动端管理工具。
* [Alibaba Cloud Toolkit](https://help.aliyun.com/document_detail/29968.html)：IDE 插件，帮助在云端高效开发与部署。

### 部署建议

您可以从以下维度考虑如何启动并使用云服务器ECS：

* 地域和可用区\
  地域表示阿里云数据中心的地理区域，创建实例后地域确定且不可更换（仅专有网络 VPC 类型 ECS 实例支持获取元数据）。请选择合适地域/可用区以满足用户地理分布、内网通信需求或产品支持情况。若需与 RDS 等云产品内网通信，相关资源需位于同一地域。更多信息见 [地域和可用区](https://help.aliyun.com/document_detail/40654.html#concept-2459516)。
* 高可用性\
  建议使用快照实现数据备份，并通过跨可用区部署、部署集、[负载均衡（Server Load Balancer）](https://www.aliyun.com/getting-started/what-is/what-is-load-balance)等方式实现应用容灾。
* 网络规划\
  推荐使用专有网络 VPC，自主规划私网 IP，全面支持新功能与新型实例规格，便于业务隔离与多地域部署。更多信息见 [专有网络（Virtual Private Cloud）](https://help.aliyun.com/zh/vpc/what-is-vpc#concept-kbk-cpz-ndb)。
* 安全方案
  * 使用安全组控制实例出入网访问策略与端口监听。详见 [安全组概述](/broken/pages/e4133156c568d53e520d7d3660ecaa5bd31356d4#concept-o2y-mqw-ydb)。
  * 阿里云为 ECS 提供免费的 DDoS 基础防护与基础安全服务（包含异常登录检测、漏洞扫描、基线配置核查等）。DDoS 基础防护默认开启（不超过 5 Gbps），如需更高防护，可购买 DDoS 高防，详见 [DDoS 高防文档](https://help.aliyun.com/zh/anti-ddos/anti-ddos-pro-and-premium/product-overview/what-are-anti-ddos-pro-and-anti-ddos-premium#concept-2417452)。若需更完整的安全能力，可购买更高级别的云安全中心，详见 [云安全中心文档](https://help.aliyun.com/zh/security-center/product-overview/what-is-security-center#concept-bjv-y5w-ydb)。

### 参考与延伸阅读

* [云服务器 ECS（产品页）](https://help.aliyun.com/zh/ecs/)
* [特性与优势](yun-fu-wu-qi-ecs-de-te-xing-yu-you-shi-ali-yun-bang-zhu-zhong-xin.md)
* [应用场景](/broken/pages/193cc3d35459889c92e8e3a96ae7201547aadedf#topic-9547)
* [计费概述](ji-fei-gui-ze-ding-jia-yu-cheng-ben-guan-li-yun-fu-wu-qi-ecsali-yun.md#concept-isb-scd-5db)
* [云产品定价页](https://www.aliyun.com/price/product#/ecs/detail)
* [云服务器ECS产品详情页（活动信息）](https://www.aliyun.com/product/ecs)

### 上一篇 / 下一篇

* 上一篇：云服务器 ECS — https://help.aliyun.com/zh/ecs/
* 下一篇：图说云服务器ECS — https://help.aliyun.com/zh/ecs/user-guide/tew-ecs-cloud-server

（页面中涉及的所有链接与图片均保留原始 URL）
