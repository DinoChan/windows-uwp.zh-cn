---
author: jnHs
Description: You can promote your app or add-on in the Microsoft Store by putting it on sale for a limited time.
title: 打折出售应用和加载项
ms.assetid: 71ABA960-0CDC-4E35-A1C8-1D34B6673817
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a25980c964e399931a088e281e959df3095ffe9a
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "2840794"
---
# <a name="put-apps-and-add-ons-on-sale"></a>打折出售应用和加载项

你还可以通过限时折扣在 Microsoft Store 中促销你的应用或加载项。 可以选择以更低的价格段或基于百分比的折扣提供产品。

> [!NOTE]
> 订阅加载项不支持销售定价。

使用提交的**定价和可用性**页的**促销价**部分暂时降低应用或加载项的价格时，查看“应用商店一览”的客户将看到删除线定价，指示价格降低（与[计划的价格更改](set-and-schedule-app-pricing.md#schedule-price-changes)不同，后者可以在不在应用商店中显示更改的情况下降低或提高价格）。 

将产品进行促销后，客户将能够在所选时间段内以更低价格购买该产品。 如果将价格降为**免费**，客户可以在促销期间免费下载产品。

> [!IMPORTANT]
> 促销价格将仅显示给使用 Windows 10 设备（包括 Xbox One）的客户。 向拥有其他产品的用户提供的促销价仅显示给使用 Windows 10 版本 1607 或更高版本的客户。
> 
> 在其他操作系统上，客户看到的是应用或加载项的常规价格，将无法以促销价进行购买。 你始终可以通过在新提交中选择其他价格区间来更改价格，但该价格不以限时促销的形式显示。


## <a name="scheduling-a-sale"></a>计划一次促销

促销将作为应用或加载项提交的一部分进行计划。 如果你想要为已发布的应用或加载项计划一次促销，你将需要创建新的提交，即使这是你想要进行的唯一更改也是如此。

**计划一次促销**

1. 在进行中的应用或加载项提交的**定价和可用性**页上，转到**促销价**部分。
2. 选择**显示选项**，然后选择**新建促销**。
3. 将出现**市场选择**弹出窗口，允许你创建指定应提供促销的市场的*市场组*。 你可以单击**全部选择**以将促销提供给你的应用可用的每个市场，也可选择单个市场或选择多个市场。 你可以选择性地为你的市场组输入名称。 当你完成选择后，请单击**创建**。 （若要在稍后编辑组中的市场，请单击其名称。）

   > [!NOTE]
   > 你在“促销价格”部分中所选的市场不会对提供该应用的市场产生任何影响；这些选择仅确定是否提供促销价格，以及在哪些市场中提供。 如果你为不提供你的应用的市场设置促销价格，这并不会使得该应用在该市场中可用。
4. 选择以下选项之一指定折扣类型：
   - **价格**：使用此选项选择你的应用将提供的更低的价格段。 你可以更改货币下拉列表以选择你偏爱的货币价格。 （价格将转换为每种货币的相应价格段。 有关详细信息，请参阅[定价](set-app-pricing-and-availability.md)。
   - **百分比**：使用此选项选择将应用于你的应用的折扣百分比。 所有货币都使用相同的折扣百分比。
5. 在**提供对象**行中，选择以下可用选项之一，包括：
   - **每个人**：将向所有客户提供促销。
   - **所有者**：将向已拥有其中一个应用的客户提供促销。 可以从显示的下拉框中选择已发布的应用。 必须拥有一个或多个已发布的应用，才能使用此选项。

  > [!IMPORTANT]
  > 如果选择**所有者**，促销将仅对使用 Windows 10 版本 1607 或更高版本的客户可见。

   - **已知用户组**：将向选择的[已知用户组](create-known-user-groups.md)中的人员提供促销。 必须已经创建了已知用户组才能使用此选项。
   - **类别**：将向所选客户类别中的人员提供促销。 你可以在此处使用[你已创建的类别](create-customer-segments.md)。 你还可以选择**首次付款人**以仅向从未在应用商店中购买过任何产品的客户提供促销。 我们在此处提供此类别是因为我们发现在客户进行第一次应用商店购买后，他们通常都会继续进行更多购买，因此这是一个非常适合利用促销价吸引的组。
6. 输入此次促销期开始和结束的日期和时间。 从以下时区选项中选择一项：
   - **UTC**：选择协调世界时 (UTC)，以便在世界各地的同一时间进行促销。
   - **本地**：你选择的时间将用于与市场相关联的每个时区。 （请注意，对于跨多个时区的市场，将仅使用该市场中的某一个时区。 对于美国，将使用东部时区。）
7. 若要计划其他促销，请选择**新建促销**。 否则，请单击**定价和可用性**页底部的**保存**，然后从提交概述中选择**提交到应用商店**。

> [!NOTE]
> 可选择高于你的应用基价的价格段。 但是，如果促销价格低于该市场中应用的常规价格，该促销价格将仅显示给客户。
>
> 如果你已在某些市场中设置了高于你的应用基价的自定义价格，并且你想要在这些市场中临时调低价格（但促销价格仍高于应用基价），则选择一个高于应用基价的价格或许适合此次促销。 如果你的选择导致某些市场中该应用的价格出现了提升，我们不会将该价格（较高价）显示给该市场中的客户；他们仍将看到该应用之前的价格（较低价）。 如果你计划在单个的重叠促销中推出不同的促销价格，我们还会向客户显示可用的最低价格。

## <a name="changing-or-canceling-a-scheduled-sale"></a>更改或取消计划的促销

若要修改或取消你之前为应用或加载项计划的促销，你将需要创建新的提交并将其提交到应用商店。

**编辑计划的促销**

1.  在进行中的应用或加载项提交的**定价和可用性**页上，转到**促销价**部分。
2.  找到你要更新的促销，然后进行更改。
3.  单击**定价和可用性**页底部的**保存**，然后从提交概述中单击**提交到应用商店**。

在你的提交经过认证过程后，这些更改便会生效。

> [!IMPORTANT]
> 如果促销已开始，你将无法编辑开始日期。 在你可以编辑结束日期时，我们建议你不要将促销编辑为早于其原始结束日期结束。 如果你在最初发布的结束日期之前结束促销，可能会让潜在客户感到懊恼（因为客户在查看应用的“应用商店一览”时会看到计划的结束日期）。

 **若要取消未尚开始的促销，请执行以下操作：**

1.  在进行中的应用或加载项提交的**定价和可用性**页上，转到**促销价**部分。
2.  找到要取消的促销，然后单击**删除**。
3.  单击**定价和可用性**页底部的**保存**，然后从提交概述中单击**提交到应用商店**。 只要促销在新的提交完成认证过程前尚未开始，那么已删除的促销就不会运作。



