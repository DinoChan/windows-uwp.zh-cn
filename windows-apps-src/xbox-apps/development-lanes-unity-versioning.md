---
author: JordanEllis6809
title: Unity - 对你的 UWP 项目进行版本控制
description: 对你的 Unity UWP 项目进行版本控制。
ms.openlocfilehash: 3b796c31e6b284cea628ba68a34799cf9317ee2e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.locfileid: "205687"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity：对你的 UWP 项目进行版本控制

仍未使用通用 Windows 平台 (UWP) 为 Xbox 生成 Unity 游戏？  请先参阅[将 Unity 游戏引入 Xbox 上的 UWP](development-lanes-unity.md)。

关于为何要将你生成的 UWP 目录的各个部分添加到版本控制有多种不同的原因，其中一个原因是为了添加依赖项（例如 Xbox Live SDK）。  我们将在此教程中使用此方案作为示例，希望它能帮助你解决你的项目的个别需求。

***免责声明：我们将使用 Git 作为版本控制解决方案。  如果你的解决方案有所不同，概念应该仍然相通。***

若要刷新内存，这是我们的游戏 ***ScrapyardPhoenix*** 的目录的当前外观：

![生成目标文件夹](images/build-destination.png)

这是我们的 UWP 目录的外观：

![UWP 与解决方案](images/uwp-vs-solution.png)

在此目录中，我们只关心一个文件夹，即 ***ScrapyardPhoenix***（在此处插入你的游戏的名称）文件夹。  在版本控制中，可以忽略其他所有内容。

***不了解什么是 .gitignore 文件？  请参阅 [gitignore](https://git-scm.com/docs/gitignore)。***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/

我们将从 **UWP/ScrapyardPhoenix** 文件夹内选择一些不同的文件和文件夹添加到我们的版本控制。  首先让我们详细查看完整内容：

![UWP 生成目录](images/uwp-build-directory.png)  

## <a name="folders"></a>文件夹  

`Assets` | ***Include*** | 包含 Windows 应用商店图像  
`Data`   | ***Ignore*** | Unity 将你的项目编译到的位置（Scenes、Shaders、Scripts、Prefabs 等）  
`Dependencies` | ***Include*** | 此文件夹是创建用于保留所有 UWP 依赖项（例如 XboxLiveSDK.dll）的文件夹  
`Properties` | ***Include*** | 包含可由开发人员修改的更多高级设置  
`Unprocessed` | ***Ignore*** | 包含 Unity `.dll` 和 `.pdb` 文件  

## <a name="files"></a>文件  

`App.cs` | ***Include*** | UWP 应用程序的入口点；可进行修改并使用其他源文件进行扩展  
`Package.appxmanifest` | ***Include*** | 你的 AppX 的程序包清单  
`project.json` | ***Include*** | 描述你的 `*.csproj` 所依赖的 NuGet 包  
`ScrapyardPhoenix.csproj` | ***Include*** | 描述你的 UWP 生成目标；如果你将其他依赖项添加到 UWP 项目，此 `*.csproj` 文件将包含该信息  
`ScrapyardPhoenix.csproj.user` | ***Ignore*** | 此文件包含本地用户信息

## <a name="resulting-gitignore"></a>生成的 .gitignore

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

于是，现在你的团队成员将与你生成的 UWP 项目同步。 现在你可以向你的 UWP 项目随意添加其他资源、源和依赖项。

有关对 UWP 文件夹进行版本控制的一些更多示例可以在[这些示例](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)中找到。

## <a name="adding-dependencies-to-your-uwp-app"></a>向 UWP 应用添加依赖项

通过将依赖项放置在 **Plugins** 文件夹下的 **Unity Assets** 文件夹中将其添加到 DLL 和 WINMD，然后选择它们并在检查器中相应地设置其目标平台设置。

![UWP 解决方案](images/uwp-solution.PNG)

例如，***ScrapyardPhoenix（通用 Windows）*** 是你将添加对 Xbox Live SDK 的引用的项目。

## <a name="see-also"></a>另请参阅
- [将现有游戏移植到 Xbox](development-lanes-landing.md)
- [Xbox One 上的 UWP](index.md)