---
title: "相關文件"
description: "適用於 macOS 開發人員的其他文件連結。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0a282c58-1c37-4f73-8440-85de2daf454a
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: 000061d8ed884f1eb248517c6131ec49134cb8a1
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2018
---
# <a name="related-documentation"></a>相關文件

除了 [developer.xamarin.com](~/mac/get-started/index.md) 的 Mac 一節外，還有三個很棒的文件來源也可以協助解決 Xamarin.Mac 問題：

- [**Xamarin.iOS 文件**](~/ios/get-started/index.md) - 對許多 API 而言 (主要是 AppKit/UIKit 之外)，iOS 和 macOS 版本之間只有微小的差異。 在某些情況下，當指定的 iOS API 的名稱是 `UIFoo` 時，即可在 macOS 上找到名為 `NSFoo` 的類似 API。 這些範例通常已在 C# 中。

- **Apple 的 [Mac 開發人員中心](https://developer.apple.com/devcenter/mac/)** \(英文\) - 很多時候，例如在 Objective-C 呼叫的 API，都可以直接的方式轉換成 C#。 如需如何執行這項操作的詳細資訊，請參閱[了解 Mac API](~/mac/app-fundamentals/mac-apis.md)。

- [**Stack Overflow**](http://stackoverflow.com/) - 簡單的單一問題的絕佳資源，例如[如何自動展開 NSOutlineView 中的所有節點](http://stackoverflow.com/questions/519751/nsoutlineview-auto-expand-all-nodes) \(英文\)。 這些範例通常會在 Objective-C 中，而且需要轉換成 C#，但在 C# 中有一部分的答案。

## <a name="user-interface"></a>使用者介面

在 Xamarin.Mac 應用程式中使用 C# 和 .NET 時，開發人員可以存取在 *OBJECTIVE-C* 和 *Xcode* 中工作的開發人員可以存取的相同使用者介面控制項。 由於 Xamarin.Mac 直接與 Xcode 整合，開發人員可以使用 Xcode 的 _Interface Builder_ 來建立與維護應用程式的使用者介面 (或選擇直接在 C# 程式碼中建立它們)。

下列指南提供在 Xamarin.Mac 應用程式中使用 macOS 元素的相關詳細資訊：

- [Windows](~/mac/user-interface/window.md)
- [對話方塊](~/mac/user-interface/dialog.md)
- [警示](~/mac/user-interface/alert.md)
- [功能表](~/mac/user-interface/menu.md)
- [工具列](~/mac/user-interface/toolbar.md)
- [資料表檢視](~/mac/user-interface/table-view.md)
- [大綱檢視](~/mac/user-interface/outline-view.md)
- [來源清單](~/mac/user-interface/source-list.md)