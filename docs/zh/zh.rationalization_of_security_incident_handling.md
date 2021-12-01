# 安全事件警报的合理化
本文档仅供参考。 它代表了截至本文档发布之日亚马逊 Web Services (AWS) 提供的当前产品和实践，这些产品和做法如有更改，恕不另行通知。 客户有责任对本文档中的信息以及对 AWS 产品或服务的任何使用情况进行自己的独立评估，每种产品或服务都是 “按原样” 提供的，无论是明示还是暗示的担保。 本文档不创建 AWS、其附属公司、供应商或许可方的任何担保、陈述、合同承诺、条件或保证。 AWS 对客户的责任和责任受 AWS 协议的控制，本文档既不是 AWS 与其客户之间的任何协议的一部分，也不修改。

© 2021 亚马逊网络服务公司或其附属公司。 保留所有权利。 本作品根据知识共享署名 4.0 国际许可协议进行许可。

提供此 AWS 内容须遵守 http://aws.amazon.com/agreement 提供的 AWS 客户协议的条款或客户与亚马逊 Web Services, Inc. 或亚马逊网络服务 EMEA SARL 或两者之间的其他书面协议。

> “如果你以同样的活力保护你的回形针和钻石，你很快就会有更多的回形针和更少的钻石。” 美国前国务卿迪恩·拉斯克说

警报由安全控制开发期间工作负载的威胁建模决定。 警报使用由响应式控件定义，但是，它的定义取决于整个安全角度：指令、预防、侦探和响应。

## 定义

### 威胁建模：

* ** 工作负载 **：你想保护什么
* ** 威胁 **：你害怕发生什么
* ** 影响 **：当威胁遇到工作负载时，业务会如何受到影响
* ** 漏洞 **：什么可以促进威胁
* ** 缓解 **：有哪些控制措施来弥补漏洞
* ** 概率 **：实施缓解措施后，威胁发生的可能性有多大

** 威胁的优先级是影响和可能性的一个因素。 **

### 安全控制：

* ** 指令性 ** 控制措施建立了环境将在其中运行的治理、风险和合规模型。
* ** 预防性 ** 控制措施可保护您的工作负载并缓解威胁和漏洞。
* ** 侦测 ** 控件提供了对 AWS 中部署运行的完全可见性和透明度。
* ** 响应性 ** 控制措施有助于修复可能偏离安全基准的情况。


! [图片] (/images/image-caf-sec.png)

## 为了最大限度地减少警报疲劳并更好地处理安全事件，威胁建模上下文应考虑以下内容：

* 工作负载与业务的相关性 (*)。
* 每个工作负载组件的数据分类 (*)。
* 一贯使用的方法，例如 STRIDE（欺骗、篡改、信息披露、拒绝服务和特权提升）
* 云安全准备情况和成熟度。
* 事件响应和威胁狩猎能力。
* 自动修复功能。
* 事件处理自动化成熟度。


(*) *** 工作负载相关性和数据分类 *** 由公司的风险框架举措定义。 示例：

### 工作负载相关性：

** 高 **：重大货币损失和形象感知损失、长期业务影响、复苏成功率低
** 中度 **：可持续的货币损失和形象感知损失、短期业务影响、高度复苏成功
** 低 **：没有可衡量的货币损失和图像感知损失，没有业务影响，复苏不适用

### 数据分类：

** 秘密 **：重大货币损失和形象感知损失、长期业务影响、复苏成功率低
** 保密 **：可持续的货币损失和图像感知损失、短期业务影响、高度复苏成功
** 未分类 **：没有可衡量的货币损失和图像感知损失，没有业务影响，复苏不适用


## 警报优先级的目标是将它们发送到适当的队列：

* 警报分类队列
* 威胁狩猎队列
* 存档队列


定义警报的最终位置的过程取决于之前列举的所有因素。 基本的排队决定如下：

### 警报分类队列：

1. 具体的风险框架任务，包括但不限于行业监管、法定合规性、业务要求。 在这种情况下，工作负载具有 ** 高 ** 相关性或数据被归类为 ** 秘密 **。
2. 启动正式事件响应流程的特定威胁模型要求。
3. 如果工作负载具有 ** 中等 ** 或 ** 高 ** 相关性或数据分类为 ** 秘密 ** 或 ** 机密 ** 的警报，自动修复失败。

### ** 威胁狩猎队列 **:

1. 对于具有 ** 中等 ** 或 ** 高 ** 相关性的工作负载，自动修复成功或数据分类属于 ** 秘密 ** 或 ** 机密 **。
2. 对于具有 ** 低 ** 相关性的工作负载自动修复失败，或者数据分类为 ** 未分类 **。

### 存档队列：

1. 对于相关性 ** 低 ** 的工作负载自动修复成功，或者数据分类为 ** 未分类 **。