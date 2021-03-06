---
title: Xamarin 中的應用程式圖示
description: '本檔說明如何使用 Xamarin 中的各種應用程式圖示。 iOS: 應用程式圖示本身、焦點圖示、設定圖示和 iTunes 插圖。'
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/22/2017
ms.openlocfilehash: 0f20e7dafaa855e3610595886537ee4910b09d2e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643059"
---
# <a name="application-icons-in-xamarinios"></a>Xamarin 中的應用程式圖示

將會詳細介紹下列主題：

* [應用程式、焦點和設定圖示](#icon-types)-iOS 應用程式所需的不同圖示類型。
* [以資產目錄管理圖示](#managing)-使用資產目錄管理應用程式圖示。
* [Itunes 插圖](#itunes)-針對傳遞應用程式的臨機操作方法提供必要的 iTunes 插圖。

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>應用程式、焦點和設定圖示

如同 Xamarin iOS 應用程式可以使用 UI 控制項的影像資產, 以及做為檔圖示的相同方式, 影像資產可以用來提供應用程式圖示。 IPad 的下列螢幕擷取畫面說明 iOS 中圖示的三種用法:

- **應用程式圖示**-每個 iOS 應用程式都必須定義應用程式圖示。 這是使用者將從 iOS 主畫面中按一下以啟動應用程式的圖示。 此外, Game Center 會使用此圖示 (如果適用的話)。 範例： 

    [![](app-icons-images/000.png "應用程式圖示")](app-icons-images/000-full.png#lightbox)
- **焦點圖示**-每當使用者在焦點搜尋中輸入應用程式的名稱時, 就會顯示此圖示。 範例： 

    [![](app-icons-images/000a.png "焦點圖示")](app-icons-images/000a-full.png#lightbox)
- **設定圖示**-如果使用者在其 iOS 裝置上輸入 [**設定**] 應用程式, 此圖示將會顯示在應用程式的 [**設定**] 清單結尾。 範例： 

    [![](app-icons-images/000b.png "設定圖示")](app-icons-images/000b-full.png#lightbox)

需要下列映射資產大小和解析度, 以支援以 iOS 5 到 iOS 9 (或更新版本) 為目標的 Xamarin iOS 應用程式所需的所有圖示類型:

### <a name="iphone-icon-sizes"></a>iPhone 圖示大小

- **iPhone: iOS 9 & 10 (iPhone 6 & 7 Plus)**

    ||倍|
    |---|---|
    |應用程式圖示|180x180|
    |突出|120x120|
    |設定|87x87|

- **iPhone: iOS 7 & 8**

    ||1x|2x|
    |---|---|---|
    |應用程式圖示|60x60<sup>1</sup>|120x120|
    |突出|40x40<sup>2</sup>|80x80|
    |設定|-|-|

- **iPhone: iOS 5 & 6**

    ||1x|2x|
    |---|---|---|
    |應用程式圖示|57x57|114x114|
    |突出|29x29|58x58|
    |設定|29x29<sup>3、4</sup>|58x58<sup>3、4</sup>|

### <a name="ipad-icon-sizes"></a>iPad 圖示大小

- **iPad: iOS 9 & 10**

    ||2x (iPad Pro)|
    |---|---|
    |應用程式圖示|167x167<sup>6</sup>|
    |突出|120x120<sup>6</sup>|
    |設定|58x58<sup>5</sup>|

- **iPad: iOS 7 & 8**

    ||1x|2x|
    |---|---|---|
    |應用程式圖示|76x76|152x152|
    |突出|40x40|80x80|
    |設定|-|-|

- **iPad: iOS 5 & 6**

    ||1x|2x|
    |---|---|---|
    |應用程式圖示|72x72|144x144|
    |突出|50x50|100x100|
    |設定|29x29<sup>3、5</sup>|58x58<sup>3、5</sup>|

 1. Visual Studio for Mac 和 Xcode 不再支援為 iOS 7 設定1x 映射。
 2. 使用資產目錄時, 不支援設定 iOS 7 的1x 映射。
 3. iOS 7 & 8 使用與 iOS 5 & 6 相同的映射大小。
 4. 使用與焦點圖示相同的影像和大小。
 5. 使用與 iPhone 相同的大小圖示。
 6. 僅支援資產目錄映射集。
 
 如需圖示的詳細資訊, 請參閱 Apple 的[圖示和影像大小](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1)檔。

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>使用資產目錄管理圖示

針對圖示, 可以將`AppIcon`特殊映射集新增至應用程式`Assets.xcassets`專案中的檔案。 支援所有解析度所需的映射的所有版本都包含在_xcasset_中, 並群組在一起。 Visual Studio for Mac 中的特殊編輯器可讓開發人員以圖形方式包含和設定這些影像。

若要使用資產目錄, 請執行下列動作:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 按兩下 **方案總管**中`Info.plist`的檔案以開啟它進行編輯。
2. 向下卷到 [**應用程式圖示**] 區段。
3. 從 [**來源**] 下拉式清單中, 確定已選取 [ **AppIcon** ]: 

    ![](app-icons-images/migrate01.png "確定已選取 [AppIcon]")
4. 在 **方案總管**中, 按兩下`Assets.xcassets`檔案以開啟它進行編輯: 

    ![](app-icons-images/asset01.png "方案總管中的 assets.xcassets 檔案")
5. 從`AppIcon`要顯示的`Icon Editor`資產清單中選取:

    ![](app-icons-images/asset02.png "AppIcon 編輯器")
6. 按一下指定的圖示類型, 然後選取所需類型/大小的影像檔案, 或從資料夾中拖曳影像並放在所需的大小。
7. 按一下 [**開啟**] 按鈕, 在專案中包含影像, 並在 xcasset 中加以設定。
8. 針對所需的所有影像重複此動作。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 按兩下**方案總管**中的**plist**檔案:

    ![](app-icons-images/icon01w.png "選取 [Info] plist")
2. 按一下 [**視覺資產**] 索引標籤, 然後按一下 [**應用程式圖示**] 底下的 [**使用資產目錄**] 按鈕: 

    ![](app-icons-images/icon02w.png "選取 [視覺資產] 索引標籤")
3. 從 [**方案總管**] 中, 展開 [**資產目錄**] 資料夾: 

    ![](app-icons-images/image009.png "展開資產目錄資料夾")
4. 按兩下**媒體**檔案, 在編輯器中開啟它: 

    ![](app-icons-images/image010.png "在編輯器中開啟媒體檔案")
5. 開發人員可以在**Properties Explorer**中選取不同類型和所需的圖示大小。
6. 按一下指定的圖示類型, 然後選取所需類型/大小的影像檔案。
7. 按一下 [**開啟**] 按鈕, 在專案中包含影像, 並在 xcasset 中加以設定。
8. 針對所需的所有影像重複此動作。

-----

這是慣用的方法, 包括和管理將用來提供應用程式、焦點和設定圖示的影像資產。

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>從 Info. plist 遷移至資產目錄

對於使用`Info.plist`檔案來管理其圖示的現有 Xamarin iOS 應用程式, 強烈建議開發人員將其切換為`AppIcons`使用中`Assets.xcassets`的影像資產。

請執行下列動作：

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 按兩下 **方案總管**中`Info.plist`的檔案以開啟它進行編輯。
2. 向下卷到 [**應用程式圖示**] 區段。
3. 從 [**來源**] 下拉式清單中, 選取 [**遷移至資產目錄**]: 

    ![](app-icons-images/migrate02.png "選取 [遷移至資產目錄]")
4. 檔案中`Info.plist`所定義的任何現有圖示, 都會遷移到`AppIcons`新增至`Assets.xcassets`的映射集: 

     ![](app-icons-images/migrate03.png "Assets.xcassets 中所設定的 AppIcons 映射。")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 按兩下 **方案總管**中`Info.plist`的檔案以開啟它進行編輯。
2. 按一下 [iPhone 圖示] 區段: 

    ![](app-icons-images/image007.png "報表 iPhone 圖示編輯器")
3. 向下卷到 [**圖示**] 區段。
4. 從 [**資產目錄**] 下拉式清單中, 選取 [**使用資產目錄**]。
5. 檔案中`Info.plist`所定義的任何現有圖示, 都會遷移到`Images`新增至`Assets.xcassets`的集合。
6. 將變更儲存至 `Info.plist` 檔案。

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes 插圖

如果使用傳遞應用程式的臨機操作方法 (適用于公司使用者或實際裝置上的 Beta 測試), 開發人員也必須包含512x512 和1024x1024 映射, 以用來代表 iTunes 中的應用程式。

若要指定 iTunes 插圖，請執行下列動作：

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 按兩下 **方案總管**中`Info.plist`的檔案以開啟它進行編輯。
2. 流覽至編輯器的**ITunes 藝術品**區段: 

    ![](app-icons-images/itunes01.png "流覽至編輯器的 iTunes 藝術品區段")
3. 針對任何遺失的影像, 按一下編輯器中的縮圖, 從 [開啟檔案] 對話方塊中選取所需 iTunes 插圖的影像檔案, 然後按一下 [**確定]** 按鈕。
4. 重複此步驟, 直到為應用程式指定所有所需的映射為止。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 按兩下 **方案總管**中`Info.plist`的檔案以開啟它進行編輯。

2. 按一下 [**視覺資產**] 索引標籤, 然後展開**iTunes 插圖**: 

    ![](app-icons-images/itunes01w.png "編輯 Visual Studio 中的 iTunes 插圖")
3. 針對任何遺漏的影像, 按一下編輯器中的縮圖, 從 [開啟檔案] 對話方塊中選取所需 iTunes 插圖的影像檔案, 然後按一下 [**開啟**] 按鈕。
4. 重複此步驟, 直到為應用程式指定所有所需的映射為止。

-----

## <a name="related-links"></a>相關連結

- [使用影像 (範例)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [自訂圖示和映射建立指導方針](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
