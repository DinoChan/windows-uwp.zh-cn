---
title: 初始化 Direct3D 11
description: 介绍如何将 Direct3D 9 初始化代码转换为 Direct3D 11，包括如何获取 Direct3D 设备和设备上下文的句柄以及如何使用 DXGI 设置交换链。
ms.assetid: 1bd5e8b7-fd9d-065c-9ff3-1a9b1c90da29
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, direct3d 11, 初始化, 移植, direct3d 9
ms.localizationpriority: medium
ms.openlocfilehash: c5a7f33ddbc6d70af5293b92165892c2098e452d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368034"
---
# <a name="initialize-direct3d-11"></a>初始化 Direct3D 11



**摘要**

-   第 1 部分：初始化 Direct3D 11
-   [第 2 部分：转换呈现框架](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   [第 3 部分：端口游戏循环](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


介绍如何将 Direct3D 9 初始化代码转换为 Direct3D 11，包括如何获取 Direct3D 设备和设备上下文的句柄以及如何使用 DXGI 设置交换链。 [将简单的 Direct3D 9 应用移植到 DirectX 11 和通用 Windows 平台 (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md) 操作实例的第 1 部分。

## <a name="initialize-the-direct3d-device"></a>初始化 Direct3D 设备


在 Direct3D 9 中，我们通过调用 [**IDirect3D9::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d9/nf-d3d9-idirect3d9-createdevice) 创建了 Direct3D 设备的一个句柄。 我们先获取指向 [**IDirect3D9 interface**](https://docs.microsoft.com/windows/desktop/api/d3d9helper/nn-d3d9helper-idirect3d9) 的指针，然后指定了控制 Direct3D 设备和交换链配置的很多参数。 执行该操作之前，我们调用了 [**GetDeviceCaps**](https://docs.microsoft.com/windows/desktop/api/wingdi/nf-wingdi-getdevicecaps) 以确保我们没有要求设备执行无法完成的操作。

Direct3D 9

```cpp
UINT32 AdapterOrdinal = 0;
D3DDEVTYPE DeviceType = D3DDEVTYPE_HAL;
D3DCAPS9 caps;
m_pD3D->GetDeviceCaps(AdapterOrdinal, DeviceType, &caps); // caps bits

D3DPRESENT_PARAMETERS params;
ZeroMemory(&params, sizeof(D3DPRESENT_PARAMETERS));

// Swap chain parameters:
params.hDeviceWindow = m_hWnd;
params.AutoDepthStencilFormat = D3DFMT_D24X8;
params.BackBufferFormat = D3DFMT_X8R8G8B8;
params.MultiSampleQuality = D3DMULTISAMPLE_NONE;
params.MultiSampleType = D3DMULTISAMPLE_NONE;
params.SwapEffect = D3DSWAPEFFECT_DISCARD;
params.Windowed = true;
params.PresentationInterval = 0;
params.BackBufferCount = 2;
params.BackBufferWidth = 0;
params.BackBufferHeight = 0;
params.EnableAutoDepthStencil = true;
params.Flags = 2;

m_pD3D->CreateDevice(
    0,
    D3DDEVTYPE_HAL,
    m_hWnd,
    64,
    &params,
    &m_pd3dDevice
    );
```

在 Direct3D 11 中，设备上下文和图形基础结构被认为是与设备本身分离的。 初始化分为多个步骤。

首先，我们创建设备。 我们会得到设备所支持的功能级别列表 - 该列表将告知我们想知道的有关 GPU 的大部分内容。 而且，我们无需创建接口即可访问 Direct3D。 我们使用 [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 核心 API。 这会为我们提供设备句柄以及设备的即时上下文。 设备上下文用来设置管道状态以及生成呈现命令。

创建 Direct3D 11 设备和上下文之后，我们可以利用 COM 指针功能获取最新版本的接口，该指针功能还包括其他功能，因此我们总是推荐使用它。

> **请注意**   D3D\_功能\_级别\_9\_1 (对应于着色器模型 2.0） 是支持所需的 Microsoft Store 游戏的最低级别。 (您的游戏 ARM 包将失败认证，如果您不支持 9\_1。)如果您的游戏还包括着色器模型 3 功能呈现路径，则应包括 D3D\_功能\_级别\_9\_数组中的 3。

 

Direct3D 11

```cpp
// This flag adds support for surfaces with a different color channel 
// ordering than the API default. It is required for compatibility with
// Direct2D.
UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
// If the project is in a debug build, enable debugging via SDK Layers.
creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

// This example only uses feature level 9.1.
D3D_FEATURE_LEVEL featureLevels[] = 
{
    D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> context;
D3D11CreateDevice(
    nullptr, // Specify nullptr to use the default adapter.
    D3D_DRIVER_TYPE_HARDWARE,
    nullptr,
    creationFlags,
    featureLevels,
    ARRAYSIZE(featureLevels),
    D3D11_SDK_VERSION, // UWP apps must set this to D3D11_SDK_VERSION.
    &device, // Returns the Direct3D device created.
    nullptr,
    &context // Returns the device immediate context.
    );

// Store pointers to the Direct3D 11.2 API device and immediate context.
device.As(&m_d3dDevice);

context.As(&m_d3dContext);
```

## <a name="create-a-swap-chain"></a>创建交换链


Direct3D 11 包含一个称为 DirectX 图形基础结构 (DXGI) 的设备 API。 使用 DXGI 接口，我们可以控制交换链的配置方式，并可设置共享设备。 初始化 Direct3D 时，我们会在这一步骤中使用 DXGI 来创建交换链。 创建设备之后，我们便可以跟随一个接口链回到 DXGI 适配器。

Direct3D 设备为 DXGI 实现一个 COM 接口。 首先，我们需要获取该接口并使用该接口来请求托管设备的 DXGI 适配器。 然后，我们使用 DXGI 适配器来创建 DXGI 工厂。

> **请注意**  这些是 COM 接口，因此第一个响应可能是使用[ **QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))。 应该使用 [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class) 智能指针。 然后，只需调用 [**As()** ](https://docs.microsoft.com/previous-versions/br230426(v=vs.140)) 方法，从而提供正确接口类型的一个空 COM 指针。

 

**Direct3D 11**

```cpp
ComPtr<IDXGIDevice2> dxgiDevice;
m_d3dDevice.As(&dxgiDevice);

// Then, the adapter hosting the device;
ComPtr<IDXGIAdapter> dxgiAdapter;
dxgiDevice->GetAdapter(&dxgiAdapter);

// Then, the factory that created the adapter interface:
ComPtr<IDXGIFactory2> dxgiFactory;
dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2),
    &dxgiFactory
    );
```

既然我们拥有了 DXGI 工厂，那么我们便可以使用它来创建交换链。 下面我们定义交换链参数。 我们需要指定的图面上的格式;我们将选择[ **DXGI\_格式\_B8G8R8A8\_UNORM** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)由于与 Direct2D 兼容。 我们将关闭显示缩放、多重采样以及立体呈现，因为该示例中没有使用这些功能。 由于我们直接在 CoreWindow 中运行，因此可以将宽度和高度设置为 0 并自动获取全屏值。

> **请注意**  始终集*SDKVersion* D3D11 参数\_SDK\_适用于 UWP 应用的版本。

 

**Direct3D 11**

```cpp
ComPtr<IDXGISwapChain1> swapChain;
dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr,
    &swapChain
    );
swapChain.As(&m_swapChain);
```

若要确保我们不通常不是屏幕实际上可以显示呈现，我们将帧延迟设置为 1 和使用[ **DXGI\_交换\_效果\_FLIP\_SEQUENTIAL**](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect). 这不仅能够省电而且还是应用商店的认证要求；我们将在本操作实例的第 2 部分中了解有关呈现到屏幕的更多信息。

> **请注意**  可以使用多线程处理 (例如， [ **ThreadPool** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading)工作项) 时能够继续执行其他工作呈现线程被阻止。

 

**Direct3D 11**

```cpp
dxgiDevice->SetMaximumFrameLatency(1);
```

现在，我们可以设置用于呈现的后台缓冲区。

## <a name="configure-the-back-buffer-as-a-render-target"></a>将后台缓冲区配置为呈现目标


首先，我们必须获取后台缓冲区的句柄。 （请注意后台缓冲区归 DXGI 交换链，而它已在 DirectX 9 归 Direct3D 设备。）然后我们让 Direct3D 设备创建呈现器目标作为呈现器目标使用它*视图*使用后台缓冲区。

**Direct3D 11**

```cpp
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChain->GetBuffer(
    0,
    __uuidof(ID3D11Texture2D),
    &backBuffer
    );

// Create a render target view on the back buffer.
m_d3dDevice->CreateRenderTargetView(
    backBuffer.Get(),
    nullptr,
    &m_renderTargetView
    );
```

现在，开始使用设备上下文了。 我们通过使用设备上下文接口告知 Direct3D 使用我们新创建的呈现目标视图。 我们将检索后台缓冲区的宽度和高度，以便我们可以将整个窗口设置为我们的目标视区。 请注意，后台缓冲区与交换链相连接，因此如果窗口大小发生改变（例如，用户将游戏窗口拖动到另一个监视器），那么后台缓冲区将需要调整大小，并且需要重新进行某些设置。

**Direct3D 11**

```cpp
D3D11_TEXTURE2D_DESC backBufferDesc = {0};
backBuffer->GetDesc(&backBufferDesc);

CD3D11_VIEWPORT viewport(
    0.0f,
    0.0f,
    static_cast<float>(backBufferDesc.Width),
    static_cast<float>(backBufferDesc.Height)
    );

m_d3dContext->RSSetViewports(1, &viewport);
```

现在，我们拥有了设备句柄和全屏呈现目标，接下来我们便可以加载和绘制几何图形了。 继续[第 2 部分：呈现](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)。

 

 




