---
title: "觸控"
description: "在許多現今的裝置上的觸控式螢幕讓使用者快速且有效地與裝置互動以自然且直覺式的方式。 不只限於簡單觸控偵測這種互動 – 也可使用筆勢以及。 例如，縮小-放大手勢是螢幕的一個非常常見的範例 – 就與使用者都可以放大或縮小的兩指部分。本指南會檢查觸控與手勢在 iOS 中。"
ms.topic: article
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: b0e1cf8b1cb18982fe319fef7c524aeb70be4a9b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/12/2018
---
# <a name="touch"></a>觸控

_在許多現今的裝置上的觸控式螢幕讓使用者快速且有效地與裝置互動以自然且直覺式的方式。不只限於簡單觸控偵測這種互動 – 也可使用筆勢以及。例如，縮小-放大手勢是螢幕的一個非常常見的範例 – 就與使用者都可以放大或縮小的兩指部分。本指南會檢查觸控與手勢在 iOS 中。_


如同其他的行動平台，iOS 會具有數種方式來處理觸控。 它可以支援多點觸控 — 許多在螢幕上的連絡點 — 和複雜的筆勢。 本指南介紹一些概念，以及任用實作在 iOS 上的觸控與手勢。

iOS 封裝中的觸控資料`UITouch`類別，可透過一系列的應用程式`UIResponder`方法。 應用程式可以覆寫這些方法中的子類別`UIView`和`UIViewController`，兩者都繼承自`UIResponder`。

除了擷取觸控資料，iOS 會提供方法，來解譯成筆勢修飾的模式。 這些筆勢辨識器接著可用來解譯應用程式特定的命令，例如將影像的旋轉或開啟的頁面。 iOS 提供豐富的類別處理一般筆勢與最小加入的程式碼集合。

觸控和筆勢辨識器之間做出選擇可能很令人混淆的其中一個。 本指南建議的一般情況下，喜好設定應該授予筆勢辨識器。 筆勢辨識器會實作做為離散類別，提供更大的分隔考量和更好的封裝。 這可讓您直接共用不同的檢視，減少撰寫的程式碼之間的邏輯。

不過，但有些的時候，當您需要使用低層級的觸控處理甚至建立 finger-paint 程式追蹤多個指。

## <a name="sections"></a>章節

-  [在 iOS 中觸控](touch-in-ios.md)
-  [逐步解說： 在 iOS 中使用觸控](ios-touch-walkthrough.md)
-  [多點觸控追蹤](touch-tracking.md)

本指南可做為在 iOS 中觸控簡介。 如需有關使用中 iOS 3D Touch 和 Haptic 意見反應的詳細資訊，其中引進在 iOS 9 和 10 分別，請參閱下方的特定指南中：

* [3D Touch](~/ios/platform/3d-touch.md)
* [提供 Haptic 意見反應](~/ios/user-interface/ios-ui/haptic-feedback.md)



## <a name="related-links"></a>相關連結

- [iOS 觸控開始 （範例）](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS 觸控最終 （範例）](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [FingerPaint （範例）](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)