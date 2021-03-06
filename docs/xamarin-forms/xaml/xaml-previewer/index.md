---
title: 適用于 Xamarin 的 XAML 預覽程式
description: 本文說明如何使用 XAML 預覽程式來查看您的 Xamarin。表單版面配置會在您輸入時呈現。 XAML 預覽程式適用于 Mac Visual Studio 2019 和 Visual Studio 2019。
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
ms.openlocfilehash: b287d523101bb8ca7faca8ea95ee898ccf9c0bb1
ms.sourcegitcommit: e02b725e48af867eb2c53ac9e17805f778fbbc8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2019
ms.locfileid: "68757254"
---
# <a name="xaml-previewer-for-xamarinforms"></a>適用于 Xamarin 的 XAML 預覽程式

_查看您的 Xamarin。表單版面配置會在您輸入時呈現_

## <a name="overview"></a>總覽

XAML 預覽程式會顯示您的 Xamarin. 表單 XAML 頁面在 iOS 和 Android 上的外觀。 當您對 XAML 進行變更時, 您會看到它們緊接在您的程式碼旁預覽。 XAML 預覽器適用于 Visual Studio 和 Visual Studio for Mac。

## <a name="getting-started"></a>使用者入門

::: zone pivot="windows"

### <a name="visual-studio-2019"></a>Visual Studio 2019

您可以按一下 [分割視圖] 窗格上的箭號來開啟 XAML 預覽程式。 如果您想要變更預設的分割視圖行為, 請使用 [**工具] > 選項 > [Xamarin > 表單預覽**器] 對話方塊。 在此對話方塊中, 您可以選取 [預設檔] 視圖和 [分割方向]。

[![Visual Studio 中的 [Xamarin] 表單預覽選項](xaml-previewer-images/xamlp-options-vs-sm.png "Visual Studio 中的 [Xamarin] 表單預覽選項")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

當您開啟 XAML 檔案時, 編輯器會根據 **工具 > 選項 > Xamarin > 表單預覽**器 對話方塊中選取的設定, 開啟預覽器的完整大小或旁邊。 不過, 您可以在 [編輯器] 視窗中變更每個檔案的分割。

#### <a name="xaml-preview-controls"></a>XAML 預覽控制項

在 [分割視圖] 窗格上選取這些按鈕, 選擇您要查看程式碼、XAML 預覽器或兩者。 中間按鈕會交換預覽程式和您的程式碼所在的位置:

[![在 Visual Studio 的設計、來源和分割視圖之間切換的 [Xamarin] 表單預覽控制項](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "在 Visual Studio 的設計、來源和分割視圖之間切換的 [Xamarin] 表單預覽控制項")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

您可以變更畫面是以垂直或水準方式分割, 或是全部折迭一個窗格:

[[ ![Xamarin] Visual Studio 中的 [表單預覽] 窗格方向控制項][ (xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Xamarin] Visual Studio 中的 [表單預覽] 窗格方向控制項")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### <a name="visual-studio-for-mac"></a>Visual Studio for Mac

當您開啟 XAML 頁面時, 編輯器中會顯示 [**預覽**] 按鈕。 按下任何 XAML 文件視窗左下方的 [**預覽**] 或 [**分割**] 按鈕, 以顯示或隱藏預覽程式:

[![[Xamarin] 已啟用 [預覽] 或 [分割] 按鈕的表單預覽](xaml-previewer-images/xamlp-list-sml.png)](xaml-previewer-images/xamlp-list.png#lightbox)

> [!NOTE]
> 在較舊版本的 Visual Studio for Mac 中, [**預覽**] 按鈕位於視窗的右上方。

::: zone-end

## <a name="xaml-previewer-options"></a>XAML 預覽器選項

[預覽] 窗格頂端的選項如下:

* **Android** –顯示 android 版本的螢幕
* **ios** –顯示 ios 版本的畫面 (*注意:如果您是在 Windows 上使用 Visual Studio, 則必須[與 Mac 配對](~/ios/get-started/installation/windows/connecting-to-mac/index.md), 才能使用此模式*)
* **裝置**-Android 或 iOS 裝置的下拉式清單, 包括解析度和螢幕大小
* 直向 **(圖示)** -使用預覽的直向方向
* **橫向 (圖示)** –使用橫向預覽的方向

## <a name="detect-design-mode"></a>偵測設計模式

靜態[`DesignMode.IsDesignModeEnabled`](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled)屬性會告訴您應用程式是否正在預覽器中執行。 使用它時, 您可以指定只在應用程式在預覽器中執行或未執行時才會執行的程式碼:

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}

if (!DesignMode.IsDesignModeEnabled)
{
  // Don't run in the Previewer  
}
```

如果您在頁面的函式中初始化無法在設計階段執行的程式庫, 這個屬性就很有用。

## <a name="troubleshooting"></a>疑難排解

如果預覽器無法運作, 請檢查下列問題和[Xamarin 論壇](https://forums.xamarin.com/categories/xamarin-forms)。

### <a name="xaml-previewer-isnt-showing-or-shows-an-error"></a>XAML 預覽程式未顯示或顯示錯誤

* 預覽程式可能需要一些時間才會啟動, 您會看到「正在初始化轉譯」, 直到準備就緒為止。
* 請嘗試關閉並重新開啟 XAML 檔案。
* 請確定您`App`的類別具有無參數的函式。
* 檢查您的 Xamarin. 表單版本-其至少必須為 Xamarin。表單3.6。 您可以透過 NuGet 更新為最新的 Xamarin. Forms 版本。
* 檢查 JDK 安裝-預覽 Android 至少需要[JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)。
* 嘗試在頁面的C#程式碼後置中`if (!DesignMode.IsDesignModeEnabled)`包裝任何初始化的類別。

### <a name="custom-controls-arent-rendering"></a>自訂控制項不會呈現

嘗試建立您的專案。 預覽程式會顯示控制項的基類 (如果無法轉譯控制項), 或是控制項的建立者退出宣告設計階段轉譯。 如需詳細資訊, 請參閱[在 XAML 預覽程式中轉譯自訂控制項](render-custom-controls.md)。
