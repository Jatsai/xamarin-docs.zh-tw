---
title: Xamarin.Essentials:疑難排解
description: 本文件說明如何為使用 Xamarin.Essentials 程式庫開發時遇到的問題進行疑難排解。
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.openlocfilehash: a9546923c9a17aade58d830ea2504406ad09d7d7
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870075"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials:疑難排解

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>錯誤：偵測到 Xamarin.Android.Support.Compat 的版本衝突

更新使用 Xamarin.Essentials 之 Xamarin.Forms 專案隨附的 NuGet 套件 (或新增套件) 時，可能會發生下列錯誤：

```error
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 1.1.0 -> Xamarin.Android.Support.CustomTabs 28.0.0.1 -> Xamarin.Android.Support.Compat (= 28.0.0.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

問題在於這兩個 NuGet 的相依性不符。 您可以手動新增支援兩者的特定相依性版本 (在本例中為 **Xamarin.Android.Support.Compat**)，來解決此問題。

若要這樣做，請手動新增衝突來源的 NuGet，並使用 [版本] 清單來選取特定版本。 Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util NuGet 的目前版本 28.0.0.1 將會解決此錯誤。

如需如何解決問題的詳細資訊和影片，請參閱[此部落格文章](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/)。

如果遇到任何問題或發現 Bug，請於 [Xamarin.Essentials GitHub 存放庫](https://github.com/xamarin/Essentials)回報。
