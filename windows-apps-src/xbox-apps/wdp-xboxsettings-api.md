---
author: m-stahl
title: Device Portal Xbox 开发人员设置 API 参考
description: 了解如何访问 Xbox 开发人员设置。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: f231fb85ed5ef4f9bcee75736157bd8630a6cbfa
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "662499"
---
# <a name="developer-settings-api-reference"></a>开发人员设置 API 参考   
你可以使用此 API 访问有助于开发的 Xbox One 设置。

## <a name="get-all-developer-settings-at-once"></a>一次性获取所有开发人员设置

**请求**

你可以使用以下请求，在一次请求中获取所有开发人员设置。

方法      | 请求 URI
:------     | :-----
GET | /ext/settings
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**   
该响应是一个设置 JSON 数组，包含所有设置。 每个设置对象都包含以下字段：

* Name -（字符串）设置的名称。
* Value -（字符串）设置的值。
* RequiresReboot -（“Yes”|“No”）此字段指示该设置是否需要重新启动才能生效。
* Disabled - ("Yes" | "No") 此字段指示设置是否已禁用且无法进行编辑。
* Category -（字符串）设置的类别。
* Type - ("Text" | "Number" | "Bool" | "Select") 此字段指示设备的类型：文本输入、布尔值（“true”或“false”），具有最小值和最大值的数字或者在特定的值列表中进行选择。

如果设置为一个号码：
* Min-（数目） 此字段表示设置的最小数值。
* 最大-（数目） 此字段表示设置的最大数值。

如果设置为选择：
* OptionsVariable-("是"|"No") 此字段指示设置选项是否变量，如果有效选项可以更改而不重新启动。
* 选项 - JSON 数组，将有效的选择选项包含为字符串。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

## <a name="get-settings-one-at-a-time"></a>一次获取一个设置
设置也可以逐个检索。

**请求**

你可以使用以下请求来获取有关各个设置的信息。

方法      | 请求 URI
:------     | :-----
GET | /ext/settings/\&lt;setting name\&gt;
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**响应**   
该响应是一个具有以下字段的 JSON 对象：

* Name -（字符串）设置的名称。
* Value -（字符串）设置的值。
* RequiresReboot -（“Yes”|“No”）此字段指示该设置是否需要重新启动才能生效。
* Disabled - ("Yes" | "No") 此字段指示设置是否已禁用且无法进行编辑。
* Category -（字符串）设置的类别。
* Type - ("Text" | "Number" | "Bool" | "Select") 此字段指示设备的类型：文本输入、布尔值（“true”或“false”），具有最小值和最大值的数字或者在特定的值列表中进行选择。

如果设置为一个号码：
* Min-（数目） 此字段表示设置的最小数值。
* 最大-（数目） 此字段表示设置的最大数值。

如果设置为选择：
* OptionsVariable-("是"|"No") 此字段指示设置选项是否变量，如果有效选项可以更改而不重新启动。
* 选项 - JSON 数组，将有效的选择选项包含为字符串。

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

## <a name="set-the-value-of-a-setting"></a>设置某个设置的值
你可以设置某个设置的值。

**请求**

你可以使用以下请求设置某个设置的值。

方法      | 请求 URI
:------     | :-----
PUT | /ext/settings/\&lt;setting name\&gt;
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**   
请求正文是 JSON 对象，包含以下字段：   
Value -（字符串）设置的新值。

**响应**   

- 无

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 请求已成功
4XX | 错误代码
5XX | 错误代码

<br />
**可用设备系列**

* Windows Xbox