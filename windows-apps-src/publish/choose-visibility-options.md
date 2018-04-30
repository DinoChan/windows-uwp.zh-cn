---
author: jnHs
Description: TSet restrictions on how your app can be discovered and acquired, including whether people can find your app in the Store or see its Store listing at all.
title: 选择可见性选项
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 可见性, 私人受众, 可用的, 可检测到的
ms.localizationpriority: high
ms.openlocfilehash: ae4e467305f91f9faedd7aa1a28a591216a93ee1
ms.sourcegitcommit: 1eabcf511c7c7803a19eb31f600c6ac4a0067786
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="choose-visibility-options"></a>选择可见性选项


[定价和可用性页面](set-app-pricing-and-availability.md)中的**可见性**部分允许你对发现和获取应用的方式设置限制。 因此你可以选择指定用户是否可以在 Microsoft Store 中找到你的应用或查看该应用的 Microsoft Store 一览。

“可见性”部分中有两个独立部分：**受众**和**可发现性**。 

## <a name="audience"></a>受众

“受众”部分让你可以指定是否希望将你的提交的可见性限制为定义的特定受众。


### <a name="public-audience"></a>公共受众

默认情况下，**公共受众**可以看到你的应用的 Microsoft Store 一览。 这适用于大多数提交，除非你希望将可以查看应用一览的用户限制为特定用户。 如果你愿意，还可以使用[可发现性](#discoverability)部分中的选项来限制可发现性。

> [!IMPORTANT]
> 如果你提交的产品将此选项设置为**公共受众**，将无法在以后的提交中选择**私人受众**。


### <a name="private-audience"></a>私人受众

如果你希望只有指定的所选用户可以看到应用一览，请选择**私人受众**。 使用此选项，对于指定组中用户以外的任何用户，应用将不可发现或不可用。 此选项通常用于[beta 版测试](beta-testing-and-targeted-distribution.md)，因为它让你可以将应用分配给测试人员，而任何其他人则无法获取该应用，甚至无法查看该应用的 Microsoft Store 一览（即使他们能够键入该应用的 Microsoft Store 一览 URL）。

当你选择**私人受众**时，需要指定至少一组应获取应用的用户。 你可以从现有[已知用户组](create-known-user-groups.md)中选择，也可以选择**创建新组**来定义一个新组。 你需要输入与要包括在该组中的每个用户的 Microsoft 帐户关联的电子邮件地址。 有关详细信息，请参阅[创建已知用户组](create-known-user-groups.md)。

发布你的提交后，指定组中的用户将能够查看该应用一览并下载应用，前提是他们必须使用与你输入的电子邮件地址关联的 Microsoft 帐户登录，并且运行的是 Windows 10 版本 1607 或更高版本（包括 Xbox One）。 但是，不属于你的私人受众的用户将无法查看该应用一览或下载该应用（无论他们运行的是哪个操作系统版本）。 你可以将更新的提交发布给私人受众，然后将按照常规应用更新的方式分发给这些受众成员（但不属于你的私人受众的用户仍然无法使用，除非你更改受众选择）。 

如果你打算在特定的日期和时间将应用提供给公共受众，则可以在创建你的提交时选择名为**公开此产品**的框。 输入要将产品提供给公众的日期和时间（UTC 时间）。 请牢记以下几点：

- 你选择的日期和时间适用于所有市场。 如果你希望针对不同市场自定义发布计划，请勿使用此框。 而是创建一个将设置更改为**公共受众**的新提交，然后使用[计划](configure-precise-release-scheduling.md)选项指定发布时间。
- 输入**公开此产品**的日期对于适用于企业的 Microsoft Store 和/或适用于教育的 Microsoft Store 则不适用。 若要允许我们通过组织许可向这些客户提供你的应用，你需要在创建新提交时选择**公共受众**（并启用[组织许可](organizational-licensing.md)）。
- 选择日期和时间后，将来的所有提交将使用**公共受众**。

如果你不指定向公共受众提供应用的日期和时间，可以在以后随时执行此操作，方法是创建一个新提交并将受众设置从**私人受众**改为**公共受众**。 在执行此操作时，请记住你的应用可能需要通过其他认证过程，因此请准备好应对可能出现的任何新认证问题。 

以下是在选择将应用分配给私人受众时要记住的一些重要事项：
- 你的私人受众中的用户将能够通过使用应用的 Microsoft 帐户一览的特定链接来获取应用，而他们需要使用 Microsoft 帐户登录才能查看链接。 当你选择**私人受众**时会提供此链接。 你还可以在[应用标识](view-app-identity-details.md)页的 **URL 下方找到该应用，条件是只有特定用户可以看到你的应用（需要身份验证）**。 请务必向测试人员提供此链接，而不是 Microsoft Store 一览的常规 URL。  
- 除非你选择**可发现性**中一个阻止发现的选项，否则私人受众中的用户将能够通过在 Microsoft Store 应用中进行搜索来找到你的应用。 但是，将无法通过搜索发现 Web 一览，即使对于属于此类受众的用户也是如此。 
- 你将无法在**定价和可用性页面**中的[计划部分配置发布日期](configure-precise-release-scheduling.md)，因为你的应用将不会发布给私人受众之外的客户。
- 你所做的其他选择适用于此受众中的用户。 例如，如果你选择除**免费**以外的价格，你的私人受众中的用户将需要支付该价格才能获取该应用。 
- 如果你希望将不同的程序包分配给私人受众中的不同用户，在初次提交后可以使用[打包外部测试版](package-flights.md)将不同的程序包更新分配给私人受众的子集。 你可以创建更多的已知用户组来定义哪些用户应获取特定程序包外部测试版。
- 你可以编辑你的私人受众中的已知用户组成员。 但是请记住，如果你要删除之前下载了你的应用的组内已有用户，他们仍然能够使用该应用，但不能获取你提供的任何更新（除非你日后选择**公共受众**）。
- 无论你的组织许可设置如何，都不能通过适用于企业的 Microsoft Store 和/或适用于教育的 Microsoft Store 提供你的应用，甚至对于私人受众中的用户也是如此。
- 虽然 Microsoft Store 确保只有使用 Microsoft 帐户登录并已添加到你的私人受众中的用户可以看到并使用你的应用，但我们无法阻止这些用户向私人受众以外的用户共享信息或屏幕截图。 保密性至关重要，请确保你的私人受众仅包括你相信不会与他人共享你的应用详细信息的用户。
- 请确保测试人员了解如何向你提供反馈。 你可能不希望他们在反馈中心留下反馈，因为其他客户都可以看到该反馈。 请考虑包括一个链接，以便他们发送电子邮件或以其他方式提供反馈。
- 你可以在[评论报告](reviews-report.md)中看到由测试人员撰写的所有评论。 但是，这些评价不会在你的应用的 Microsoft Store 一览中发布，即使将你的提交移至**公共受众**后也是如此。  
- 当你将应用从**私人受众**移至**公共受众**时，Microsoft Store 一览中显示的**发布日期**将是首次向公共受众发布的日期（除非你指定了一个不同的[显示发布日期](set-app-pricing-and-availability.md#display-release-date)）。

## <a name="discoverability"></a>可发现性

**可发现性**部分中的选择指示客户如何发现和获取你的应用。 

> [!IMPORTANT]
> 如果你选择**私人受众**，则你的**可发现性**选择仅适用于你的私人受众中的用户。 不在你指定的组中的客户将无法获取该应用，甚至无法看到该应用一览。 


### <a name="make-this-product-available-and-discoverable-in-the-store"></a>在 Microsoft Store 中提供此产品，并使其可被发现

这是默认选项。 如果你希望应用在 Microsoft Store 中列出，以便客户通过应用的直接链接和/或其他方法（包括搜索、浏览和包含在特选表中）进行查找，请选中此选项。 

### <a name="make-this-product-available-but-not-discoverable-in-the-store"></a>在 Microsoft Store 中提供此产品，但使其不可被发现

当选择此选项时，客户无法通过搜索或浏览的方法在 Microsoft Store 中找到你的应用；获取你的应用一览的唯一方法是使用直接链接。 

> [!TIP]
> 如果你不希望公众查看一览，甚至是通过直接链接查看，请在**受众**部分选择**私人受众**，如上所述。

同时必须选择以下选项中的一个来指定获取你的应用的方式：


>[!IMPORTANT]
> 以下每个选项限制了客户获取你的应用时需使用的操作系统版本。 请仔细阅读说明，以确保知悉所支持的操作系统版本。 

- **仅直接链接：任何具有指向产品一览的直接链接的客户均可下载，使用 Windows 8.x 操作系统的客户除外。** 所有通过直接链接访问你的应用一览的客户均可将其下载到运行 Windows 10 或 Windows Phone 8.1 及更早版本的设备上（但不包括运行 Windows 8.x 的设备）。
- **停止购置：任何具有直接链接的客户均可看到产品的应用商店一览，但仅当曾经拥有过该产品或拥有一个促销代码，同时使用的是 Windows 10 设备时，这些客户才能够下载该产品。** 即使客户具有直接链接，仍无法下载该应用，除非其拥有[促销代码](generate-promotional-codes.md)并且使用的是 Windows 10 设备。 如果客户有一个促销代码，他们可以使用该代码免费获取应用（仅限 Windows 10），即使不再向任何其他客户提供代码时也是如此。 使用促销代码是所有用户获取你的应用的唯一方法。

> [!TIP]
> 如果你想要停止向任何新客户提供应用，则可以从概述页面选择**禁止提供应用**。 在你对禁止提供的应用进行确认后，数小时内它不会再出现在 Microsoft Store 中，并且新客户将无法获取它（除非他们拥有[促销代码](generate-promotional-codes.md)并使用 Windows 10 设备）。 此操作将会覆盖提交中的**可见性**选择。 若要将应用再次提供给新客户（根据**可见性**选择），你可以随时在概述页面单击**提供应用**。 有关详细信息，请参阅[从 Microsoft Store 中删除应用](guidance-for-app-package-management.md#removing-an-app-from-the-store)。



