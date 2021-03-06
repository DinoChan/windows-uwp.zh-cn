---
title: 延长显示初始屏幕的时间
description: 通过为你的应用创建延长的初始屏幕，延长显示初始屏幕的时间。 此延长的屏幕将模仿你的应用启动时显示的初始屏幕，但是也可以进行自定义。
ms.assetid: CD3053EB-7F86-4D74-9C5A-950303791AE3
ms.date: 02/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bbc0c7c695a99354ee389118087773440b60fb20
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366285"
---
# <a name="display-a-splash-screen-for-more-time"></a>延长显示初始屏幕的时间

**重要的 Api**

-   [SplashScreen 类](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)
-   [Window.SizeChanged 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.sizechanged)
-   [Application.OnLaunched 方法](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched)

通过为你的应用创建延长的初始屏幕，延长显示初始屏幕的时间。 此延长的屏幕将模仿你的应用启动时显示的初始屏幕，但是也可以进行自定义。 无论你是要显示实时加载信息还是想要简单地为应用提供更多时间来准备其初始 UI，延长的初始屏幕允许你定义启动体验。

> [!NOTE]
> 短语"扩展的初始屏幕"本主题中引用的时间很长一段将停留在屏幕的初始屏幕。 它不表示从 [SplashScreen](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 类派生的子类。

通过遵循以下建议，确保延长的初始屏幕准确地模仿默认初始屏幕：

-   你的延长的初始屏幕页面应该使用 620 x 300 像素的图像，与应用清单中为初始屏幕指定的图像（你的应用的初始屏幕图像）一致。 在 Microsoft Visual Studio 2015 中，初始屏幕设置存储在**初始屏幕**一部分**视觉对象资产**你应用程序清单 （Package.appxmanifest 文件） 中的选项卡。
-   你的延长的初始屏幕使用的背景色应该与应用清单中为初始屏幕指定的背景色（你的应用的初始屏幕背景）一致。
-   你的代码应该使用 [SplashScreen](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 类将你的应用的初始屏幕图像放置在默认初始屏幕的相同屏幕坐标处。
-   通过使用 [SplashScreen](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 类在你的延长的初始屏幕上重新放置项目，你的代码应该响应窗口调整大小事件（例如，在旋转屏幕时或将应用移动到屏幕上靠近另一个应用的位置时）。

使用以下步骤创建一个延长的初始屏幕，该屏幕可有效地模仿默认初始屏幕。

## <a name="add-a-blank-page-item-to-your-existing-app"></a>将“空白页”  项目添加到你的现有应用


本主题假设你希望将延长的初始屏幕添加到使用 C#、Visual Basic 或 C++ 的现有通用 Windows 平台 (UWP) 应用。

-   在 Visual Studio 中打开您的应用程序。
-   从菜单栏按下或打开 **“项目”** ，然后单击 **“添加新项”** 。 将出现 **“添加新项”** 对话框。
-   从此对话框，向你的应用添加新的 **“空白页”** 。 本主题将延长的初始屏幕页命名为“ExtendedSplash”。

添加 **“空白页”** 项目将生成两个文件，一个用于标记 (ExtendedSplash.xaml)，另一个用于代码 (ExtendedSplash.xaml.cs)。

## <a name="essential-xaml-for-an-extended-splash-screen"></a>延长的初始屏幕的必需 XAML


请按照以下步骤将图像和进度控件添加到你的延长的初始屏幕。

在你的 ExtendedSplash.xaml 文件中，执行以下操作：

-   更改[背景](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.backgroundproperty)属性的默认[网格](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)元素以匹配您应用程序清单中设置应用程序的初始屏幕的背景色 (在**视觉对象资产**Package.appxmanifest 文件的部分)。 默认初始屏幕颜色为浅灰色 (十六进制值\#464646)。 请注意，在你创建新的 **“空白页”** 时，将默认提供此 **Grid** 元素。 你不必使用 **Grid**；它只是为构建延长的初始屏幕提供了一个方便。
-   将 [Canvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) 元素添加到 [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)。 你将使用此 **Canvas** 来放置你的延长的初始屏幕图像。
-   将 [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 元素添加到 [Canvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas)。 将你为默认初始屏幕选择的 600 x 320 像素图像用于延长的初始屏幕。
-   （可选）添加一个进度控件，以向用户显示正在加载的应用。 此主题添加了一个 [ProgressRing](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)，而非一个确定或不确定的 [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)。

下面的示例演示[网格](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)使用这些新增功能和更改。

```xaml
    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"></ProgressRing>
        </Canvas>
    </Grid>
```

> [!NOTE]
> 此示例中设置的宽度[ProgressRing](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)到 20 像素。 你可以将其宽度手动设置为对你的应用有效的值，但是，该控件无法以小于 20 像素的宽度呈现。

## <a name="essential-code-for-an-extended-splash-screen-class"></a>延长的初始屏幕类的必需代码


你的延长的初始屏幕需要在每次窗口大小（仅限 Windows）或方向发生更改时作出响应。 你使用的图像的位置必须进行更新，这样你的延长的初始屏幕将看起来很好，而不管窗口大小如何改变。

使用以下步骤定义方法以正确地显示你的延长的初始屏幕。

1.  **添加所需的命名空间**

    将需要添加到以下命名空间**ExtendedSplash.xaml.cs**访问[初始屏幕](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)类[Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect)结构和[Window.SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.sizechanged)事件。

    ```cs
    using Windows.ApplicationModel.Activation;
    using Windows.Foundation;
    using Windows.UI.Core;
    ```

2.  **创建一个分部类，并声明类变量**

    将以下代码包括在 ExtendedSplash.xaml.cs 中，以创建一个部分类来代表延长的初始屏幕。

    ```cs
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

       // Define methods and constructor
    }
    ```

    这些类变量由多个方法使用。 `splashImageRect` 变量存储系统为应用显示初始屏幕图像所在位置的坐标。 `splash` 变量存储一个 [SplashScreen](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) 对象，而 `dismissed` 变量跟踪是否已解除系统所显示的初始屏幕。

3.  **定义您正确放置图像的类的构造函数**

    以下代码为延长的初始屏幕类定义了一个构造函数，用于侦听窗口调整大小事件、将图像和（可选）进度控件放置在延长的初始屏幕上、为导航创建一个框架，以及调用异步方法来还原保存的会话状态。

    ```cs
    public ExtendedSplash(SplashScreen splashscreen, bool loadState)
    {
        InitializeComponent();

        // Listen for window resize events to reposition the extended splash screen image accordingly.
        // This ensures that the extended splash screen formats properly in response to window resizing.
        Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

        splash = splashscreen;
        if (splash != null)
        {
            // Register an event handler to be executed when the splash screen has been dismissed.
            splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

            // Retrieve the window coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            PositionRing();
        }

        // Create a Frame to act as the navigation context
        rootFrame = new Frame();            
    }
    ```

    确保在你的类构造函数中注册 [Window.SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.sizechanged) 处理程序（示例中为 `ExtendedSplash_OnResize`），以便你的应用在延长的初始屏幕中正确放置图像。

4.  **定义用于在您的扩展的初始屏幕中放置图像的类方法**

    此代码演示了如何使用 `splashImageRect` 类变量将图像放置在延长的初始屏幕页面上。

    ```cs
    void PositionImage()
    {
        extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
        extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
        extendedSplashImage.Height = splashImageRect.Height;
        extendedSplashImage.Width = splashImageRect.Width;
    }
    ```

5.  **（可选）定义用于在您的扩展的初始屏幕中定位进度控件的类方法**

    如果你选择将 [ProgressRing](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) 添加到你的延长的初始屏幕，请将它放置在与初始屏幕图像相对的位置。 将以下代码添加到 ExtendedSplash.xaml.cs 以将 **ProgressRing** 32 像素居中放置在图像的下方。

    ```cs
    void PositionRing()
    {
        splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
        splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
    }
    ```

6.  **在类中，定义为已忽略事件处理程序**

    在 ExtendedSplash.xaml.cs 中，通过将 `dismissed` 类变量设置为 true，在发生 [SplashScreen.Dismissed](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.splashscreen.dismissed) 事件时进行响应。 如果你的应用包含设置操作，请将它们添加到事件处理程序。

    ```cs
    // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
    void DismissedEventHandler(SplashScreen sender, object e)
    {
        dismissed = true;

        // Complete app setup operations here...
    }
    ```

    完成应用设置后，导航离开你的延长的初始屏幕。 以下代码定义了一个称为 `DismissExtendedSplash` 的方法，可用于导航至应用的 MainPage.xaml 文件中定义的 `MainPage`。

    ```cs
    async void DismissExtendedSplash()
      {
         await Windows.ApplicationModel.Core.CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal,() =>            {
              rootFrame = new Frame();
              rootFrame.Content = new MainPage(); Window.Current.Content = rootFrame;
            });
      }
      ```

7.  **在类中，定义 Window.SizeChanged 事件处理程序**

    在用户调整窗口大小时，准备你的延长的初始屏幕以重新放置其元素。 当 [Window.SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.sizechanged) 事件发生时，此代码将通过捕获新坐标和重新放置图像来进行响应。 如果你已将进度控件添加到延长的初始屏幕，同样将它重新放置在该事件处理程序内。

    ```cs
    void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
    {
        // Safely update the extended splash screen image coordinates. This function will be executed when a user resizes the window.
        if (splash != null)
        {
            // Update the coordinates of the splash screen image.
            splashImageRect = splash.ImageLocation;
            PositionImage();

            // If applicable, include a method for positioning a progress control.
            // PositionRing();
        }
    }
    ```

    > [!NOTE]
    > 在尝试获取之前图像位置，请确保类变量 (`splash`) 中包含有效[SplashScreen](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)对象，如示例所示。

     

8.  **（可选）添加一个类方法来还原已保存的会话状态**

    添加到代码[OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched)步骤 4 中的方法：[修改启动激活处理程序](#modify-the-launch-activation-handler)导致你的应用启动后显示的扩展的初始屏幕。 要合并到扩展的初始屏幕类中的应用程序启动相关的所有方法，你可以考虑将方法添加到 ExtendedSplash.xaml.cs 文件，以便还原应用程序的状态。

    ```cs
    void RestoreState(bool loadState)
    {
        if (loadState)
        {
             // code to load your app's state here
        }
    }
    ```

    在修改在 App.xaml.cs 中的启动激活处理程序时，您将设置`loadstate`为 true 的上一个[ApplicationExecutionState](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ApplicationExecutionState)您的应用程序的已**Terminated**。 如果出现这种情况，`RestoreState` 方法会将应用还原到其之前的状态。 有关应用启用、暂停和终止的概述，请参阅[应用生命周期](app-lifecycle.md)。

## <a name="modify-the-launch-activation-handler"></a>修改启动激活处理程序


启动应用时，系统将初始屏幕信息传递给应用的启动激活事件处理程序。 你可以使用该信息将图像正确放置在延长的初始屏幕页面上。 你可以从传递给你的应用的 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched) 处理程序的激活事件参数获取此初始屏幕信息（请参阅以下代码中的 `args` 变量）。

如果你具有尚未重写[OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched)处理程序为你的应用，请参阅[应用程序生命周期](app-lifecycle.md)若要了解如何处理激活事件。

在 App.xaml.cs 中，添加以下代码以创建和显示延长的初始屏幕。

```cs
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    if (args.PreviousExecutionState != ApplicationExecutionState.Running)
    {
        bool loadState = (args.PreviousExecutionState == ApplicationExecutionState.Terminated);
        ExtendedSplash extendedSplash = new ExtendedSplash(args.SplashScreen, loadState);
        Window.Current.Content = extendedSplash;
    }

    Window.Current.Activate();
}
```

## <a name="complete-code"></a>完成代码

下面的代码略有不同于上一步骤中所示的代码段。
-   ExtendedSplash.xaml 包括一个 `DismissSplash` 按钮。 单击此按钮时，事件处理程序 `DismissSplashButton_Click` 将调用 `DismissExtendedSplash` 方法。 在你的应用中，在应用完成资源加载或初始化其 UI 后调用 `DismissExtendedSplash`。
-   此应用还会使用 UWP 应用项目模板，该模板使用 [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) 导航。 因此，在 App.xaml.cs 中，启动激活处理程序 ([OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched)) 将定义一个 `rootFrame` 并使用它来设置应用窗口的内容。

### <a name="extendedsplashxaml"></a>ExtendedSplash.xaml

此示例包括`DismissSplash`按钮因为它不具有应用程序资源加载。 在你的应用中，如果你的应用已完成资源加载或已准备好其初始 UI，将自动忽略延长的初始屏幕。

```xml
<Page
    x:Class="SplashScreenExample.ExtendedSplash"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SplashScreenExample"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="#464646">
        <Canvas>
            <Image x:Name="extendedSplashImage" Source="Assets/SplashScreen.png"/>
            <ProgressRing Name="splashProgressRing" IsActive="True" Width="20" HorizontalAlignment="Center"/>
        </Canvas>
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Bottom">
            <Button x:Name="DismissSplash" Content="Dismiss extended splash screen" HorizontalAlignment="Center" Click="DismissSplashButton_Click" />
        </StackPanel>
    </Grid>
</Page>
```

### <a name="extendedsplashxamlcs"></a>ExtendedSplash.xaml.cs

请注意，`DismissExtendedSplash`方法从的单击事件处理程序调用`DismissSplash`按钮。 在你的应用中，你将不需要 `DismissSplash` 按钮。 而是，在资源加载完成后以及在你想要导航到其主页面时调用 `DismissExtendedSplash`。

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

using Windows.ApplicationModel.Activation;
using SplashScreenExample.Common;
using Windows.UI.Core;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234238

namespace SplashScreenExample
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    partial class ExtendedSplash : Page
    {
        internal Rect splashImageRect; // Rect to store splash screen image coordinates.
        private SplashScreen splash; // Variable to hold the splash screen object.
        internal bool dismissed = false; // Variable to track splash screen dismissal status.
        internal Frame rootFrame;

        public ExtendedSplash(SplashScreen splashscreen, bool loadState)
        {
            InitializeComponent();

            // Listen for window resize events to reposition the extended splash screen image accordingly.
            // This is important to ensure that the extended splash screen is formatted properly in response to snapping, unsnapping, rotation, etc...
            Window.Current.SizeChanged += new WindowSizeChangedEventHandler(ExtendedSplash_OnResize);

            splash = splashscreen;

            if (splash != null)
            {
                // Register an event handler to be executed when the splash screen has been dismissed.
                splash.Dismissed += new TypedEventHandler<SplashScreen, Object>(DismissedEventHandler);

                // Retrieve the window coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();

                // Optional: Add a progress ring to your splash screen to show users that content is loading
                PositionRing();
            }

            // Create a Frame to act as the navigation context
            rootFrame = new Frame();

            // Restore the saved session state if necessary
            RestoreState(loadState);
        }

        void RestoreState(bool loadState)
        {
            if (loadState)
            {
                // TODO: write code to load state
            }
        }

        // Position the extended splash screen image in the same location as the system splash screen image.
        void PositionImage()
        {
            extendedSplashImage.SetValue(Canvas.LeftProperty, splashImageRect.X);
            extendedSplashImage.SetValue(Canvas.TopProperty, splashImageRect.Y);
            extendedSplashImage.Height = splashImageRect.Height;
            extendedSplashImage.Width = splashImageRect.Width;

        }

        void PositionRing()
        {
            splashProgressRing.SetValue(Canvas.LeftProperty, splashImageRect.X + (splashImageRect.Width*0.5) - (splashProgressRing.Width*0.5));
            splashProgressRing.SetValue(Canvas.TopProperty, (splashImageRect.Y + splashImageRect.Height + splashImageRect.Height*0.1));
        }

        void ExtendedSplash_OnResize(Object sender, WindowSizeChangedEventArgs e)
        {
            // Safely update the extended splash screen image coordinates. This function will be fired in response to snapping, unsnapping, rotation, etc...
            if (splash != null)
            {
                // Update the coordinates of the splash screen image.
                splashImageRect = splash.ImageLocation;
                PositionImage();
                PositionRing();
            }
        }

        // Include code to be executed when the system has transitioned from the splash screen to the extended splash screen (application's first view).
        void DismissedEventHandler(SplashScreen sender, object e)
        {
            dismissed = true;

            // Complete app setup operations here...
        }

        void DismissExtendedSplash()
        {
            // Navigate to mainpage
            rootFrame.Navigate(typeof(MainPage));
            // Place the frame in the current Window
            Window.Current.Content = rootFrame;
        }

        void DismissSplashButton_Click(object sender, RoutedEventArgs e)
        {
            DismissExtendedSplash();
        }
    }
}
```

### <a name="appxamlcs"></a>App.xaml.cs

此项目使用 UWP 应用创建**空白应用 (XAML)** Visual Studio 中的项目模板。 `OnNavigationFailed` 和 `OnSuspending` 事件处理程序均自动生成并且无需进行任何更改即可实现延长的初始屏幕。 此主题将仅修改 `OnLaunched`。

如果没有为你的应用使用的项目模板，请参阅步骤 4:[修改启动激活处理程序](#modify-the-launch-activation-handler)有关的已修改示例`OnLaunched`不使用[帧](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame)导航。

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Application template is documented at https://go.microsoft.com/fwlink/p/?LinkID=234227

namespace SplashScreenExample
{
    /// <summary>
    /// Provides application-specific behavior to supplement the default Application class.
    /// </summary>
    sealed partial class App : Application
    {
        /// <summary>
        /// Initializes the singleton application object.  This is the first line of authored code
        /// executed, and as such is the logical equivalent of main() or WinMain().
        /// </summary>
        public App()
        {
            Microsoft.ApplicationInsights.WindowsAppInitializer.InitializeAsync(
            Microsoft.ApplicationInsights.WindowsCollectors.Metadata |
            Microsoft.ApplicationInsights.WindowsCollectors.Session);
            this.InitializeComponent();
            this.Suspending += OnSuspending;
        }

        /// <summary>
        /// Invoked when the application is launched normally by the end user.  Other entry points
        /// will be used such as when the application is launched to open a specific file.
        /// </summary>
        /// <param name="e">Details about the launch request and process.</param>
        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
#if DEBUG
            if (System.Diagnostics.Debugger.IsAttached)
            {
                this.DebugSettings.EnableFrameRateCounter = true;
            }
#endif

            Frame rootFrame = Window.Current.Content as Frame;

            // Do not repeat app initialization when the Window already has content,
            // just ensure that the window is active
            if (rootFrame == null)
            {
                // Create a Frame to act as the navigation context and navigate to the first page
                rootFrame = new Frame();
                // Set the default language
                rootFrame.Language = Windows.Globalization.ApplicationLanguages.Languages[0];

                rootFrame.NavigationFailed += OnNavigationFailed;

                //  Display an extended splash screen if app was not previously running.
                if (e.PreviousExecutionState != ApplicationExecutionState.Running)
                {
                    bool loadState = (e.PreviousExecutionState == ApplicationExecutionState.Terminated);
                    ExtendedSplash extendedSplash = new ExtendedSplash(e.SplashScreen, loadState);
                    rootFrame.Content = extendedSplash;
                    Window.Current.Content = rootFrame;
                }
            }

            if (rootFrame.Content == null)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(typeof(MainPage), e.Arguments);
            }
            // Ensure the current window is active
            Window.Current.Activate();
        }

        /// <summary>
        /// Invoked when Navigation to a certain page fails
        /// </summary>
        /// <param name="sender">The Frame which failed navigation</param>
        /// <param name="e">Details about the navigation failure</param>
        void OnNavigationFailed(object sender, NavigationFailedEventArgs e)
        {
            throw new Exception("Failed to load Page " + e.SourcePageType.FullName);
        }

        /// <summary>
        /// Invoked when application execution is being suspended.  Application state is saved
        /// without knowing whether the application will be terminated or resumed with the contents
        /// of memory still intact.
        /// </summary>
        /// <param name="sender">The source of the suspend request.</param>
        /// <param name="e">Details about the suspend request.</param>
        private void OnSuspending(object sender, SuspendingEventArgs e)
        {
            var deferral = e.SuspendingOperation.GetDeferral();
            // TODO: Save applicaiton state and stop any background activity
            deferral.Complete();
        }
    }
}
```

## <a name="related-topics"></a>相关主题


* [应用生命周期](app-lifecycle.md)

**参考**

* [Windows.ApplicationModel.Activation 命名空间](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation)
* [Windows.ApplicationModel.Activation.SplashScreen 类](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)
* [Windows.ApplicationModel.Activation.SplashScreen.ImageLocation 属性](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.splashscreen.imagelocation)
* [Windows.ApplicationModel.Core.CoreApplicationView.Activated 事件](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)

 

 
