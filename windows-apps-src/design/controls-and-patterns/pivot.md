---
author: serenaz
Description: The Pivot control enables touch-swiping between a small set of content sections.
title: Pivot
template: detail.hbs
ms.author: sezhen
ms.date: 06/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e8f0fbbfacc3fa4edb602f7505ea1e88f211a81a
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/24/2018
ms.locfileid: "2840336"
---
# <a name="pivot"></a>Pivot

[数据透视表](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)控件使触摸刷之间一小组内容部分。

> **重要的 Api**: [Pivot 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)、 [NavigationView 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.NavigationView)

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处到<a href="xamlcontrolsgallery:/item/Pivot">打开应用程序，请参阅操作中的数据透视表控件</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

数据透视表控件，就像[NavigationView](navigationview.md)，加下划线的选定的项。

![默认焦点为选择的标题添加下划线](images/pivot_focus_selectedHeader.png)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

若要获得常见的顶部导航和选项卡模式，建议使用[NavigationView](navigationview.md)，其自动适应不同的屏幕大小并允许更大的自定义。

但是，如果您的导航需要触摸刷，我们建议使用数据透视表。

NavigationView 和数据透视表控件之间的其他主要差异是默认溢出行为和导航 API:

- 数据透视表笔盘溢出项目，而 NavigationView 使用下拉菜单溢出，以便用户可以看到所有项目。
- 数据透视处理内容部分，而允许更好地控制导航行为 NavigationView 之间导航。

## <a name="use-navigationview-instead-of-pivot"></a>使用 NavigationView 而不是数据透视表

如果您的应用程序用户界面使用的数据透视表控件，然后您可以将转换数据透视表为 NavigationView 与下面的代码。

此 XAML 3 各节内容的内容，如示例中[创建数据透视表控件](#create-a-pivot-control)的数据透视表所示创建 NavigationView。

```xaml
<NavigationView x:Name="rootNavigationView" Header="Category Title"
 ItemInvoked="NavView_ItemInvoked">
    <NavigationView.MenuItems>
        <NavigationViewItem Content="Section 1" x:Name="Section1Content" />
        <NavigationViewItem Content="Section 2" x:Name="Section2Content" />
        <NavigationViewItem Content="Section 3" x:Name="Section3Content" />
    </NavigationView.MenuItems>
</NavigationView>

<Page x:Class="AppName.Section1Page">
    <TextBlock Text="Content of section 1."/>
</Page>

<Page x:Class="AppName.Section2Page">
    <TextBlock Text="Content of section 2."/>
</Page>

<Page x:Class="AppName.Section3Page">
    <TextBlock Text="Content of section 3."/>
</Page>
```

NavigationView 提供更好地控制导航自定义和需要相应的代码隐藏。 与配套上述 XAML，请使用下面的代码隐藏：

```csharp
private void NavView_ItemInvoked(NavigationView sender, NavigationViewItemInvokedEventArgs args)
{
    FrameNavigationOptions navOptions = new FrameNavigationOptions();
    navOptions.TransitionInfoOverride = new SlideNavigationTransitionInfo() {
         SlideNavigationTransitionDirection=args.RecommendedNavigationTransitionInfo
    };

    navOptions.IsNavigationStackEnabled = False;

    switch (item.Name)
    {
        case "Section1Content":
            ContentFrame.NavigateToType(typeof(Section1Page), null, navOptions);
            break;

        case "Section2Content":
            ContentFrame.NavigateToType(typeof(Section2Page), null, navOptions);
            break;

        case "Section3Content":
            ContentFrame.NavigateToType(typeof(Section3Page), null, navOptions);
            break;
    }  
}
```

此代码模拟数据透视表控件的内置导航体验，请减去内容部分之间的触摸刷体验。 但是，您可以看到，也无法定制几点，包括动画的转换、 导航参数和堆栈功能。

## <a name="create-a-pivot-control"></a>创建透视表控件

此代码 3 各节内容的内容创建基本的数据透视表控件。

```xaml
<Pivot x:Name="rootPivot" Title="Category Title">
    <PivotItem Header="Section 1">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 1."/>
    </PivotItem>
    <PivotItem Header="Section 2">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 2."/>
    </PivotItem>
    <PivotItem Header="Section 3">
        <!--Pivot content goes here-->
        <TextBlock Text="Content of section 3."/>
    </PivotItem>
</Pivot>
```

### <a name="pivot-items"></a>透视表项目

透视表是一个 [ItemsControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.aspx)，因此可以包含任何类型的项目集合。 你添加到透视表的任何非显式 [PivotItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivotitem.aspx) 的项目都隐式包装在 PivotItem 中。 由于透视表经常用于在内容页面之间导航，因此通常使用 XAML UI 元素直接填充 [Items](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.items.aspx) 集合。 或者，你可以将 [ItemsSource](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemssource.aspx) 属性设置为数据源。 ItemsSource 中绑定的项目可以属于任何类型，但如果它们不是显式 PivotItems，则必须定义 [ItemTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) 和 [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.headertemplate.aspx) 来指定这些项目的显示方式。

你可以使用 [SelectedItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selecteditem.aspx) 属性获取或设置透视表的活动项目。 使用 [SelectedIndex](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.selectedindex.aspx) 属性获取或设置活动项目的索引。

### <a name="pivot-headers"></a>透视表标题

你可以使用 [LeftHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.leftheader.aspx) 和 [RightHeader](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.pivot.rightheader.aspx) 属性将其他控件添加到透视表标题。

例如，你可以在透视表的 RightHeader 中添加 [CommandBar](https://docs.microsoft.com/en-us/windows/uwp/controls-and-patterns/app-bars)。

```xaml
<Pivot>
    <Pivot.RightHeader>
        <CommandBar>
                <AppBarButton Icon="Add"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Edit"/>
                <AppBarButton Icon="Delete"/>
                <AppBarSeparator/>
                <AppBarButton Icon="Save"/>
        </CommandBar>
    </Pivot.RightHeader>
</Pivot>
```

### <a name="pivot-interaction"></a>透视表交互

该控件的特色之处在于以下触摸手势交互：

- 通过点击透视表项目标题，可导航到该标题的部分内容。
- 通过在透视表项目标题上向左或向右轻扫，可导航到相邻部分。
- 通过在部分内容上向左或向右轻扫，可导航到相邻部分。

该控件有以下两种模式：

**固定不动**

- 当所有透视表标题都适合所允许的空间时，透视表将固定不动。
- 点击某个透视表标签即可导航到相应的页面，即使透视表无法自行移动也是如此。 活动透视表将突出显示。

**旋转**

- 当所有透视表标题不适合所允许的空间时，可旋转透视表。
- 点击某个透视表标签即可导航到相应的页面，并且活动透视表标签将旋转至第一个位置。
- 从最后一个到第一个透视表部分的旋转循环中的透视表项目。

> **注意**透视表标题不应在 [10 英尺环境](../devices/designing-for-tv.md)中旋转。 如果你的应用将在 Xbox 上运行，请将 [IsHeaderItemsCarouselEnabled](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot.IsHeaderItemsCarouselEnabled) 属性设置为 **false**。

## <a name="recommendations"></a>建议

- 当使用旋转（来回旋转）模式时，避免使用 5 个以上的标题，因为超过 5 个标题的循环可能会令人困惑。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-topics"></a>相关主题

- [透视表类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Pivot)
- [导航设计基础知识](../basics/navigation-basics.md)