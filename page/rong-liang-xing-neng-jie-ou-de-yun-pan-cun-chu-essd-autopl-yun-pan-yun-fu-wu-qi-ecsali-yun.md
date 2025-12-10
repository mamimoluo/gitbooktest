# 容量性能解耦的云盘存储-ESSD AutoPL云盘-云服务器 ECS-阿里云

阿里云ESSD AutoPL云盘基于ESSD云盘，在保持ESSD云盘原有功能与性能的同时，实现了云盘容量与云盘性能的解耦。您可以在配置ESSD AutoPL云盘容量的同时，根据业务需求自定义云盘的预配置性能和性能突发。本文主要介绍ESSD AutoPL云盘的特点、规格、计费、应用场景以及配置操作等信息。

说明

* ESSD AutoPL云盘所承载的压力均来自于它所挂载的实例，实例中所有应用程序使用AutoPL云盘的性能，均会被计入性能统计中，包含但不限于云盾、云监控、云助手等运行在实例操作系统中的应用程序。
* 云盘的常用性能指标解释，请参见 性能指标：https://help.aliyun.com/zh/ecs/user-guide/block-storage-performance#section-yzi-wb6-9gc
* 有关ESSD云盘的相关介绍，请参见 ESSD云盘：https://help.aliyun.com/zh/ecs/user-guide/essds

## 云盘特点

ESSD AutoPL云盘的性能是基准性能、预配置性能以及性能突发之和：

* 基准性能\
  ESSD AutoPL云盘基准性能与价格，均与ESSD云盘的PL1级别保持一致。
* 支持配置云盘预配置性能\
  ESSD AutoPL云盘允许在存储容量不变的情况下，根据业务需求灵活配置预配置性能，实现容量与性能解耦。
* 支持云盘性能突发\
  ESSD AutoPL云盘默认开启性能突发，波动性业务在面临突发的数据读写压力时，ESSD AutoPL云盘会根据业务实际情况临时提升云盘性能，直至业务恢复至平稳状态。最大可以支持100万IOPS、4 GB/s吞吐量和1,000 IOPS/GiB的性能密度。

## 应用场景

ESSD AutoPL云盘可应用于ESSD云盘适用的场景，例如时延敏感的应用或者IO密集型业务场景。同时，基于灵活的性能配置以及性能突发的能力，其还适用于以下情况：

* 固定容量但需要更高性能的业务。
* 业务波动较大，波峰高频出现，需要云盘具备应对突发业务的能力。

## ESSD AutoPL云盘规格

ESSD AutoPL云盘规格如下表所示：

| **性能类别**        | **云盘属性**                                     | **规格说明**                                                                                             |
| --------------- | -------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| 基准性能            | 云盘容量范围（GiB）                                  | 1\~65,536                                                                                            |
|                 | 单盘最大IOPS（Input/Output Operations Per Second） | 50,000                                                                                               |
|                 | 单个IO大小的最大值（KB）                               | 16                                                                                                   |
|                 | 单盘最大吞吐量（MB/s）                                | 350                                                                                                  |
|                 | 单路随机写平均时延（ms），Block Size=4k                  | 0.2                                                                                                  |
|                 | 单盘IOPS性能计算公式（基准性能①）                          | max{min{1,800+50\*容量，50,000}, 3,000}                                                                 |
|                 | 单盘吞吐量性能计算公式（基准性能①，MB/s）                      | max{min{120+0.5\*容量，350}，125}                                                                        |
| 预配置性能           | 单盘预配置性能②的最大IOPS                              | 50,000                                                                                               |
|                 | 单盘IOPS性能计算公式（预配置性能②）                         | <p>容量（GiB）&#x3C;=3：不支持设置预配置性能。<br>容量（GiB）>=4：[1，min{(1,000 IOPS/GiB*容量-基准IOPS)，50,000}]</p>          |
|                 | 单盘吞吐量性能计算公式（预配置性能②，MB/s）                     | 16 KB\*预配置IOPS/1,024                                                                                 |
| 性能突发            | 单盘突发IOPS计算公式（性能突发③）                          | 实际最终IOPS-基准性能IOPS-预配置性能IOPS                                                                          |
|                 | 单盘突发吞吐量计算公式（性能突发③）                           | 实际最终吞吐量-基准性能吞吐量-预配置性能吞吐量                                                                             |
| ESSD AutoPL云盘性能 | 性能组成                                         | 基准性能①+预配置性能②+性能突发③                                                                                   |
|                 | 云盘最大IO密度④                                    | 1,000 IOPS/GiB                                                                                       |
|                 | 单盘IOPS性能上限（开启性能突发）                           | <p>容量 &#x3C;= 3GiB：不支持开启性能突发。<br>容量 >= 4GiB：min{100万，1,000*容量}</p>                                   |
|                 | 单盘吞吐量性能上限（开启性能突发）                            | <p>容量 &#x3C;= 3GiB：不支持开启性能突发。<br>容量 >= 4GiB：max{min{16 KB * IOPS/1,024，4GB/s}, (基准吞吐量 + 预配置吞吐量)}</p> |
|                 | 单盘IOPS性能上限（不开启性能突发，即基准性能+预配置性能的上限）           | 10万                                                                                                  |
|                 | 单盘吞吐量性能上限（不开启性能突发，即基准性能+预配置性能的上限）            | 1,131 MB/s                                                                                           |

注释

* ①：仅指云盘本身可以提供的基准性能，不包含云盘的预配置性能和性能突发。
* ②：除基准性能之外，云盘可以提供的预配置性能。
* ③：除基准性能和预配置性能之外，云盘可以提供的性能突发。您可以通过EBS的数据洞察（CloudLens for EBS）实时监控ESSD AutoPL云盘的突发详情，包括Burst时间、Burst数量（即突发IO总量）等。更多信息，请参见 云盘分析：https://help.aliyun.com/zh/ecs/user-guide/cloud-disk-analysis
* ④：IO密度=IOPS/云盘容量，单位是IOPS/GiB，表示每GiB可达到的IOPS能力。

说明：云盘的读写性能同时受到实例规格和云盘本身规格的限制，任意一项达到上限都会受到限制。例如，当实例规格限制云盘带宽为6 Gbit/s（换算为750 MB/s）时，实际云盘吞吐量将不会超过750 MB/s。

* 实例规格限制的云盘IOPS及带宽，请参见 实例规格族：https://help.aliyun.com/zh/ecs/user-guide/overview-of-instance-families#concept-sx4-lxv-tdb 。如果因为实例规格限制而无法达到云盘性能上限，建议您升级实例规格。
* 实例规格与存储I/O性能的关系，请参见 实例规格与存储I/O性能的关系：https://help.aliyun.com/zh/ecs/user-guide/storage-i-or-o-performance#section-v1m-8kg-ckz

## 支持的实例规格族

ESSD AutoPL云盘支持的实例规格族与ESSD云盘支持的实例规格族保持一致。实例规格族信息，请参见 实例规格族：https://help.aliyun.com/zh/ecs/user-guide/overview-of-instance-families#concept-sx4-lxv-tdb

## 计费说明

### 计费项及其计费规则

ESSD AutoPL云盘费用包括： 云盘容量费用 + 预配置性能费用 + 性能突发费用。具体计费规则请参见 块存储计费：https://help.aliyun.com/zh/ecs/block-storage-devices#concept-1937442

* 云盘容量费用：创建即收费，购买后具备和ESSD云盘PL1相同的基准性能。
* 预配置性能费用：开启后按量收费。
* 性能突发费用：开启后按量收费。
  * 每小时突发IO总量小于等于10万： 享受10万IO免费额度，不收取性能突发费用。
  * 每小时突发IO总量大于10万： 采用费用封顶规则，确保在享受高性能的同时，无需担心费用超支的风险。详情请参见 性能突发费用封顶规则：https://help.aliyun.com/zh/ecs/user-guide/essd-autopl-disks#c69ee53799gmi

性能突发费用封顶规则

ESSD AutoPL云盘的性能突发费用封顶旨在应对业务极端状况，过高的突发IOPS可能导致费用累计超出预期。为解决这一问题，ESSD AutoPL云盘采用性能突发费用封顶规则，确保即使面对极端性能突发状况，性能突发费用支出也能保持在可控范围内，无需担心费用超支风险。

* 封顶判定依据：以计量周期内的最大突发IO密度（最大突发IOPS/云盘容量）是否大于200 IOPS/GiB为界限来判定。突发IOPS不包括基准性能和预配置性能。

封顶规则表

| 突发IO总量（IO/小时） | 最大突发IO密度（最大突发IOPS/GiB） | 封顶前费用                        | 封顶后费用                                         | 典型场景                |
| ------------- | ---------------------: | ---------------------------- | --------------------------------------------- | ------------------- |
| 小于等于10万       |                    不涉及 | 费用=突发总量 \* 性能突发单价            | 免费（10万IO免费额度）                                 | 偶发毛刺IO              |
| 大于10万         |                小于等于200 | 不封顶：费用=(突发IO-10万) \* 性能突发单价  | —                                             | 偶发业务高峰，但未超过IOPS封顶阈值 |
| 大于10万         |              大于200（封顶） | 费用=(突发IO-10万) \* 性能突发单价（计算前） | 费用封顶：min((突发IO-10万)_单价, 云盘容量费&#x7528;_&#x38;) | 极端负载且持续时间长          |

示例（费用节省对比）

例如杭州地域，一块40 GiB的ESSD AutoPL云盘在使用期间发生了性能突发，该小时突发总量为439万IO，突发IO总量大于10万，该小时内最大突发IO密度达到230 IOPS/GiB（大于200 IOPS/GiB），满足费用封顶条件，则性能突发费用说明如下：

* 封顶前费用（示例计算）：439万 \* 0.02 = 8.78 元
* 封顶后费用（示例计算）：min((439-10)万 \* 0.02, 云盘容量费用\*8) = 0.672 元

您可以在账单中心查看ESSD AutoPL云盘的性能突发费用以及账单详情。具体操作，请参见 账单查询：https://help.aliyun.com/zh/ecs/view-billing-details

（示例图片） https://help-static-aliyun-doc.aliyuncs.com/assets/img/zh-CN/5062924271/p808028.png

定价详情

ESSD AutoPL云盘的定价详情，请参见 块存储定价：https://www.aliyun.com/price/product#/disk/detail

欠费影响

如果您的阿里云账号因ESSD AutoPL云盘的预配置性能和性能突发的按量付费账单发生欠费，阿里云会立即停止预配置性能和性能突发的按量服务，但基准性能不受影响。结清账单后，预配置性能和性能突发服务会立即恢复。

## 配置云盘预配置性能和性能突发

您可以在ECS管理控制台上创建ESSD AutoPL云盘，或者在创建ECS实例选择ESSD AutoPL云盘时，按需配置。

说明

* 创建ECS实例和创建云盘的具体操作，请参见 自定义购买实例：https://help.aliyun.com/zh/ecs/user-guide/create-an-instance-by-using-the-wizard#task-vwq-5g4-r2b 和 创建空数据盘：https://help.aliyun.com/zh/ecs/user-guide/create-a-disk/#concept-jx1-tx1-ydb。
* 您可以在创建完成后，根据业务需求灵活调整云盘的预配置性能和性能突发，从而节约您的费用成本。具体操作，请参见 修改ESSD AutoPL云盘性能配置：https://help.aliyun.com/zh/ecs/user-guide/modify-the-performance-configurations-of-an-essd-autopl-disk#task-2173169

创建 ESSD AutoPL 云盘（ECS 管理控制台 示例）

{% stepper %}
{% step %}
### 步骤

访问 ECS 控制台 - 云盘： https://ecs.console.aliyun.com/disk/
{% endstep %}

{% step %}
### 步骤

在页面左侧顶部，选择目标资源所在的资源组和地域。\
（示例图片）https://help-static-aliyun-doc.aliyuncs.com/assets/img/zh-CN/5587314271/p680076.png
{% endstep %}

{% step %}
### 步骤

单击 创建云盘。
{% endstep %}

{% step %}
### 步骤

在创建云盘页面中，设置云盘的配置参数：

* 选择云盘类型为 ESSD AutoPL，并配置云盘容量。
* 选中 预配置性能，并配置 IOPS 大小。
* 默认选中 开启性能突发，您可以根据业务需求关闭性能突发。

更多参数与说明请参见 创建空数据盘：https://help.aliyun.com/zh/ecs/user-guide/create-a-disk/#concept-jx1-tx1-ydb

（示例图片）https://help-static-aliyun-doc.aliyuncs.com/assets/img/zh-CN/1869791371/p872973.png
{% endstep %}
{% endstepper %}

## ESSD AutoPL云盘计费示例

以下是配置ESSD AutoPL云盘的预配置性能和性能突发后，云盘的性能指标以及产生的费用示例情况（示例地域：华东1（杭州）），该地域示例单价如下：

* 包年包月
  * 基准性能单价：1.00元/1 GiB/月
  * 预配置性能单价：0.0000625元/1 IOPS/小时
  * 性能突发单价：0.02元/万 IO
* 按量付费
  * 基准性能单价：0.0021元/1 GiB/小时
  * 预配置性能单价：0.0000625元/1 IOPS/小时
  * 性能突发单价：0.02元/万 IO

说明：本示例价格仅为示例，具体价格以 块存储定价：https://www.aliyun.com/price/product#/disk/detail 为准。

示例一：配置了预配置性能但未开启性能突发

* 假设容量 100 GiB，预配置性能 200 IOPS，未开启性能突发。

包年包月计费示例（购买时长 1 个月）：

* 基准性能费用：100 \* 1 \* 1 = 100 元
* 预配置性能费用：0.0000625 \* 200 \* (30\*24) = 9 元
* 总费用：100 + 9 = 109 元

按量付费示例（计费时长 24 小时）：

* 基准性能费用：100 \* 0.0021 \* 24 = 5.04 元
* 预配置性能费用：0.0000625 \* 200 \* 24 = 0.3 元
* 总费用：5.04 + 0.3 = 5.34 元

性能/吞吐量计算（示例）

* 基准IOPS：1,800 + 50\*100 = 6,800
* 云盘总IOPS：6,800 + 200 = 7,000
* 基准吞吐量：120 + 0.5\*100 = 170 MB/s
* 预配置吞吐量：16 KB \* 200 / 1,024 = 3.125 MB/s
* 云盘总吞吐量：约 173.125 MB/s（控制台取整显示）

示例二：配置了预配置性能，同时开启性能突发

* 假设容量 100 GiB，预配置性能 200 IOPS，开启性能突发。以下为不同突发场景的示例费用说明。

场景一：仅 IOPS 突发（突发量 8,000，持续 1s）

* 因每小时享受 10 万 IO 免费额度，该次突发对应的性能突发费用为 0。

包年包月总费用示例：

* 基准性能费用：100 元
* 预配置性能费用：9 元
* 性能突发费用：0（小于10万免费）
* 总费用：109 元

按量付费总费用示例（24 小时计费）：

* 基准性能费用：5.04 元
* 预配置性能费用：0.3 元
* 性能突发费用：0
* 总费用：5.34 元

场景二：仅吞吐量突发（1 GB/s，持续 15s）

* 将吞吐量折算为 IO：1 GB/s ≈ 65,536 IO/s，15s 内突发 IO = 65,536 \* 15 = 983,040，按上万取整为 990,000 IO（示例取值 99 万IO）。突发IO总量大于10万且IO密度>200 IOPS/GiB，满足费用封顶条件。

包年包月示例中性能突发费用：

* 性能突发费用：min((99-10)&#x4E07;_&#x30;.02, 云盘容量费&#x7528;_&#x38;) = 1.68 元
* 总费用：100 + 9 + 1.68 = 110.68 元

按量付费示例总费用：5.04 + 0.3 + 1.68 = 7.02 元

场景三：IOPS 与吞吐量均突发，以突发 IO 量更大者计费（2s，IOPS 突发 8,000，吞吐突发 1 GB/s）

* 2s 内 IOPS 突发量 = 8,000 \* 2 = 16,000
* 2s 内吞吐折算 IO = &#x31;_&#x31;,02&#x34;_&#x31;,024/16 \* 2 = 131,072 → 上万取值 140,000 IO
* 计费按 140,000 IO，满足封顶条件，包年包月示例性能突发费用为 0.08 元，总费用约 109.08 元；按量付费示例总费用约 5.42 元。

## 相关文档

* 变更云盘类型：https://help.aliyun.com/zh/ecs/user-guide/change-the-category-of-a-disk#task-2473687
* 修改ESSD AutoPL云盘性能配置：https://help.aliyun.com/zh/ecs/user-guide/modify-the-performance-configurations-of-an-essd-autopl-disk#task-2173169
* 测试ESSD云盘IOPS性能：https://help.aliyun.com/zh/ecs/user-guide/test-the-iops-performance-of-an-essd#task-2363356

参考链接（页面中引用）

* 性能指标：https://help.aliyun.com/zh/ecs/user-guide/block-storage-performance#section-yzi-wb6-9gc
* ESSD 云盘：https://help.aliyun.com/zh/ecs/user-guide/essds
* 云盘分析：https://help.aliyun.com/zh/ecs/user-guide/cloud-disk-analysis
* 块存储计费：https://help.aliyun.com/zh/ecs/block-storage-devices#concept-1937442
* 性能突发费用封顶规则：https://help.aliyun.com/zh/ecs/user-guide/essd-autopl-disks#c69ee53799gmi
* 块存储定价：https://www.aliyun.com/price/product#/disk/detail
* 账单查询：https://help.aliyun.com/zh/ecs/view-billing-details

示例图片

* https://help-static-aliyun-doc.aliyuncs.com/assets/img/zh-CN/5062924271/p808028.png
* https://help-static-aliyun-doc.aliyuncs.com/assets/img/zh-CN/5587314271/p680076.png
* https://help-static-aliyun-doc.aliyuncs.com/assets/img/zh-CN/1869791371/p872973.png

（页面中原有的导航、页内目录、反馈小组件等 UI 导航元素已移除，仅保留正文内容与原始链接。）
