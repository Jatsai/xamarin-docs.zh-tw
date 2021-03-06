---
title: Hello, watchOS –逐步解說
description: 本檔提供使用 Xamarin 建立簡單 watchOS 應用程式的逐步解說。 其中說明如何在 Visual Studio 和 Visual Studio for Mac 中工作、使用分鏡腳本, 以及在程式碼中回應事件。
ms.prod: xamarin
ms.assetid: AD1DA488-51AB-420A-A0B7-3AE69A964A40
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 12/14/2016
ms.openlocfilehash: 26c418355c83da807d6dfa514e58f9bf1675759f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528422"
---
# <a name="hello-watchos--walkthrough"></a>Hello, watchOS –逐步解說

在您依照[安裝和安裝](~/ios/watchos/get-started/installation.md)中的步驟建立解決方案之後, 您將會有3個專案:

- IOS 父系應用程式, 用於安裝程式或其他裝置上的系統管理工作。 (使用其他類型的 iOS 延伸模組, 這通常稱為「容器」應用程式)。使用 Watch 應用程式, 使用者就可以開始執行 Watch 應用程式, 而不需要執行父系應用程式;
- 監看式延伸模組, 其中包含監看式應用程式的程式碼;和
- 監看式應用程式, 它會保存顯示在監看式上的分鏡腳本和影像資源。

請檢查您的[參考是否正確](~/ios/watchos/get-started/project-references.md): 父應用程式是否有延伸模組的參考, 以及此延伸模組是否有 Watch 應用程式的參考。

確認您的套件組合識別碼，請遵循\*.watchkitextension \*.watchkitapp 慣例，而且您的延伸模組 Info.plist 檔案具有它已 **WKApp 套件組合識別碼** 值設定為套件組合識別碼您的監看式應用程式。

您應該能夠立即執行監看式應用程式, 但因為監看式應用程式中的分鏡腳本檔案是空白的, 所以您將無法分辨。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](hello-watch-images/projectstructure.png "方案總管")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-projectstructure.png "方案總管")

-----

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
按兩下監看式應用程式中的介面, 以啟動 Xamarin iOS 設計工具 (如果您是在 Mac 上, 您也可以用滑鼠右鍵按一下並**開啟, > Xcode Interface Builder**)


1. 確定 [**工具箱**] 和 [**屬性**] 板可見,
1. 按一下以選取介面控制器,
1. 將介面控制器的識別碼和標題設定為**interfaceController**和**Hi Watch**,
1. 確認**類別**已設定為**InterfaceController**

    ![](hello-watch-images/interfacecontrollerattributes.png "將介面控制器的識別碼和標題設定為 interfaceController 和 Hi Watch")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

按兩下 [監看式] 應用程式中的 [腳本], 在 Visual Studio 中使用 Xamarin iOS 設計工具進行編輯:

1. 開啟 [屬性] 窗格;
1. 將類別變更為**InterfaceController**;
1. 按一下介面控制器;和
1. 將介面控制器的識別碼和標題設定為**interfaceController**和**Hi Watch**。

    ![](hello-watch-images/vs-interfacecontrollerattributes.png "將介面控制器的識別碼和標題設定為 interfaceController 和 Hi Watch")

-----


建立您的 UI:

1. 從 [**工具箱**] 面板中,
1. 將拖放 **按鈕** 並 **標籤** 光芒，和
1. 設定控制項的文字和屬性, 如下所示:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](hello-watch-images/draganddrop.png "設定控制項的文字和屬性, 如下所示")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](hello-watch-images/vs-draganddrop.png "設定控制項的文字和屬性, 如下所示")

-----

1. 在**Properties** pad 中, 為每個控制項設定**名稱**。 在此範例中, 我們`myButton`使用`myLabel`了和。

1. 選取分鏡腳本上的按鈕, 並移至**Properties** Pad 的**事件**清單, 然後

1. 輸入`OnButtonPress`並按**enter**, 以建立新的**動作**。
  動作會出現在清單中, 而且會在中C#自動建立部分方法。

![](hello-watch-images/buttonaction.png "新增至按鈕的 OnButtonPress 動作")

儲存腳本之後, **InterfaceController.designer.cs**會以控制項名稱和動作進行更新。 如果您在更新檔案之後開啟此檔案, 您可以看到與該`RegisterAttribute`控制器對應的方式, 以及 UI 控制項如何對應C#至以標記的實例`OutletAttribute`變數, 以及動作如何對應至標記了的`ActionAttribute`部分方法:

```csharp
// WARNING
//
// This file has been generated automatically by Visual Studio for Mac from the outlets and
// actions declared in your storyboard file.
// Manual changes to this file will not be maintained.
//
[Register ("InterfaceController")]
partial class InterfaceController
{
    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceButton myButton { get; set; }

    [Outlet]
    [GeneratedCode ("iOS Designer", "1.0")]
    WatchKit.WKInterfaceLabel myLabel { get; set; }

    [Action ("OnButtonPress:")]
    [GeneratedCode ("iOS Designer", "1.0")]
    partial void OnButtonPress (WatchKit.WKInterfaceButton sender);

    void ReleaseDesignerOutlets ()
    {
        if (myButton != null) {
            myButton.Dispose ();
            myButton = null;
        }
        if (myLabel != null) {
            myLabel.Dispose ();
            myLabel = null;
        }
    }
}
```

現在開啟**InterfaceController.cs** (*而非*InterfaceController.designer.cs), 並新增下列程式碼:

```csharp
int clickCount = 0;
partial void OnButtonPress (WatchKit.WKInterfaceButton sender)
{
  var msg = String.Format("Clicked {0} times", ++clickCount);
  myLabel.SetText(msg);
}
```

此程式碼應該是相當透明的: 每`clickCount`次`OnButtonPress`呼叫函式時, 執行個體變數都會遞增。 的文字`myLabel`會變更以反映此計數;`myLabel`當然, 是您在 XCode 中建立的其中一個輸出的名稱。 `partial`函式是與您所指定之動作名稱相關聯的函式的實作用。

如果它還不是啟始專案,

1. 以滑鼠右鍵按一下監看式擴充功能專案, 然後選擇 [**設定為啟始專案**]。

1. 將部署目標設定為與監看套件相容的模擬器映射 (例如 iPhone 6 iOS 8.2)、

1. 按下 [**調試**程式] 按鈕, 以觸發組建和模擬器啟動。

    [![](hello-watch-images/readytodebug-sml.png "Visual Studio 介面元素")](hello-watch-images/readytodebug.png#lightbox)

當模擬器啟動時, 按下按鈕以遞增標籤。
恭喜您, 您已經擁有一個監看式應用程式!

![](hello-watch-images/running.png "在模擬器中執行的應用程式")


## <a name="related-links"></a>相關連結

- [消費者入門 (範例)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-gettingstarted)
- [設定與安裝](~/ios/watchos/get-started/installation.md)
- [Watch 應用程式上的影片](https://blog.xamarin.com/your-first-watch-kit-app/)
