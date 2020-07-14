---
title: 在 UWP 应用中使用 SQL Server 数据库
description: 在 UWP 应用中使用 SQL Server 数据库。
ms.date: 06/26/2020
ms.topic: article
keywords: Windows 10, uwp, SQL Server, 数据库
ms.localizationpriority: medium
ms.openlocfilehash: d5c5c0f107a9e9a2adb0587b544a2c74b8321886
ms.sourcegitcommit: 48e047a581fcfcc9a4084d65a78b89f2c01cf4f3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/26/2020
ms.locfileid: "85448337"
---
# <a name="use-a-sql-server-database-in-a-uwp-app"></a>在 UWP 应用中使用 SQL Server 数据库
通过使用 [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) 命名空间中的类，你的应用可以直接连接到 SQL Server 数据库然后存储和检索数据。

在本指南中，我们将向你介绍一种操作方法。 如果你将 [Northwind](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/linq/downloading-sample-databases) 示例数据库安装到 SQL Server 实例上，然后使用这些代码段，你最终将获得显示了来自 Northwind 示例数据库的产品的基本 UI。

![Northwind 产品](images/products-northwind.png)

本指南中显示的代码段基于此更[完整的示例](https://github.com/StefanWickDev/IgniteDemos/tree/master/NorthwindDemo)。

## <a name="first-set-up-your-solution"></a>首先，设置你的解决方案

要将你的应用直接连接到 SQL Server 数据库，请确保你的项目的最低版本以秋季创意者更新为目标。  你可以在 UWP 项目的属性页中找到该信息。

![Windows SDK 的最低版本](images/min-version-fall-creators.png)

在清单设计器中打开你的 UWP 项目的 Package.appxmanifest 文件。

在“功能”选项卡中，如果要使用 Windows 身份验证对 SQL Server 进行身份验证，则选中“企业身份验证”复选框。

![企业身份验证功能](images/enterprise-authentication.png)

> [!IMPORTANT]
> 无论你是否使用 Windows 身份验证，都需要选择“Internet (客户端和服务器)”、“Internet (客户端)”和“私有网络(客户端和服务器)”  。

## <a name="add-and-retrieve-data-in-a-sql-server-database"></a>在 SQL Server 数据库中添加和检索数据

在本节中，我们将执行以下操作：

:one:添加连接字符串。

:two:创建用于保存产品数据的类。

:three:从 SQL Server 数据库检索产品。

:four:添加基本用户界面。

:five:使用产品填充 UI。

>[!NOTE]
> 本节介绍了一种组织你的数据访问代码的方法。 这不仅仅是为了提供如何使用 [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) 在 SQL Server 数据库中存储和检索数据的示例。 你可以采用对你的应用程序设计最有意义的任何方式组织你的代码。

### <a name="add-a-connection-string"></a>添加连接字符串

在 App.xaml.cs 文件中，向 ``App`` 类添加一个属性，该属性为你的解决方案中的其他类提供了对连接字符串的访问权限。

我们的连接字符串指向 SQL Server Express 实例中的 Northwind 数据库。

```csharp
sealed partial class App : Application
{
    // Connection string for using Windows Authentication.
    private string connectionString =
        @"Data Source=YourServerName\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

    // This is an example connection string for using SQL Server Authentication.
    // private string connectionString =
    //     @"Data Source=YourServerName\YourInstanceName;Initial Catalog=DatabaseName; User Id=XXXXX; Password=XXXXX";

    public string ConnectionString { get => connectionString; set => connectionString = value; }

    ...
}
```

### <a name="create-a-class-to-hold-product-data"></a>创建用于保存产品数据的类

我们将创建一个实现 [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged) 事件的类，以便将 XAML UI 中的属性绑定到此类中的属性。

```csharp
public class Product : INotifyPropertyChanged
{
    public int ProductID { get; set; }
    public string ProductCode { get { return ProductID.ToString(); } }
    public string ProductName { get; set; }
    public string QuantityPerUnit { get; set; }
    public decimal UnitPrice { get; set; }
    public string UnitPriceString { get { return UnitPrice.ToString("######.00"); } }
    public int UnitsInStock { get; set; }
    public string UnitsInStockString { get { return UnitsInStock.ToString("#####0"); } }
    public int CategoryId { get; set; }

    public event PropertyChangedEventHandler PropertyChanged;
    private void NotifyPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

}
```

### <a name="retrieve-products-from-the-sql-server-database"></a>从 SQL Server 数据库检索产品

创建一个从 Northwind 示例数据库获取产品的方法，然后将这些产品作为 ``Product`` 实例的 [ObservableCollection](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1) 集合返回。

```csharp
public ObservableCollection<Product> GetProducts(string connectionString)
{
    const string GetProductsQuery = "select ProductID, ProductName, QuantityPerUnit," +
       " UnitPrice, UnitsInStock, Products.CategoryID " +
       " from Products inner join Categories on Products.CategoryID = Categories.CategoryID " +
       " where Discontinued = 0";

    var products = new ObservableCollection<Product>();
    try
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            if (conn.State == System.Data.ConnectionState.Open)
            {
                using (SqlCommand cmd = conn.CreateCommand())
                {
                    cmd.CommandText = GetProductsQuery;
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            var product = new Product();
                            product.ProductID = reader.GetInt32(0);
                            product.ProductName = reader.GetString(1);
                            product.QuantityPerUnit = reader.GetString(2);
                            product.UnitPrice = reader.GetDecimal(3);
                            product.UnitsInStock = reader.GetInt16(4);
                            product.CategoryId = reader.GetInt32(5);
                            products.Add(product);
                        }
                    }
                }
            }
        }
        return products;
    }
    catch (Exception eSql)
    {
        Debug.WriteLine("Exception: " + eSql.Message);
    }
    return null;
}
```

### <a name="add-a-basic-user-interface"></a>添加基本用户界面

 将以下 XAML 添加到 UWP 项目的 MainPage.xaml 文件。

 此 XAML 将创建一个 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview)（用来显示你在上一个代码段中返回的每个产品），并会将 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 中的每个行的属性绑定到我们在 ``Product`` 类中定义的属性。

```xml
<Grid Background="{ThemeResource SystemControlAcrylicWindowBrush}">
    <RelativePanel>
        <ListView Name="InventoryList"
                  SelectionMode="Single"
                  ScrollViewer.VerticalScrollBarVisibility="Auto"
                  ScrollViewer.IsVerticalRailEnabled="True"
                  ScrollViewer.VerticalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollBarVisibility="Auto"
                  ScrollViewer.IsHorizontalRailEnabled="True"
                  Margin="20">
            <ListView.HeaderTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal"  >
                        <TextBlock Text="ID" Margin="8,0" Width="50" Foreground="DarkRed" />
                        <TextBlock Text="Product description" Width="300" Foreground="DarkRed" />
                        <TextBlock Text="Packaging" Width="200" Foreground="DarkRed" />
                        <TextBlock Text="Price" Width="80" Foreground="DarkRed" />
                        <TextBlock Text="In stock" Width="80" Foreground="DarkRed" />
                    </StackPanel>
                </DataTemplate>
            </ListView.HeaderTemplate>
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:Product">
                    <StackPanel Orientation="Horizontal" >
                        <TextBlock Name="ItemId"
                                    Text="{x:Bind ProductCode}"
                                    Width="50" />
                        <TextBlock Name="ItemName"
                                    Text="{x:Bind ProductName}"
                                    Width="300" />
                        <TextBlock Text="{x:Bind QuantityPerUnit}"
                                   Width="200" />
                        <TextBlock Text="{x:Bind UnitPriceString}"
                                   Width="80" />
                        <TextBlock Text="{x:Bind UnitsInStockString}"
                                   Width="80" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RelativePanel>
</Grid>
```

### <a name="show-products-in-the-listview"></a>在 ListView 中显示产品

打开 MainPage.xaml.cs 文件，并将代码添加到 ``MainPage`` 类的构造函数，该类可将 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 的 ItemSource 属性设置为 ``Product`` 实例的 [ObservableCollection](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1)。

```csharp
public MainPage()
{
    this.InitializeComponent();
    InventoryList.ItemsSource = GetProducts((App.Current as App).ConnectionString);
}
```

启动项目并看到来自 Northwind 示例数据库的产品出现在 UI 中。

![Northwind 产品](images/products-northwind.png)

探索 [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) 命名空间以了解 SQL Server 数据库中的数据的其他作用。

## <a name="trouble-connecting-to-your-database"></a>连接数据库时遇到问题？

在大多数情况下，需要更改 SQL Server 配置的某些方面。 如果能够从其他类型的桌面应用程序（例如 Windows 窗体或 WPF 应用程序）连接到数据库，请确保已为 SQL Server 启用 TCP/IP。 可以在“计算机管理”控制台中执行该操作。

![筿福恨瞶](images/computer-management.png)

然后，确保 SQL Server Browser 服务正在运行。

![SQL Server Browser 服务](images/sql-browser-service.png)

## <a name="next-steps"></a>后续步骤

**使用轻量级数据库在用户设备上存储数据**

参阅[在 UWP 应用中使用 SQLite 数据库](sqlite-databases.md)。

**在跨不同平台的不同应用之间共享代码**

参阅[在桌面应用和 UWP 应用之间共享代码](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate)。

**使用 Azure SQL 后端添加大纲/细节页面**

参阅[客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)。
