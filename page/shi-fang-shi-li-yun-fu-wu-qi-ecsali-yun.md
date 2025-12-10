# 释放实例-云服务器 ECS-阿里云

对于不再使用或闲置的ECS实例，为避免产生不必要的费用，可通过控制台或API立即或定时释放实例。如需防止ECS实例被意外释放，可开启实例释放保护。

### 影响与风险

* 数据丢失：释放实例时，本地盘、系统盘、设置了随实例释放的数据盘会一起释放，此外，设置了随云盘释放的自动快照也会一起被删除。\
  规避方法：提前手动创建快照或自定义镜像备份数据。
  * 手动创建快照：https://help.aliyun.com/zh/ecs/user-guide/create-a-snapshot ""
  * 自定义镜像：https://help.aliyun.com/zh/ecs/user-guide/create-a-custom-image-from-a-snapshot-1 ""
* IP丢失：实例的固定公网IP将被回收，无法找回。\
  规避方法：将固定公网IP转为弹性公网IP：https://help.aliyun.com/zh/ecs/user-guide/convert-the-public-ip-address-of-an-instance-in-a-vpc-to-an-eip ""
* 可能持续计费：未设置随实例释放的云盘、弹性公网IP（EIP）、云盘快照等独立云资源不会被删除，并会继续计费。\
  规避方法：释放实例后，通过账单辅助排查并手动释放其余资源。https://help.aliyun.com/zh/ecs/view-billing-details ""

***

### 释放按量付费实例

控制台 / API

{% stepper %}
{% step %}
### 控制台操作（按量付费实例）

1. 访问 ECS 控制台 - 实例： https://ecs.console.aliyun.com/server/region ，选择目标实例所在地域与资源组。
2. 单击目标实例 “操作” 列下的图标 > 实例状态 > 释放。
3. 在释放实例对话框中，选择释放设置后单击下一步：
   * 立即释放：在确认后立即开始释放流程。
   * 定时释放：在设定的时间自动执行释放。取消方法：https://help.aliyun.com/zh/ecs/user-guide/release-an-instance#467edd1422urg ""
4. 仔细阅读界面提示，并确认释放与保留的关联资源，确认无误后单击 确认。实例会根据释放设置释放实例。
{% endstep %}

{% step %}
### API

* 释放一台ECS实例：DeleteInstance - 删除实例\
  https://help.aliyun.com/zh/ecs/developer-reference/api-ecs-2014-05-26-deleteinstance ""
* 释放一台或多台ECS实例：DeleteInstances - 批量删除实例\
  https://help.aliyun.com/zh/ecs/developer-reference/api-ecs-2014-05-26-deleteinstances ""
{% endstep %}
{% endstepper %}

***

### 释放包年包月实例

重要：只能释放 已过期 或 已退款停机 的包年包月实例，未过期的包年包月实例释放前需先退订。退订说明：https://help.aliyun.com/zh/ecs/refund-instructions#f5c9488c1231k ""

控制台 / API

{% stepper %}
{% step %}
### 控制台操作（包年包月实例）

1. 访问 ECS 控制台 - 实例： https://ecs.console.aliyun.com/server/region ，选择目标实例所在地域与资源组。
2. 单击目标实例 “操作” 列下的图标 > 实例状态 > 释放。
3. 在释放实例对话框中，选择释放设置为 立即释放，单击 下一步。
4. 仔细阅读界面提示，并确认释放与保留的关联资源，确认无误后单击 确认释放。
{% endstep %}

{% step %}
### API

* 释放一台ECS实例：DeleteInstance - 删除实例\
  https://help.aliyun.com/zh/ecs/developer-reference/api-ecs-2014-05-26-deleteinstance ""
* 释放一台或多台ECS实例：DeleteInstances - 批量删除实例\
  https://help.aliyun.com/zh/ecs/developer-reference/api-ecs-2014-05-26-deleteinstances ""

提示：释放 已过期 或 已退款停机 的包年包月实例时，需指定 TerminateSubscription=true。
{% endstep %}
{% endstepper %}

***

### 实例释放保护

启用后，系统将拒绝所有通过控制台、API或CLI发起的手动释放请求，以防止实例因人为误操作或恶意行为被意外释放。此功能仅支持按量付费实例，且仅针对手动释放操作，在以下由系统自动触发的释放场景中不会生效：

* 账号欠费导致实例被回收。
* 预设的定时释放任务到期。
* 实例因违反云平台安全规则被强制释放。https://help.aliyun.com/document\_detail/467859.html ""
* 实例所在的弹性伸缩组触发自动缩容。

#### 开启释放保护

控制台 / API

{% stepper %}
{% step %}
### 控制台操作（开启释放保护）

1. 访问 ECS 控制台 - 实例： https://ecs.console.aliyun.com/server/region ，选择目标实例所在地域与资源组。
2. 单击目标实例 “操作” 列下的图标 > 实例属性 > 开启实例释放保护。
3. 在弹出的对话框中，单击 开启保护。
{% endstep %}

{% step %}
### API（开启释放保护）

调用 ModifyInstanceAttribute，将 DeletionProtection=true 表示开启实例释放保护：\
https://help.aliyun.com/zh/ecs/developer-reference/api-ecs-2014-05-26-modifyinstanceattribute#doc-api-Ecs-ModifyInstanceAttribute ""

预期效果：释放实例时报错 InvalidOperation.DeletionProtection。
{% endstep %}
{% endstepper %}

#### 关闭释放保护

控制台 / API

{% stepper %}
{% step %}
### 控制台操作（关闭释放保护）

1. 访问 ECS 控制台 - 实例： https://ecs.console.aliyun.com/server/region ，选择目标实例所在地域与资源组。
2. 单击目标实例 “操作” 列下的图标 > 实例属性 > 关闭实例释放保护。
3. 在弹出的对话框中，单击 关闭保护。
{% endstep %}

{% step %}
### API（关闭释放保护）

调用 ModifyInstanceAttribute，将 DeletionProtection=false 表示关闭实例释放保护：\
https://help.aliyun.com/zh/ecs/developer-reference/api-ecs-2014-05-26-modifyinstanceattribute#doc-api-Ecs-ModifyInstanceAttribute ""
{% endstep %}
{% endstepper %}

#### 查看释放保护状态

控制台 / API

{% stepper %}
{% step %}
### 控制台查看状态

1. 访问 ECS 控制台 - 实例： https://ecs.console.aliyun.com/server/region ，选择目标实例所在地域与资源组。
2. 单击目标实例ID进入实例详情页，在 其他信息 区域查看 释放保护 状态。
{% endstep %}

{% step %}
### API 查看状态

调用 DescribeInstances，查看 Instances.Instance.DeletionProtection；为 true 则代表已开启实例释放保护：\
https://help.aliyun.com/zh/ecs/developer-reference/api-ecs-2014-05-26-describeinstances ""
{% endstep %}
{% endstepper %}

***

### 应用于生产环境（最佳实践）

* 强制开启释放保护：为所有生产环境实例强制开启释放保护，防止因人为误操作或恶意行为导致核心资产被意外删除。
* 权限控制：仅为必要人员授予释放实例的权限（ecs:DeleteInstance 和 ecs:DeleteInstances）。同时，将修改实例属性（关闭释放保护）的权限（ecs:ModifyInstanceAttribute）与删除实例的权限分离，交由不同角色管理。
* 操作审计：启用 操作审计 服务，记录所有账号内的操作，为 DeleteInstance 和 ModifyInstanceAttribute 等高危事件配置告警规则，以便在发生此类操作时立即收到通知，从而进行追踪、审计和快速响应。https://help.aliyun.com/zh/actiontrail/product-overview/what-is-actiontrail ""

***

### 常见问题（FAQ）

<details>

<summary>为什么实例“释放”按钮是灰色，无法点击？</summary>

可能实例是 未到期的包年包月实例，此实例释放前需先退订： https://help.aliyun.com/zh/ecs/refund-instructions#f5c9488c1231k ""

</details>

<details>

<summary>“停止”实例和“释放”实例有什么区别？</summary>

这是两个完全不同的概念。

* 停止：类似于关机，可以随时 启动 实例恢复运行。
* 释放：类似于删除和销毁。实例和其系统盘、本地盘、部分数据盘（开启了随实例释放）将被 永久删除，数据无法恢复。操作后，该资源将从实例列表移除。

</details>

<details>

<summary>能恢复一台刚刚被误释放的实例吗？</summary>

不能。释放实例是永久性、不可撤销操作。系统不会保留已释放实例的任何数据或配置，也无法通过技术手段恢复。请严格遵守 先备份、后释放 的原则。

</details>

<details>

<summary>释放了实例，但为什么还在扣费？</summary>

释放实例不等于释放所有关联资源。可通过 账单 排查是否有关联的 独立云盘、弹性公网IP（EIP）或快照 等资源仍在计费： https://help.aliyun.com/zh/ecs/view-billing-details ""

</details>

<details>

<summary>如何批量释放实例？</summary>

* 控制台：在实例列表页执行批量操作 https://help.aliyun.com/zh/ecs/user-guide/batch-operation-of-ecs-instances-through-the-ecs-console ""
* API：DeleteInstances - 批量删除实例 https://help.aliyun.com/zh/ecs/developer-reference/api-ecs-2014-05-26-deleteinstances ""

</details>

<details>

<summary>释放实例时设置了定时释放，如何取消？</summary>

在 ECS 控制台 - 实例：https://ecs.console.aliyun.com/server/region 单击实例ID进入实例详情页，在 基本信息 > 自动释放时间 中，单击 取消。

</details>

***

如果需要查看相关 API 文档或控制台页面，文中已保留所有原始链接以便参考。
