---
title: 配置测试参考
description: 了解Auditor功能如何测试Adobe Experience Platform Debugger中的配置。
exl-id: 92b07224-57f1-4891-9923-aa079945e6bc
source-git-commit: 2223e29de6876639c5dbffda4954e114dcd32521
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 68%

---

# 配置测试参考

此参考可提供有关Adobe Experience Platform Debugger中的Auditor功能如何运行配置测试的更多信息。

>[!NOTE]
>
>有关Platform Debugger中Auditor测试的更多信息，请参阅 [auditor功能概述](./overview.md).

配置测试会扫描实施中的特定设置、值，或者有无潜在冲突。Platform Auditor 会根据其他规则和推荐的最佳做法来评估标记。

| 测试 | 粗细 | 标准 | 推荐 |
| --- | --- | --- | --- |
| Advertising Cloud - 转化名称仅使用字母数字字符 | 3 | 此 `ev_conversion_property_name` 参数应仅包含数字和小数值，但 `ev_transid` 参数，可以包含文本或数值。 查找 `everesttech.net` 像素，其中包含以 `ev_` _ 开头的 URL 参数。 | 确保事务属性参数仅包含数值和小数值。<br><br>警告：任何其他值类型都可能会导致数据丢失。 |
| Advertising Cloud - 转化名称使用 URL 安全字符 | 3 | 转化属性名称不应包含 &amp; 符号或问号。 | 确保事务属性参数不包含非编码的 &amp; 符号或问号。这会破坏 URL 格式。<br><br>警告：包含非编码&amp;符号或问号的属性参数(例如：  `ev_formComplete?=1` 或  `ev_formComplete&Submit=1`)，可能会导致数据丢失。 |
| Advertising Cloud - 已正确实施事务 ID | 1 | 属性名称  `ev_transid=` 不应为空。 | 属性名称  `ev_transid=` 不应保留为空白。 如果没有任何值，则可能会丢失事务数据。将值分配给 `ev_transid=` 或者从像素中删除参数。 |
| Analytics - 在 DOM 中实例化 | 5 | 未安装或无法执行 Adobe Analytics 代码。在网页上找不到Analytics代码时返回0。 | 验证是否已在页面上实施 Analytics 标记，且没有遭到后续脚本活动的阻止。<br><br>[其他信息](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=zh-Hans) |
| Analytics - 实例化一次 | 5 | 在页面上多次检测到 Adobe Analytics 代码。在网页上找不到A-Analytics代码时返回0。 | 确保页面上仅有一个 Analytics 标记。<br><br>[其他信息](https://experienceleague.adobe.com/docs/analytics/implementation/home.html?lang=zh-Hans) |
| Analytics - 最新版本 | 3 | 您的页面没有运行最新版本的 Analytics 代码库。为了利用性能改进并提供最新功能，我们会不断地更新和调整旨在支持 Experience Cloud 技术的代码库。在网页上找不到Analytics代码时返回0。 | 安装最新版本的 Analytics 代码库。<br><br>[其他信息](https://experienceleague.adobe.com/docs/analytics/implementation/appmeasurement-updates.html?lang=zh-Hans) |
| Launch - DOM就绪后，异步加载第三方标记 | 3 | 为了在良好的用户体验与收集准确的数据之间取得平衡，应在DOM就绪时触发第三方标记。 这将可以确保执行相关的跟踪脚本，同时又不会影响站点的正常运转。 | 通过调整所有执行第三方像素的规则，使其在DOM就绪时触发，来解决此问题。<br><br>[其他信息](https://experienceleague.adobe.com/docs/experience-platform/tags/ui/rules.html) |
| Experience Cloud ID 服务 - 最新版本 | 2 | 您的页面没有运行最新版本的访客 ID 服务代码库 visitorAPI.js。为了利用性能改进并提供最新功能，我们会不断地更新和调整旨在支持 Experience Cloud 技术的代码库。 | 安装最新版本的访客 ID 服务代码库。<br><br>[其他信息](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/library.html) |
| Launch - 最新版本 | 2 | 这些页面未运行最新版本的标记代码库(Turbine)。 为了利用性能改进并提供最新功能，我们会不断地更新和调整旨在支持 Experience Cloud 技术的代码库。 | 重建并发布标记库。<br><br>[其他信息](https://experienceleague.adobe.com/docs/experience-platform/tags/get-started/quick-start.html) |
| Target - 最新版本 | 2 | 您的页面没有运行最新版本的 Target 代码库。为了利用性能改进并提供最新功能，我们会不断地更新和调整旨在支持 Experience Cloud 技术的代码库。 | 安装最新版本的 Target 代码库。<br><br>[其他信息](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/implement-target-for-client-side-web.html) |
| Target - mboxDefault 先于 mboxCreate | 5 | mboxCreate 的正确用法类似于下面的示例：<br><br> `<div class="mboxDefault"><!-Customer content--></div><script>mboxCreate('myMboxName')</script>` | 确保包括  `<div class="mboxDefault"></div>` 标记。 at.js 并不会为您添加此标记。<br><br>[其他信息](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/implement-target-for-client-side-web.html) |
| Target - 有效的 DOCTYPE | 5 | 检测到无效的 DOCTYPE。在这种情况下，将不会触发 mbox。对于 at.js，DOCTYPE 必须处于“标准”模式，否则 Target 将无法正常运作。 | 更新页面上的 DOCTYPE。<br><br>[其他信息](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/faq-at-js/target-atjs-faq.html) |

{style="table-layout:auto"}
