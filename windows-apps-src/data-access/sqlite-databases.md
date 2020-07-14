---
title: 在 UWP 应用中使用 SQLite 数据库
description: 在 UWP 应用中使用 SQLite 数据库。
ms.date: 06/26/2020
ms.topic: article
keywords: Windows 10, uwp, SQLite, 数据库
ms.localizationpriority: medium
ms.openlocfilehash: c8eb384be08addd75e77f37d6fb579a179e68581
ms.sourcegitcommit: 48e047a581fcfcc9a4084d65a78b89f2c01cf4f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448347"
---
# <a name="use-a-sqlite-database-in-a-uwp-app"></a>在 UWP 应用中使用 SQLite 数据库
可以使用 SQLite 在用户设备上的轻量级数据库中存储和检索数据。 本指南演示如何执行该操作。

## <a name="some-benefits-of-using-sqlite-for-local-storage"></a>使用 SQLite 进行本地存储一些好处

:heavy_check_mark:SQLite 具有轻量和独立的特点。 它是没有其他任何依赖项的代码库。 无需进行任何配置。

:heavy_check_mark:没有数据库服务器。 客户端和服务器在同一进程中运行。

:heavy_check_mark:SQLite 位于公共域中，因此你可以自由地使用它并将它与应用一起分配。

:heavy_check_mark:SQLite 可跨平台和体系结构工作。

可在[此处](https://sqlite.org/about.html)了解有关 SQLite 的详细信息。

## <a name="choose-an-abstraction-layer"></a>选择抽象层

我们建议使用由 Microsoft 构建的 Entity Framework Core 或开源 [SQLite 库](https://github.com/aspnet/Microsoft.Data.Sqlite/)。

### <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) 是一个对象关系映射程序，可用于使用特定于域的对象处理关系数据。 如果已使用此框架处理其他 .NET 应用中的数据，则可以将该代码迁移到 UWP 应用，它将处理对连接字符串的相应更改。

若要试用它，请参阅[开始使用通用 Windows 平台 (UWP) 上的带新数据库的 EF Core](https://docs.microsoft.com/ef/core/get-started/uwp/getting-started)。

### <a name="sqlite-library"></a>SQLite 库

[Microsoft.Data.Sqlite](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0) 库可在 [System.Data.Common](https://docs.microsoft.com/dotnet/api/system.data.common) 命名空间中实现接口。 Microsoft 将主动保留这些实现，它们提供了围绕低级别本机 SQLite API 的直观的包装器。

本指南的其余部分将帮助你使用此库。

## <a name="set-up-your-solution-to-use-the-microsoftdatasqlite-library"></a>将解决方案设置为使用 Microsoft.Data.SQlite 库

我们将从基本 UWP 项目入手，添加类库，然后安装合适的 Nuget 包。

添加到解决方案的类库的类型以及安装的特定程序包取决于应用面向的最低版本的 Windows SDK。 可以在 UWP 项目的属性页中找到该信息。

![Windows SDK 的最低版本](images/min-version.png)

根据 UWP 项目面向的最低版本的 Windows SDK，使用以下章节之一。

### <a name="the-minimum-version-of-your-project-does-not-target-the-fall-creators-update"></a>项目的最低版本没有将 Fall Creators Update 作为目标

如果使用的是 Visual Studio 2015，请单击“帮助”->“关于 Microsoft Visual Studio”。 然后，在已安装程序的列表中，确保你具有 NuGet 包管理器版本 **3.5** 或更高版本。 如果版本号较低，请安装[此处](https://www.nuget.org/downloads)提供的更高版本的 NuGet。 在该页面上，你将发现所有版本的 Nuget 都在 **Visual Studio 2015** 标题下方列出。

接下来，将类库添加到解决方案。 你不必使用类库来包含你的数据访问代码，但我们会使用一个我们的示例。 我们将库命名为 **DataAccessLibrary**，并将库中的类命名为 **DataAccess**。

![类库](images/class-library.png)

右键单击该解决方法，然后单击“管理解决方案的 NuGet 包”。

![管理 NuGet 包](images/manage-nuget.png)

如果使用的是 Visual Studio 2015，请选择“已安装”选项卡，并确保 **Microsoft.NETCore.UniversalWindowsPlatform** 程序包的版本号为 **5.2.2** 或更高。

![.NETCore 的版本](images/package-version.png)

如果不是，请将包更新到更新的版本。

选择“浏览”选项卡，然后搜索“Microsoft.Data.SQLite”程序包。 安装该程序包的版本 **1.1.1**（或更低）。

![SQLite 程序包](images/sqlite-package.png)

移动到本指南的[在 SQLite 数据库中添加和检索数据](#use-data)部分。

### <a name="the-minimum-version-of-your-project-targets-the-fall-creators-update"></a>你最低版本的项目已锁定 Fall Creators Update

将 UWP 项目的最低版本升级到 Fall Creators Update 有几个好处。

首先，你可以使用 .NET Standard 2.0 库而不是常规的类库。 这意味着你可以将数据访问代码与任何其他基于 .NET 的应用（如 WPF、Windows 窗体、Android、iOS 或 ASP.NET 应用）共享。

其次，应用不需将 SQLite 库打包。 相反，应用可以使用随 Windows 一起安装的 SQLite 版本。 这将带来几个方面的好处。

:heavy_check_mark:减小了应用程序的大小，因为你不必下载 SQLite 二进制文件然后将其打包为应用程序的一部分。

:heavy_check_mark:如果 SQLite 发布了针对 SQLite 中的 bug 和安全漏洞的重要修复程序，你就不必向用户推送你的应用的新版本。 Windows 版本的 SQLite 由 Microsoft 与 SQLite.org 协作维护。

:heavy_check_mark:应用加载时间可能更短，因为 SDK 版本的 SQLite 很有可能已被加载到内存中。

让我们开始向你的解决方案添加 .NET Standard 2.0 类库。 你不必使用类库来包含你的数据访问代码，但我们会使用一个我们的示例。 我们将库命名为 **DataAccessLibrary**，并将库中的类命名为 **DataAccess**。

![类库](images/dot-net-standard.png)

右键单击该解决方法，然后单击“管理解决方案的 NuGet 包”。

![管理 NuGet 包](images/manage-nuget-2.png)

> [!NOTE]
> 如果希望 .NET Standard 类库能够访问 UWP 应用的应用文件夹和图像资产，则需要将其标记为其属性中的 EmbeddedResource 和 CopyAlways  。

此时，你已经有一个选择。 你可以使用 Windows 附带的 SQLite 版本，如果你出于某种原因要使用特定版本的 SQLite，则可以在程序中包含 SQLite 库。

让我们开始演示如何使用 Windows 附带的 SQLite 版本。

#### <a name="to-use-the-version-of-sqlite-that-is-installed-with-windows"></a>使用随 Windows 一起安装的 SQLite 版本

选择“浏览”选项卡，搜索“Microsoft.Data.SQLite.core”程序包，然后安装它。

![SQLite Core 程序包](images/sqlite-core-package.png)

搜索“SQLitePCLRaw.bundle_winsqlite3”程序包，然后仅将它安装到应用程序中的 UWP 项目。

![SQLite PCL Raw 程序包](images/sqlite-raw-package.png)

#### <a name="to-include-sqlite-with-your-app"></a>将 SQLite 包含在你的应用中

你不必执行此操作。 但如果你出于某种原因要将特定版本的 SQLite 包含在你的应用中，请选择“浏览”选项卡，然后搜索“Microsoft.Data.SQLite”程序包。 安装该程序包的版本 **2.0**（或更低）。

![SQLite 程序包](images/sqlite-package-v2.png)

## <a name="add-and-retrieve-data-in-a-sqlite-database"></a>在 SQLite 数据库中添加和检索数据

我们将执行以下操作：

:one:准备数据访问类。

:two:初始化 SQLite 数据库。

:three:将数据插入到 SQLite 数据库。

:four:从 SQLite 数据库检索数据。

:five:添加基本用户界面。

### <a name="prepare-the-data-access-class"></a>准备数据访问类

从 UWP 项目中，添加对解决方案中的 **DataAccessLibrary** 项目的引用。

![数据访问类库](images/ref-class-library.png)

在 UWP 项目中，将以下 ``using`` 语句添加到 **App.xaml.cs** 和 **MainPage.xaml.cs** 文件。

```csharp
using DataAccessLibrary;
```

在 **DataAccessLibrary** 解决方案中打开 **DataAccess** 类并将其设为静态。

>[!NOTE]
>尽管我们的示例将数据访问代码放在静态类中，但这只是一个设计选择，并且是全凭自愿。

```csharp
namespace DataAccessLibrary
{
    public static class DataAccess
    {

    }
}

```

将以下 using 语句添加到此文件顶部。

```csharp
using Microsoft.Data.Sqlite;
using System.Collections.Generic;
```



### <a name="initialize-the-sqlite-database"></a>初始化 SQLite 数据库

向 **DataAccess** 类添加一个初始化 SQLite 数据库的方法。

```csharp
public async static void InitializeDatabase()
{ 
     await ApplicationData.Current.LocalFolder.CreateFileAsync("sqliteSample.db", CreationCollisionOption.OpenIfExists);
     string dbpath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "sqliteSample.db");
     using (SqliteConnection db =
        new SqliteConnection($"Filename={dbpath}"))
    {
        db.Open();

        String tableCommand = "CREATE TABLE IF NOT " +
            "EXISTS MyTable (Primary_Key INTEGER PRIMARY KEY, " +
            "Text_Entry NVARCHAR(2048) NULL)";

        SqliteCommand createTable = new SqliteCommand(tableCommand, db);

        createTable.ExecuteReader();
    }
}
```

此代码将创建 SQLite 数据库并将其存储在应用程序的本地数据存储区中。

在此示例中，我们将数据库命名为 ``sqlliteSample.db``，但你可以使用任何想要的名称，只要你在你实例化的所有 [SqliteConnection](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqliteconnection?view=msdata-sqlite-2.0.0) 对象中使用该名称。

在 UWP 项目的 **App.xaml.cs** 文件的构造函数中，调用 **DataAccess** 类的 ``InitializeDatabase`` 方法。

```csharp
public App()
{
    this.InitializeComponent();
    this.Suspending += OnSuspending;

    DataAccess.InitializeDatabase();

}
```



### <a name="insert-data-into-the-sqlite-database"></a>将数据插入到 SQLite 数据库

向 **DataAccess** 类添加一个将数据插入到 SQLite 数据库的方法。 此代码在查询中使用参数以阻止 SQL 注入攻击。

```csharp
public static void AddData(string inputText)
{
    string dbpath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "sqliteSample.db");
    using (SqliteConnection db =
      new SqliteConnection($"Filename={dbpath}"))
    {
        db.Open();

        SqliteCommand insertCommand = new SqliteCommand();
        insertCommand.Connection = db;

        // Use parameterized query to prevent SQL injection attacks
        insertCommand.CommandText = "INSERT INTO MyTable VALUES (NULL, @Entry);";
        insertCommand.Parameters.AddWithValue("@Entry", inputText);

        insertCommand.ExecuteReader();

        db.Close();
    }

}
```



### <a name="retrieve-data-from-the-sqlite-database"></a>从 SQLite 数据库检索数据

添加从 SQLite 数据库获取数据行的方法。

```csharp
public static List<String> GetData()
{
    List<String> entries = new List<string>();

   string dbpath = Path.Combine(ApplicationData.Current.LocalFolder.Path, "sqliteSample.db");
   using (SqliteConnection db =
      new SqliteConnection($"Filename={dbpath}"))
    {
        db.Open();

        SqliteCommand selectCommand = new SqliteCommand
            ("SELECT Text_Entry from MyTable", db);

        SqliteDataReader query = selectCommand.ExecuteReader();

        while (query.Read())
        {
            entries.Add(query.GetString(0));
        }

        db.Close();
    }

    return entries;
}
```

[Read](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.read?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_Read) 方法将向前浏览返回的数据的行。 如果有剩下的行，它将返回 **true**，否则返回 **false**。

[GetString](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getstring?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetString_System_Int32_) 方法返回字符串形式的指定列的值。 它将接受一个整数值，该值表示所需的数据的从零开始的列序号。 你可以使用类似的方法，例如 [GetDataTime](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getdatetime?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetDateTime_System_Int32_) 和 [GetBoolean](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite.sqlitedatareader.getboolean?view=msdata-sqlite-2.0.0#Microsoft_Data_Sqlite_SqliteDataReader_GetBoolean_System_Int32_)。 请根据列包含的数据的类型选择方法。

在此例子中，序号参数并不重要，因为我们选择了单个列中的所有条目。 但是，如果多个列是你的查询的一部分，请使用序号值获取你要从中拉取数据的列。

## <a name="add-a-basic-user-interface"></a>添加基本用户界面

在 UWP 项目的 **MainPage.xaml** 文件中，添加以下 XAML。

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Name="Input_Box"></TextBox>
        <Button Click="AddData">Add</Button>
        <ListView Name="Output">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextBlock Text="{Binding}"/>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackPanel>
</Grid>
```

此基本用户界面为用户提供了 ``TextBox``，可用于键入我们将添加到 SQLite 数据库的字符串。 我们要将此 UI 中的 ``Button`` 连接到事件处理程序，后者将从 SQLite 数据库检索数据然后在 ``ListView`` 中显示数据。

在 **MainPage.xaml.cs** 文件中，添加以下处理程序。 这是我们关联到 UI 中的 ``Button`` 的 ``Click``事件的方法。

```csharp
private void AddData(object sender, RoutedEventArgs e)
{
    DataAccess.AddData(Input_Box.Text);

    Output.ItemsSource = DataAccess.GetData();
}
```

完成了。 探索 [Microsoft.Data.Sqlite](https://docs.microsoft.com/dotnet/api/microsoft.data.sqlite?view=msdata-sqlite-2.0.0) 以了解 SQLite 数据库的其他功能。 查看下面的链接，了解在 UWP 应用中使用数据的其他方法。

## <a name="next-steps"></a>后续步骤

**将应用直接连接到 SQL Server 数据库**

请参阅[在 UWP 应用中使用 SQL Server 数据库](sql-server-databases.md)。

**在跨不同平台的不同应用之间共享代码**

参阅[在桌面应用和 UWP 应用之间共享代码](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate)。

**使用 Azure SQL 后端添加大纲/细节页面**

参阅[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)。
