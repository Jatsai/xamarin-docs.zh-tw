---
title: JAVA 與 Xamarin 整合
description: JAVA 生態系統包含各種不同的元件集合。 其中許多元件都可以用來減少開發 Android 應用程式所需的時間。 本檔將介紹並提供開發人員使用這些現有 JAVA 元件來改善其 Xamarin. Android 應用程式開發體驗的一些方式的高階總覽。
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/18/2017
ms.openlocfilehash: 22c3217eec1b2e531ad4534fc1cb35a701a06e34
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524081"
---
# <a name="java-integration-with-xamarinandroid"></a>JAVA 與 Xamarin 整合

_JAVA 生態系統包含各種不同的元件集合。其中許多元件都可以用來減少開發 Android 應用程式所需的時間。本檔將介紹並提供開發人員使用這些現有 JAVA 元件來改善其 Xamarin. Android 應用程式開發體驗的一些方式的高階總覽。_

## <a name="overview"></a>總覽

基於 JAVA 生態系統的範圍, 您很有可能是 Xamarin Android 應用程式所需的任何功能都已經以 JAVA 編碼。 因此, 在建立 Xamarin. Android 應用程式時, 嘗試並重複使用這些現有的程式庫會很有吸引力。

有三種可能的方式可以在 Xamarin Android 應用程式中重複使用 JAVA 程式庫: 

- **建立 JAVA**系結程式庫使用這項技術時, 會使用 Xamarin. Android 專案來C#建立 JAVA 類型的包裝函式。 &ndash; 然後, Xamarin 應用程式可以參考此專案C#所建立的包裝函式, 然後使用該`.jar`檔案。 

- **JAVA 原生介面**C++ C# JAVA 原生介面 (JNI) 是一種架構, 可讓非 JAVA 程式碼 (例如或) 呼叫或由在 JVM 內執行的 JAVA 程式碼呼叫。 &ndash; 

- **埠程式碼**此方法牽涉到取得 JAVA 原始程式碼, 然後將它轉換成C# &ndash; 這可以手動執行, 或使用自動化工具 (例如「銳化」) 來完成。 

前兩項技術的核心是*JAVA 原生介面*(JNI)。 JNI 是一種架構, 可讓不是以 JAVA 撰寫的應用程式與在 JAVA 虛擬機器中執行的 JAVA 程式碼互動。 Xamarin 會使用 JNI 來建立程式代碼的C#系結。 

第一種方法是更自動化的宣告式 JAVA 程式庫方法。 其牽涉到使用 Visual Studio for Mac 或由 Xamarin &ndash;提供的 Visual Studio 專案類型 JAVA 系結程式庫。 若要成功建立這些系結, JAVA 系結程式庫可能仍需要進行一些手動修改, 但不像單純的 JNI 方法那麼多。 如需 JAVA 系結程式庫的詳細資訊, 請參閱系結[java](~/android/platform/binding-java-library/index.md)程式庫。 

第二種方法是使用 JNI, 以較低的層級運作, 但可以提供更細微的控制, 並存取通常無法透過 JAVA 系結程式庫存取的 JAVA 方法。 

第三個技術與前兩個方法截然不同: 將程式碼從 JAVA 移植C#到。 將程式碼從一種語言移植至另一種語言可能是非常費力的過程, 但可以透過稱為「*銳化*」的工具來降低這項工作。 「銳化」是一種開放原始碼工具, 它是C# JAVA 到轉換器。 



## <a name="summary"></a>總結

本檔提供一些可在 Xamarin Android 應用程式中重複使用 JAVA 程式庫之不同方式的高階總覽。 其中引進了系結和受控可呼叫包裝函式的概念, 並討論將 JAVA C#程式碼移植至的選項。 


## <a name="related-links"></a>相關連結

- [架構](~/android/internals/architecture.md)
- [繫結 Java 程式庫](~/android/platform/binding-java-library/index.md)
- [使用 JNI](~/android/platform/java-integration/working-with-jni.md)
- [銳利化](https://github.com/slluis/sharpen)
- [JAVA 原生介面](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
