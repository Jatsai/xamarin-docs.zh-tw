---
title: 如何在 Xamarin Studio 中，為 iOS 專案設定 Mono 執行階段環境變數？
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/31/2017
ms.openlocfilehash: ba74e316f706e5bf22f973de4dad38d94ccd0db9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2019
ms.locfileid: "61417655"
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>如何在 Xamarin Studio 中，為 iOS 專案設定 Mono 執行階段環境變數？

如果您需要設定任何執行階段環境變數的 Mono，他們可以設定**專案選項 > 執行 > 一般**頁面。

注意:SGen 記憶體回收集合環境變數 (單聲道\_GC\_PARAMS) 從 Xamarin Studio 啟動時，才會使用這種方式的設定。 如果您啟動從裝置應用程式時，將會忽略 Sgen 的設定。 

若要永久設定環境變數中的應用程式，您需要將此新增為其他 mtouch 引數 （適用於所有相關的設定）：

```csharp
   --setenv=NAME=VALUE
```

若要查看可以設定環境變數，請參閱 Mono man 頁面：[http://docs.go-mono.com/?link=man%3amono(1)](http://docs.go-mono.com/?link=man%3amono(1)) 請參閱節： `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "設定專案的環境變數")
