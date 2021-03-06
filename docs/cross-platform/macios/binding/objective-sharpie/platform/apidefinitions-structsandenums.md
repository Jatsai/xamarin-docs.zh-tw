---
title: ApiDefinitions 與 StructsAndEnums 檔案
description: 本文件說明目標 Sharpie 產生 ApiDefinitions.cs 和 StructsAndEnums.cs 檔案。 這些檔案接著會用來存取 OBJECTIVE-C 程式碼，從C#。
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3b991f6105c6053f473b049d195aaef63cbcdd57
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977670"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions 與 StructsAndEnums 檔案

當目標 Sharpie 曾順利執行時，它會產生`Binding/ApiDefinitions.cs`和`Binding/StructsAndEnums.cs`檔案。
這兩個檔案會新增至繫結專案在 Visual Studio for Mac，或直接傳遞至`btouch`或`bmac`工具來產生最終的繫結。

在 *某些*但多個通常開發人員必須以手動方式修改這些產生的檔案，以修正任何問題，可能不會自動處理工具 （例如那些標有旗標，這些產生的檔案可能會在需要時，所有的案例具有[`Verify`屬性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md))。

接下來的步驟包括：

- **調整名稱**:有時會想要調整的方法和類別，以符合.NET Framework 設計方針的名稱。
- **方法或屬性**:有時候使用目標 Sharpie 的啟發學習法會挑選要轉換成屬性的方法。 此時，您可能會決定這是否預期的行為。
- **連結事件**:您無法連結您與您的委派類別的類別，並自動產生這些事件。
- **通知連結**:不可能從單純的標頭檔中擷取 API 合約的通知，這將需要某趟車程支付的 API 文件。 如果您想強類型的通知，您必須更新結果。
- **API 策展**:此時，您可以選擇提供額外的建構函式，加入方法 (以允許C#初始化上建構語法)，運算子多載和額外的定義檔案中實作您自己的介面。

請參閱[繫結 API](~/cross-platform/macios/binding/objective-c-libraries.md)描述，請參閱 < 如何將這些檔案放入繫結程序中，如下圖所示：

![](apidefinitions-structsandenums-images/binding-flowchart.png "此圖顯示繫結程序")

請參閱[繫結型別參考](~/cross-platform/macios/binding/binding-types-reference.md)如需這些檔案的內容。

