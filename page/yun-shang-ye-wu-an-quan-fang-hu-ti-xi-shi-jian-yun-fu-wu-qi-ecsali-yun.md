# 云上业务安全防护体系实践-云服务器ECS-阿里云

[官方文档](https://help.aliyun.com/)

云服务器 ECS 主要入口

* [云服务器 ECS](https://help.aliyun.com/zh/ecs/)
* [用户指南](tan-xing-kuo-zhan-de-yun-ji-suan-yun-fu-wu-qi-ecsali-yun.md)
* [开发参考](/broken/pages/f66c43dad1f5a640878ac28e3a4351b9561f7057)
* [产品计费](ji-fei-gui-ze-ding-jia-yu-cheng-ben-guan-li-yun-fu-wu-qi-ecsali-yun.md)
* [常见问题](/broken/pages/7863517210e068cf2abc739ae93c4c6c2ab43dd9)
* [动态与公告](/broken/pages/6a784655e941c50d89e9c509e06f9f3560c21c0a)

按产品/能力分类的帮助中心（节选）

* 计算
  * [云服务器 ECS](https://help.aliyun.com/zh/ecs/)
  * [GPU云服务器](https://help.aliyun.com/zh/egs/)
  * [轻量应用服务器](https://help.aliyun.com/zh/simple-application-server/)
  * [弹性容器实例](https://help.aliyun.com/zh/eci/)
  * [专有宿主机](https://help.aliyun.com/zh/dedicated-host/)
  * [弹性伸缩](https://help.aliyun.com/zh/auto-scaling/)
  * ...
* 高性能计算
  * [弹性高性能计算](https://help.aliyun.com/zh/e-hpc/)
  * [批量计算（文档停止维护）](https://help.aliyun.com/zh/batchcompute/)
* Serverless
  * [函数计算](https://help.aliyun.com/zh/functioncompute/)
  * [Serverless 应用引擎](https://help.aliyun.com/zh/sae/)
* 操作系统
  * [Alibaba Cloud Linux](https://help.aliyun.com/zh/alinux/)
* 容器
  * [容器服务 Kubernetes 版 ACK](https://help.aliyun.com/zh/ack/)
  * [容器镜像服务](https://help.aliyun.com/zh/acr/)
* 存储
  * [对象存储](https://help.aliyun.com/zh/oss/)
  * [文件存储NAS](https://help.aliyun.com/zh/nas/)
  * [表格存储](https://help.aliyun.com/zh/tablestore/)
  * ...
* 网络与 CDN
  * [负载均衡](https://help.aliyun.com/zh/slb/)
  * [弹性公网IP](https://help.aliyun.com/zh/eip/)
  * [专有网络VPC](https://help.aliyun.com/zh/vpc/)
  * [云企业网](https://help.aliyun.com/zh/cen/)
  * [CDN](https://help.aliyun.com/zh/cdn/)
* 安全
  * [DDoS防护](https://help.aliyun.com/zh/anti-ddos/)
  * [Web应用防火墙](https://help.aliyun.com/zh/waf/)
  * [云防火墙](https://help.aliyun.com/zh/cloud-firewall/)
  * [云安全中心](https://help.aliyun.com/zh/security-center/)
* 中间件、数据库、AI、大数据、媒体、企业服务、物联网、开发工具、迁移与运维等（详见原始帮助中心页面）

从这里开始（入门与框架）

* [新手指南](https://help.aliyun.com/product/47310.html)
* [云采用框架](https://help.aliyun.com/zh/caf/)
* [卓越架构](https://help.aliyun.com/product/2362200.html)
* [阿里云安全指南](https://help.aliyun.com/zh/acsg/)

（更多产品与服务请参见上游链接，以上为摘录以便快速访问）

在云环境中构建和维护安全、合规的业务系统，需重点关注五个核心领域的安全防护。

方案概览

* 基础安全方案：建议所有业务场景采纳，是保障云上资产安全的基本要求。
* 增强安全方案：建议对安全性敏感或有合规要求的企业级业务采纳，具备全面的安全防护能力。

安全建议一览（按安全类型与子类型）

| 安全类型      | 安全子类型           | 最佳实践（链接）                                                                           | 基础安全方案 | 增强安全方案 |
| --------- | --------------- | ---------------------------------------------------------------------------------- | ------ | ------ |
| 操作系统安全    | 凭据安全            | [避免使用弱口令登录实例](/broken/pages/d1eca0f35cda796d79aaa88cff2bba1cb944bbdc)              | 推荐     | 推荐     |
| 基于网络的访问控制 | 实例运维端口限制        | [实例运维端口应限制IP访问来源](/broken/pages/095d7d95b461d000410cb462bc238e9372653da0)          | 推荐     | 推荐     |
| 漏洞管理      | 主机关联防护          | [启用主机安全防护](/broken/pages/309263ef839af9b4d90fe86ec4b150ff8ae3cb2b)                 | 推荐     | 推荐     |
| 漏洞管理      | 漏洞修复            | [修复高危安全漏洞](/broken/pages/9f489077d3255517a36f7703e400c59b6bc48acd)                 | 推荐     | 推荐     |
| 特权访问管理    | 避免 root 登录      | [避免使用root账号登录实例](/broken/pages/d082e7b8462c175b073e8e684174e504e4714257)           |        | 推荐     |
| 凭据安全      | 镜像凭据            | [避免使用自定义镜像中的预设登录凭证](/broken/pages/b40563fc646164d2978ce8c7d92160576570490a)        |        | 推荐     |
| 供应链安全     | 镜像使用限制          | [限制使用指定范围镜像创建实例](/broken/pages/635aceae4b321997fa518457410e85f2554b1cf9)           |        | 推荐     |
| 监控报警      | 异常登录检测          | [启用异常登录检查](/broken/pages/9e02d52dba2dbdd1cb1345b65d3d9ec3739e35b8)                 |        | 推荐     |
| 数据安全      | 数据保护            | [开启云盘自动快照策略](/broken/pages/9f528805ae99792e0e4235fe3f26566cc0a1d277)               | 推荐     | 推荐     |
| 敏感信息      | API 访问加固        | [仅允许HTTPS访问ECS OpenAPI](/broken/pages/aa016f06ec3327366704ed6f9fbe9cf305415157)    | 推荐     | 推荐     |
| 供应链安全     | 镜像/快照权限         | [镜像/快照权限管控与数据防泄露](/broken/pages/bd7d1f7c546564ce90e3f2affbef36abd966ef42)          |        | 推荐     |
| 加密        | 加密云盘            | [使用加密云盘](/broken/pages/645a9f3a6693346c9e0c0b6300c0f9667777ac90)                   |        | 推荐     |
| 加密        | 元数据访问模式         | [使用仅加固模式访问实例元数据](/broken/pages/4416592df0e22d9a0aa677943631e7531fd103b6)           |        | 推荐     |
| 身份与访问控制   | 账号保护            | [保护云账号，防止凭据泄露](/broken/pages/260634288898993b24d479fbf8fa4d278a289142)             | 推荐     | 推荐     |
| 特权访问管理    | 主账号与子账号         | [避免使用主账号，为不同职责子账号成员授予不同权限](/broken/pages/572acf54ab032a04d109ed3ae46d1e4fd3a4a202) | 推荐     | 推荐     |
| 基于网络的访问控制 | OpenAPI 源 IP 约束 | [约束OpenAPI调用来源IP合法范围](/broken/pages/2238eb9f7da1ae69a0ccbcb788849e8f0abd1338)      |        | 推荐     |
| 资源级别权限控制  | 资源组             | [使用资源组实现横向分权管理](/broken/pages/c2c6a3f17fd5edc4412be6b70182556a3c757a61)            |        | 推荐     |
| 资源级别权限控制  | 标签权限            | [启用标签细粒度资源权限管理](/broken/pages/c6d0715f72cf9c49968b34b9e1a80c82a35ff990)            |        | 推荐     |
| 合规约束配置    | RAM Policy      | [使用RAM Policy约束云上操作](/broken/pages/49d8114c1eb907cae9e30140a26e1e9df2a025cc)       |        | 推荐     |
| 网络安全      | 公网暴露收敛          | [收敛ECS实例的公网暴露风险](/broken/pages/8cfc9d5c100a4c374d74ccec0e8b496f3c95e1bd)           | 推荐     | 推荐     |
| 网络安全      | VPC 隔离          | [使用VPC实现网络隔离](/broken/pages/ac873d1f17737f2414ad38bc86a7d855f331b70f)              |        | 推荐     |
| 网络安全      | 安全组             | [使用安全组实现内网访问控制](/broken/pages/dceb397c8d994a5902c5247e2138d0dbd5d92060)            |        | 推荐     |
| 网络安全      | 网络ACL           | [使用网络ACL加强网络访问控制](/broken/pages/e642dfe89fe3709b6fd36d5ffd6c085d8596fd1b)          |        | 推荐     |
| 网络安全      | 私网/专线访问         | [通过内网或专线访问云上服务](/broken/pages/ff64f257154bb753287de72952197016884fe43b)            |        | 推荐     |
| 网络安全      | PrivateLink     | [使用PrivateLink减少非必要的公网通信](/broken/pages/43de5e8f3cf96138a672b157fdd4ecc7b1ed8bd1)  |        | 推荐     |
| 流量安全      | DDoS 防护         | [使用DDoS清洗服务抵御公网攻击](/broken/pages/939b0432e955e573c6b20b2ac1301a244986547c)         |        | 推荐     |
| 流量安全      | 云防火墙            | [使用云防火墙抵御公网攻击](/broken/pages/700865d48d129db3fc2e5a7c0709b6ccf73cc086)             |        | 推荐     |
| Web 流量安全  | WAF             | [使用Web应用防火墙抵御Web攻击](/broken/pages/78cbc31f537c0986289fbc61af2f2166db35abf1)        |        | 推荐     |
| 安全审计与运维   | 操作审计            | [使用操作审计记录和分析云操作](/broken/pages/9da62c7acadd56db74e68528c7ca943a306a5afb)           | 推荐     | 推荐     |
| 安全审计与运维   | 安全联系人           | [维护正确的安全联系人，及时接收与处理安全事件](/broken/pages/b3ea731565aab0013538e06056a07aae3cdab34a)   | 推荐     | 推荐     |
| 特权访问管理    | 堡垒机             | [使用堡垒机运维满足等保2.0要求](/broken/pages/ac732271956f77c335e87ddb9e4a1411ede77776)         |        | 推荐     |
| 日志        | VPC 流日志         | [启用VPC流日志，分析和审计网络流量](/broken/pages/324d02aecc19ca665aa792bca1d154ee172db3f0)       |        | 推荐     |

相关章节

* 上一篇：禁止RAM用户为ECS实例分配公网IP\
  https://help.aliyun.com/zh/ecs/user-guide/ram-ecs-ip
* 下一篇：操作系统安全\
  https://help.aliyun.com/zh/ecs/user-guide/operating-system-security/

可交互元素

<details>

<summary>该文章对您有帮助吗？ 反馈</summary>

如果需要反馈或评价此页面，请使用原文页面中的反馈功能（保留原始链接与上下文）。

</details>

说明与注意

* 本页保留了原始帮助中心中的所有链接及查询参数，未做任何修改或替换。
* 已移除页面导航、登录/注册提示、AI 助理小窗等非正文内容以便适配文档平台。
