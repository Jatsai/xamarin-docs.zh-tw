---
title: Getting Started With 目標 Sharpie
description: 本文件提供的高階目標 Sharpie，用來自動建立的工具概觀C#繫結 OBJECTIVE-C 程式碼。
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: 4fb5e503a82a2b666bf6f8d7d7166475e94546e7
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978408"
---
# <a name="getting-started-with-objective-sharpie"></a>Getting Started With 目標 Sharpie

> [!IMPORTANT]
> 目標 Sharpie 是經驗豐富的 Xamarin 開發人員提供進階知識的 Objective C （並延伸，C） 的工具。 然後再嘗試繫結 Objective C 程式庫中，您應該有穩固的了解如何建置命令列 （和深入了解原生程式庫的運作方式） 上的原生程式庫。

<a name="installing" />

## <a name="installing-objective-sharpie"></a>安裝目標 Sharpie

目標 Sharpie 目前是獨立的命令列工具的 Mac OS X 10.10 及更新版本，而且_不完全支援的 Xamarin 產品_。 它應該只供進階開發人員可協助建立一個繫結專案到第 3 方 OBJECTIVE-C 程式庫。

目標 Sharpie 可以下載為標準的 OS X 套件安裝程式。
執行安裝程式，並依照所有螢幕上從安裝精靈的提示：

- **目前的版本：3.4**
  - [下載最新版本](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
  - [論壇公告](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> 使用`sharpie update`命令，更新為最新版本。

## <a name="basic-walkthrough"></a>基本的逐步解說

目標 Sharpie 是建立繫結第 3 的合作對象 Objective C 程式庫，以所需的定義，可以協助的 Xamarin 提供的命令列工具C#。
即使是使用目標 Sharpie，開發人員*將*需要目標 Sharpie 完成來解決任何問題，可能不會自動處理此工具之後，修改產生的檔案。

可能的話，目標 Sharpie 會加上註解有一些不確定如何正確地繫結的 Api （原生程式碼中的許多建構是模稜兩可的）。
這些註解會顯示為[`[Verify]`屬性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)。

目標 Sharpie 的輸出是一組檔案- [ `ApiDefinition.cs`並`StructsAndEnums.cs` ](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -，可用來建立繫結的專案，您可以使用編譯成程式庫中的 Xamarin 應用程式。

> [!IMPORTANT]
> 目標 Sharpie 隨附於**主要**適當使用方式的規則： 您必須完全將它傳遞正確的 clang 編譯器命令列引數以確保正確剖析。 這是因為剖析階段目標 Sharpie 的簡單工具[對 clang libtooling API 實作](http://clang.llvm.org/docs/LibTooling.html)。

也就是說，目標 Sharpie Clang 的完整功能 (C/Objective C /C++您將繫結的編譯器，實際上會編譯原生程式庫) 和所有其內部知識的繫結的標頭檔。
而不是轉譯之已剖析[AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree)物件的程式碼，目標 Sharpie 轉譯至 ASTC#繫結 」 建立 「 適用於輸入至`bmac`並`btouch`Xamarin 繫結工具。

如果目標 Sharpie 錯誤出在剖析期間，就表示發生錯誤時該 clang 其剖析階段嘗試建構 AST，而且您需要找出原因。

**新功能 ！** 3.0 版嘗試解決這些複雜的直接支援的 Xcode 專案。 如果原生程式庫中有有效的 Xcode 專案，目標 Sharpie 可以評估指定的目標和設定，以減少所需的輸入標頭檔和編譯器旗標的專案。

如果沒有任何的 Xcode 專案可供使用，您必須為與專案更熟悉的推算正確輸入標頭檔、 標頭檔搜尋路徑，以及其他必要的編譯器旗標。 請務必了解用來建置原生程式庫的編譯器旗標都相同，必須傳遞至目標 Sharpie。 這是較為手動的程序，以及需要熟悉命令列中利用 Clang 工具鏈的原生程式碼編譯一堆。

**新功能 ！** 版本 3.0 也引進了一項工具輕鬆地繫結[CocoaPods](https://cocoapods.org)透過`sharpie pod`命令。
如果您感興趣的程式庫是以 CocoaPod，我們建議您先嘗試將具有目標 Sharpie CocoaPod 繫結 （相對於嘗試直接對來源繫結）。
