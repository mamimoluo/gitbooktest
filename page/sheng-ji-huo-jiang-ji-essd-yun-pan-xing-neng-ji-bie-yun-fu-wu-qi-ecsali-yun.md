# 升级或降级ESSD云盘性能级别-云服务器 ECS-阿里云

ESSD（Enterprise SSD）云盘提供了 PL0/PL1/PL2/PL3 四种性能级别以满足不同应用场景的需求。您可以根据业务负载的变化调整 ESSD 云盘的性能级别，以平衡性能和成本。本文介绍如何修改 ESSD 云盘的性能级别。

关于 ESSD 云盘四种性能级别的详细性能说明，请参见： [块存储性能](/broken/pages/198bdbfcbba40f7b7b9a1fa00d3a5684a01c3149)。

## 使用限制

修改 ESSD 云盘性能级别时，使用限制如下：

* 状态限制
  * ESSD 云盘必须是 使用中 或 待挂载 状态。
  * 若 ESSD 云盘已挂载到 ECS 实例上，实例必须处于 运行中 或 已停止 状态，且 ECS 实例不能处于 过期 状态或账号欠费状态。
* 性能级别限制
  * 包年包月 ESSD 云盘仅支持升级性能级别。
  * 按量付费 ESSD 云盘支持升级和降低性能级别，但不支持降配到 PL0。

{% hint style="info" %}
只有以下云盘支持配置到 ESSD PL0：

* 高效云盘支持变配到 ESSD PL0。具体操作，请参见 [变更云盘类型](/broken/pages/01cca38070417833fa92d070d3c61c56c5471974)。
* 不同专属块存储集群中 ESSD PL1 云盘支持迁移到 ESSD PL0 云盘。具体操作，请参见 [ModifyDiskDeployment](/broken/pages/77833a8f351cf10ab8f7131ff26f18f5b415dc5e)。

此外，ESSD 云盘可选择的性能级别与云盘容量有关。如果您的 ESSD 云盘无法选择更高性能级别，可以先 [扩容云盘](/broken/pages/8a08e5daa2f54739f4502f2bab2bc5b682bab42e#concept-e1g-44g-ydb)，然后再升级 ESSD 云盘性能级别。
{% endhint %}

## 计费说明

升级 ESSD 云盘性能级别后，系统按照新性能级别单价计算消费账单，建议您根据实际业务需求，合理修改性能级别。更多信息，请参见： [块存储计费](/broken/pages/1f0798a3a257f5c2ab2c04ed33b8815087bc57cf)。

ESSD 云盘不同性能级别的价格不同，具体请参见：[块存储价格](https://www.aliyun.com/price/product#/disk/detail)。

## 操作步骤

{% stepper %}
{% step %}
### 步骤

访问 ECS 控制台 - 云盘： https://ecs.console.aliyun.com/disk/
{% endstep %}

{% step %}
### 步骤

在页面左侧顶部，选择目标资源所在的资源组和地域。\
![地域](../.gitbook/assets/p680076.png)
{% endstep %}

{% step %}
### 步骤

找到目标 ESSD 云盘，在 “操作” 列选择 ![更多](../.gitbook/assets/p479245.png) > 修改性能级别。
{% endstep %}

{% step %}
### 步骤

在 “修改性能级别” 对话框中，选择新的 性能级别，然后单击 确定。
{% endstep %}
{% endstepper %}

## 相关文档

* 通过 API 接口修改 ESSD 云盘性能级别： [ModifyDiskSpec](/broken/pages/d2872c61d27edd34ec5979f43b9111670e7f69dc#doc-api-Ecs-ModifyDiskSpec)。
* 通过云盘变配功能修改 ESSD 云盘性能级别： [变更云盘类型](/broken/pages/01cca38070417833fa92d070d3c61c56c5471974#task-2473687)。
