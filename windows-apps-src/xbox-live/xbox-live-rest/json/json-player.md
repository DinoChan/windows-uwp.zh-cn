---
title: Player (JSON)
assetID: eaf6d082-869b-d2d3-d548-5cef65e54541
permalink: en-us/docs/xboxlive/rest/json-player.html
author: KevinAsgari
description: " Player (JSON)"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 25d9262ac16eab3d1c2f35960445321fa3872c30
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "4059672"
---
# <a name="player-json"></a>Player (JSON)
玩家数据包含在游戏会话中。 
<a id="ID4EN"></a>

 
## <a name="player"></a>玩家
 
玩家对象具有以下规范。
 
| 成员| 类型| 说明| 
| --- | --- | --- | 
| customData| 8 位无符号整数的数组| 1024 字节的 Base64 编码的特定于游戏的玩家数据。 此值不透明到服务器。| 
| 玩家代号| 字符串| 玩家代号，最多 15 个字符的 — 的玩家。 在确定玩家时，客户端应在 UI 中使用此值。 | 
| isCurrentlyInSession| 布尔值| 指示是否玩家当前在会话中或离开会话。| 
| seatIndex| 32 位有符号的整数| 在会话中玩家的索引。| 
| xuid| 64 位无符号的整数| Xbox 用户 ID (XUID) 的玩家。| 
  
<a id="ID4E3C"></a>

 
## <a name="sample-json-syntax"></a>JSON 语法示例
 

```json
{
    "xuid": 2533274790412952,
    "gamertag":"MyTestUser",
    "seatindex": 3
    "customData":"AIHJ2?iE?/jiKE!l5S=T..."
    "isCurrentlyInSession":"true"
}
    
```

  
<a id="ID4EFD"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EHD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[JavaScript 对象表示法 (JSON) 对象参考](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference"></a>参考 

[GameSession (JSON)](json-gamesession.md)

   