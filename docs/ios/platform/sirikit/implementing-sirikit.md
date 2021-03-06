---
title: 在 Xamarin 中執行 SiriKit
description: 本檔說明在 Xamarin iOS 應用程式中執行 SiriKit 支援所需的步驟。 其中討論意圖延伸模組和意圖 UI 延伸模組。
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/03/2018
ms.openlocfilehash: 78d39b080a136f66c81b48d6cceb5fbdd9d24c11
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654933"
---
# <a name="implementing-sirikit-in-xamarinios"></a>在 Xamarin 中執行 SiriKit

_本文涵蓋在 Xamarin iOS 應用程式中執行 SiriKit 支援所需的步驟。_

SiriKit 是 iOS 10 的新手, 可讓 Xamarin iOS 應用程式提供使用者可使用 Siri 和 iOS 裝置上的 Maps 應用程式來存取的服務。 本文涵蓋在 Xamarin iOS 應用程式中, 藉由新增必要的意圖擴充功能、意圖 UI 延伸模組和詞彙來執行 SiriKit 支援所需的步驟。

Siri 適用于**網域**的概念、相關工作的已知動作群組。 應用程式與 Siri 之間的每個互動都必須屬於其中一個已知的服務網域, 如下所示:

- 音訊或影片通話。
- 預約。
- 管理 workouts。
- 關鍵.
- 正在搜尋相片。
- 傳送或接收付款。

當使用者提出包含其中一個應用程式延伸模組服務的 Siri 要求時, SiriKit 會將描述使用者要求的**意圖**物件以及任何支援的資料傳送給此延伸模組。 然後, 應用程式延伸模組會針對指定的**意圖**產生適當的**回應**物件, 詳述延伸模組如何處理要求。

本指南將提供在現有應用程式中包含 SiriKit 支援的快速範例。 基於此範例的目的, 我們將使用假的 MonkeyChat 應用程式:

[![](implementing-sirikit-images/monkeychat01.png "MonkeyChat 圖示")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat 會保留自己的使用者朋友的連絡人書籍, 每個都與一個螢幕名稱相關聯 (例如 Bobo), 並允許使用者透過其螢幕名稱將文字聊天傳送給每位朋友。

## <a name="extending-the-app-with-sirikit"></a>使用 SiriKit 擴充應用程式

如[瞭解 SiriKit 概念](~/ios/platform/sirikit/understanding-sirikit.md)指南中所示, 使用 SiriKit 擴充應用程式有三個主要部分:

[![](implementing-sirikit-images/elements01.png "使用 SiriKit 圖擴充應用程式")](implementing-sirikit-images/elements01.png#lightbox)

它們包括：

1. **意圖延伸**模組-驗證使用者回應, 確認應用程式可處理要求, 並實際執行工作以完成使用者的要求。
2. **意圖 UI 延伸** - 模組 (*選擇性*) 提供自訂 UI 給 Siri 環境中的回應, 並可將應用程式 ui 和商標帶入 Siri, 以豐富使用者的體驗。
3. **應用**程式-為應用程式提供使用者特定詞彙, 以協助 Siri 使用它。 

下列各節將詳細說明所有這些元素以及將它們包含在應用程式中的步驟。


## <a name="preparing-the-app"></a>準備應用程式

SiriKit 是以擴充功能為基礎, 不過, 在將任何延伸模組新增至應用程式之前, 開發人員必須執行幾項工作, 以協助採用 SiriKit。

### <a name="moving-common-shared-code"></a>移動一般共用程式碼 

首先, 開發人員可以將應用程式與擴充功能之間共用的一些通用程式碼, 移至_共用專案_、_可移植的類別庫 (Pcl)_ 或_原生程式庫_。

這些擴充功能必須能夠執行應用程式所做的一切動作。 在範例 MonkeyChat 應用程式的條款中, 尋找連絡人、新增連絡人、傳送訊息和抓取訊息歷程記錄等專案。

藉由將此通用程式碼移至共用專案、PCL 或原生程式庫, 可讓您輕鬆地在一個共同的位置維護該程式碼, 並確保延伸模組和父系應用程式能為使用者提供一致的體驗和功能。

在範例應用程式 MonkeyChat 的情況下, 資料模型和處理常式代碼 (例如網路和資料庫存取) 將會移至原生程式庫。

請執行下列動作：

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 啟動 Visual Studio for Mac 並開啟 MonkeyChat 應用程式。
2. 以滑鼠右鍵按一下**Solution Pad**中的方案名稱, 然後選取 [**加入** > **新專案**]: 

    [![](implementing-sirikit-images/prep01.png "加入新的專案")](implementing-sirikit-images/prep01.png#lightbox)
3. 選取 [ **iOS**  > 連結**庫** > ]**類別庫**, 然後按 [**下一步]** 按鈕: 

    [![](implementing-sirikit-images/prep02.png "選取類別庫")](implementing-sirikit-images/prep02.png#lightbox)
4. 輸入`MonkeyChatCommon`作為**名稱**, 然後按一下 [**建立**] 按鈕: 

    [![](implementing-sirikit-images/prep03.png "在 [名稱] 中輸入 MonkeyChatCommon")](implementing-sirikit-images/prep03.png#lightbox)
5. 在**方案總管**中, 以滑鼠右鍵按一下主要應用程式的 [**參考**] 資料夾, 然後選取 [**編輯參考 ...** ]。檢查**MonkeyChatCommon**專案, 然後按一下 [**確定]** 按鈕: 

    [![](implementing-sirikit-images/prep05.png "檢查 MonkeyChatCommon 專案")](implementing-sirikit-images/prep05.png#lightbox)
6. 在**方案總管**中, 將通用的共用程式碼從主要應用程式拖曳至原生程式庫。
7. 在 MonkeyChat 的案例中, 將 [ **DataModels** ] 和 [**處理器**] 資料夾從主要應用程式拖曳至原生程式庫: 

    [![](implementing-sirikit-images/prep06.png "方案總管中的 [DataModels] 和 [處理器] 資料夾")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 啟動 Visual Studio 並開啟 MonkeyChat 應用程式。
2. 以滑鼠右鍵按一下**方案總管**中的方案名稱, 然後選取 [**加入** > **新專案**]。
3. 選取 **[ C#視覺化**  > **共用專案**], 然後按 [**下一步]** 按鈕: 

    [![](implementing-sirikit-images/prep02.w157-sml.png "選取類別庫")](implementing-sirikit-images/prep02.w157.png#lightbox)
4. 輸入`MonkeyChatCommon`作為**名稱**, 然後按一下 [**建立**] 按鈕。
5. 在**方案總管**中, 以滑鼠右鍵按一下主要應用程式的 [**參考**] 資料夾, 然後選取 [**編輯參考 ...** ]。檢查**MonkeyChatCommon**專案, 然後按一下 [**確定]** 按鈕: 

    [![](implementing-sirikit-images/prep05w.png "檢查 MonkeyChatCommon 專案")](implementing-sirikit-images/prep05w.png#lightbox)
6. 在 **方案總管**中, 將通用的共用程式碼從主要應用程式拖曳至共用專案。
7. 在 MonkeyChat 的案例中, 將 [ **DataModels** ] 和 [**處理器**] 資料夾從主要應用程式拖曳至原生程式庫。

-----

編輯任何已移至原生程式庫的檔案, 並將命名空間變更為符合程式庫的名稱。 例如, 變更`MonkeyChat`為`MonkeyChatCommon`:

```csharp
using System;
namespace MonkeyChatCommon
{
    /// <summary>
    /// A message sent from one user to another within a conversation.
    /// </summary>
    public class MonkeyMessage
    {
        public MonkeyMessage ()
        {
        }
        ...
    }
}
```

接下來, 回到主要應用程式, 並在應用`using`程式使用其中一個已移動類別的任何位置, 為原生程式庫的命名空間新增語句:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using MonkeyChatCommon;

namespace MonkeyChat
{
    public partial class MasterViewController : UITableViewController
    {
        public DetailViewController DetailViewController { get; set; }

        DataSource dataSource;
        ...
    }
}
```

### <a name="architecting-the-app-for-extensions"></a>架構應用程式的擴充功能

一般而言, 應用程式會註冊多個意圖, 而開發人員必須確保應用程式會針對適當數目的意圖延伸模組進行架構。

在應用程式需要多個意圖的情況下, 開發人員可以選擇將其所有意圖處理放在一個意圖延伸模組中, 或為每個意圖建立個別的意圖延伸模組。

如果選擇為每個意圖建立個別的意圖延伸模組, 開發人員最後可能會在每個延伸模組中複製大量的重複使用程式碼, 並建立大量的處理器和記憶體額外負荷。

若要協助在兩個選項之間進行選擇, 請查看是否有任何意圖自然屬於同一個。 例如, 進行音訊和影片呼叫的應用程式可能會想要將這兩個意圖包含在單一意圖延伸中, 因為它們會處理類似的工作, 並可提供最多的程式碼重複使用。

針對不符合現有群組的任何意圖或意圖群組, 請在應用程式的解決方案中建立新的意圖延伸模組以包含這些專案。


### <a name="setting-the-required-entitlements"></a>設定必要權利

任何包含 SiriKit 整合的 Xamarin iOS 應用程式, 都必須設定正確的權利。 如果開發人員未正確設定這些必要權利, 他們將無法在實際的 iOS 10 (或更新版本) 硬體上安裝或測試應用程式, 因為 iOS 10 模擬器不支援 SiriKit。

請執行下列動作：

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 按兩下 **方案總管**中`Entitlements.plist`的檔案以開啟它進行編輯。
2. 若要切換**來源**索引標籤。
3. `Boolean` `Yes`新增屬性, 並將類型設定為, 並將值設為: `com.apple.developer.siri` 

    [![](implementing-sirikit-images/setup01.png "新增 siri 屬性。")](implementing-sirikit-images/setup01.png#lightbox)
4. 將變更儲存到檔案。
5. 按兩下**專案檔**中**方案總管中**以開啟它進行編輯。
6. 選取 **iOS 套件組合簽署**，並確定`Entitlements.plist`中選取檔案**自訂權利**欄位： 

    [![](implementing-sirikit-images/setup02.png "在 [自訂權利] 欄位中選取 plist 檔案")](implementing-sirikit-images/setup02.png#lightbox)
7. 按一下 [確定]  按鈕以儲存所做的變更。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 按兩下 **方案總管**中`Entitlements.plist`的檔案以開啟它進行編輯。
2. `Boolean` `Yes`新增屬性, 並將類型設定為, 並將值設為: `com.apple.developer.siri` 

    [![](implementing-sirikit-images/setup01w.png "新增 siri 屬性。")](implementing-sirikit-images/setup01w.png#lightbox)
3. 將變更儲存到檔案。
4. 按兩下**專案檔**中**方案總管中**以開啟它進行編輯。
5. 選取 [iOS 套件組合**簽署**], `Entitlements.plist`並確認已在 [**自訂權利**] 欄位中選取檔案。

-----

完成後, 應用程式的`Entitlements.plist`檔案應該如下所示 (在外部編輯器中開啟):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.siri</key>
    <true/>
</dict>
</plist>
```

### <a name="correctly-provisioning-the-app"></a>已正確布建應用程式

由於 Apple 已在 SiriKit 架構中提供嚴格的安全性, 因此任何執行 SiriKit 的 Xamarin iOS 應用程式都_必須_具有正確的應用程式識別碼和權利 (請參閱上一節), 而且必須以適當的布建設定檔簽署。

在您的 Mac 上執行下列動作:

1. 在網頁瀏覽器中, 流覽[https://developer.apple.com](https://developer.apple.com)至並登入您的帳戶。
2. 按一下 [**憑證**、**識別碼**及**設定檔**]。
3. 選取 **佈建設定檔**，然後選取 **應用程式識別碼**，然後按一下 **+** 按鈕。
4. 輸入新設定檔的**名稱**。
5. 輸入遵循 Apple 命名建議的配套**識別碼**。
6. 向下**應用程式服務**區段中, 選取 [ **SiriKit** ], 然後按一下 [**繼續**] 按鈕: 

    [![](implementing-sirikit-images/setup03.png "選取 SiriKit")](implementing-sirikit-images/setup03.png#lightbox)
7. 確認所有設定, 然後**提交**應用程式識別碼。
8. 選取 **佈建設定檔** > **開發**，按一下 **+** 按鈕，選取 **Apple ID**，然後按一下 **繼續**。
9. 按一下 [全選], 然後按一下 [**繼續**]。
10. 再次按一下 [**全選**], 然後按一下 [**繼續**]。
11. 使用 Apple 的命名建議來輸入**設定檔名稱**, 然後按一下 [**繼續**]。
12. 啟動 Xcode。
13. 從 [Xcode] 功能表選取 [**喜好設定 ...** ]
14. 選取 [**帳戶**], 然後按一下 [**查看詳細資料 ...** ] button 

    [![](implementing-sirikit-images/setup04.png "選取帳戶")](implementing-sirikit-images/setup04.png#lightbox)
15. 按一下左下角的 [**下載所有設定檔**] 按鈕: 

    [![](implementing-sirikit-images/setup05.png "下載所有設定檔")](implementing-sirikit-images/setup05.png#lightbox)
16. 確定先前建立的布建**設定檔**已安裝在 Xcode 中。
17. 開啟要在 Visual Studio for Mac 中新增 SiriKit 支援的專案。
18. 按兩下 **方案總管**中`Info.plist`的檔案。
19. 確定套件組合**識別碼**與上述的 Apple 開發人員入口網站中所建立的套件相符: 

    [![](implementing-sirikit-images/setup06.png "套件組合識別碼")](implementing-sirikit-images/setup06.png#lightbox)
20. 在**方案總管**，選取**專案**。
21. 以滑鼠右鍵按一下專案, 然後選取 [**選項**]。
22. 選取 **iOS 套件組合簽署**，選取**簽署身分識別**並**佈建設定檔**上面所建立： 

    [![](implementing-sirikit-images/setup07.png "選取 [簽署識別] 和 佈建設定檔")](implementing-sirikit-images/setup07.png#lightbox)
23. 按一下 [確定]  按鈕以儲存所做的變更。

> [!IMPORTANT]
> 測試 SiriKit 只適用于實際的 iOS 10 硬體裝置, 而不是在 iOS 10 模擬器中。 如果在實際硬體上安裝已啟用 SiriKit 的 Xamarin iOS 應用程式發生問題, 請確定已在 Apple 的開發人員入口網站和 Visual Studio for Mac 中正確設定必要的權利、應用程式識別碼、簽署識別碼和布建設定檔。

### <a name="requesting-siri-authorization"></a>要求 Siri 授權

在應用程式新增任何使用者特定詞彙或意圖延伸模組連接到 Siri 之前, 它必須向使用者要求授權, 才能存取 Siri。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

編輯應用程式的`Info.plist`檔案, 切換至**來源** `NSSiriUsageDescription`視圖並加入具有字串值的索引鍵, 以描述應用程式將如何使用 Siri, 以及將傳送哪些類型的資料。 例如, MonkeyChat 應用程式可能會說「MonkeyChat 連絡人將會傳送至 Siri」:

[![](implementing-sirikit-images/request01.png "Plist 編輯器中的 NSSiriUsageDescription")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

編輯應用程式的`Info.plist`檔案, 並以`NSSiriUsageDescription`字串值新增金鑰, 其中描述應用程式將如何使用 Siri, 以及將傳送哪些類型的資料。 例如, MonkeyChat 應用程式可能會說「MonkeyChat 連絡人將會傳送至 Siri」:

[![](implementing-sirikit-images/request01w.png "Plist 編輯器中的 NSSiriUsageDescription")](implementing-sirikit-images/request01w.png#lightbox)

-----

第一次啟動應用`INPreferences`程式時, 呼叫類別的方法。`RequestSiriAuthorization` 編輯類別, `FinishedLaunching`讓方法看起來如下所示: `AppDelegate.cs`


```csharp
using Intents;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{

    // Request access to Siri
    INPreferences.RequestSiriAuthorization ((INSiriAuthorizationStatus status) => {
        // Respond to returned status
        switch (status) {
        case INSiriAuthorizationStatus.Authorized:
            break;
        case INSiriAuthorizationStatus.Denied:
            break;
        case INSiriAuthorizationStatus.NotDetermined:
            break;
        case INSiriAuthorizationStatus.Restricted:
            break;
        }
    });


    return true;
}
```

第一次呼叫此方法時, 會顯示警示, 提示使用者允許應用程式存取 Siri。 開發人員新增到上述的`NSSiriUsageDescription`訊息將會顯示在此警示中。 如果使用者一開始拒絕存取, 他們就可以使用 [**設定**] 應用程式來授與應用程式的存取權。

應用程式隨時都可以藉由呼叫`SiriAuthorizationStatus` `INPreferences`類別的方法, 檢查應用程式存取 Siri 的能力。

### <a name="localization-and-siri"></a>當地語系化和 Siri

在 iOS 裝置上, 使用者可以選取與系統預設值不同的 Siri 語言。 使用當地語系化資料時, 應用程式必須使用`SiriLanguageCode` `INPreferences`類別的方法, 從 Siri 取得語言代碼。 例如：

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>新增使用者特定詞彙

使用者特定詞彙將提供對應用程式的個別使用者而言是唯一的單字或片語。 這些將在執行時間從主要應用程式 (而非應用程式延伸模組) 提供為一組已排序的字詞, 並以使用者最重要的使用優先順序排序, 並在清單的開頭具有最重要的詞彙。

使用者特定詞彙必須屬於下列其中一個類別:

- 連絡人名稱 (不受 Contacts 架構管理)。
- 相片標記。
- 相片專輯名稱。
- 健身名稱。

選取要註冊為自訂詞彙的術語時, 請只選擇不熟悉應用程式的人員可能會誤解的詞彙。 絕對不要註冊常見的詞彙, 例如「我的健身」或「我的專輯」。 例如, MonkeyChat 應用程式將會註冊與使用者通訊錄中每個連絡人相關聯的昵稱。

應用程式會藉由呼叫`SetVocabularyStrings` `INVocabulary` `NSOrderedSet`類別的方法, 並從主要應用程式傳入集合, 來提供使用者特定的詞彙。 應用程式應該一律先呼叫`RemoveAllVocabularyStrings`方法, 以移除任何現有的詞彙, 然後再加入新的字詞。 例如：

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Foundation;
using Intents;

namespace MonkeyChatCommon
{
    public class MonkeyAddressBook : NSObject
    {
        #region Computed Properties
        public List<MonkeyContact> Contacts { get; set; } = new List<MonkeyContact> ();
        #endregion

        #region Constructors
        public MonkeyAddressBook ()
        {
        }
        #endregion

        #region Public Methods
        public NSOrderedSet<NSString> ContactNicknames ()
        {
            var nicknames = new NSMutableOrderedSet<NSString> ();

            // Sort contacts by the last time used
            var query = Contacts.OrderBy (contact => contact.LastCalledOn);

            // Assemble ordered list of nicknames by most used to least
            foreach (MonkeyContact contact in query) {
                nicknames.Add (new NSString (contact.ScreenName));
            }

            // Return names
            return new NSOrderedSet<NSString> (nicknames.AsSet ());
        }

        // This method MUST only be called on a background thread!
        public void UpdateUserSpecificVocabulary ()
        {
            // Clear any existing vocabulary
            INVocabulary.SharedVocabulary.RemoveAllVocabularyStrings ();

            // Register new vocabulary
            INVocabulary.SharedVocabulary.SetVocabularyStrings (ContactNicknames (), INVocabularyStringType.ContactName);
        }
        #endregion
    }
}
```

當此程式碼準備就緒時, 可能會呼叫如下:

```csharp
using System;
using System.Threading;
using UIKit;
using MonkeyChatCommon;
using Intents;

namespace MonkeyChat
{
    public partial class ViewController : UIViewController
    {
        #region AppDelegate Access
        public AppDelegate ThisApp {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do we have access to Siri?
            if (INPreferences.SiriAuthorizationStatus == INSiriAuthorizationStatus.Authorized) {
                // Yes, update Siri's vocabulary
                new Thread (() => {
                    Thread.CurrentThread.IsBackground = true;
                    ThisApp.AddressBook.UpdateUserSpecificVocabulary ();
                }).Start ();
            }
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

> [!IMPORTANT]
> Siri 會將自訂詞彙視為提示, 並盡可能納入最多的術語。 不過, 自訂詞彙的空間有限, 因此請務必_只_註冊可能會造成混淆的術語, 因而將已註冊的詞彙總數保持在最少。

如需詳細資訊, 請參閱我們的[使用者特定詞彙參考](~/ios/platform/sirikit/understanding-sirikit.md)和 Apple 的[指定自訂詞彙參考](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)。

### <a name="adding-app-specific-vocabulary"></a>新增應用程式特定詞彙

應用程式特定詞彙會定義所有應用程式使用者都知道的特定單字和片語, 例如車輛類型或健身名稱。 因為這些是應用程式的一部分, 所以會在檔案中`AppIntentVocabulary.plist`定義為主要應用程式套件組合的一部分。 此外, 應將這些單字和片語當地語系化。

應用程式特定詞彙必須屬於下列其中一個類別:

- 選擇。
- 健身名稱。

應用程式特定詞彙檔案包含兩個根層級索引鍵:

- `ParameterVocabularies`**必要**-定義應用程式適用的自訂字詞和意圖參數。
- `IntentPhrases`**選擇性**-包含使用中`ParameterVocabularies`定義之自訂詞彙的範例片語。

中的`ParameterVocabularies`每個專案都必須指定識別碼字串、詞彙和詞彙適用的意圖。 此外, 單一詞彙可能適用于多個意圖。

如需可接受的值和必要檔案結構的完整清單, 請參閱 Apple 的[應用程式詞彙檔案格式參考](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)。

若要將`AppIntentVocabulary.plist`檔案新增至應用程式專案, 請執行下列動作:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 在   > **方案總管**中的專案名稱上按一下滑鼠右鍵, 然後選取 **新增檔案 ...** 。 >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "新增屬性清單")](implementing-sirikit-images/plist01.png#lightbox)
2. 按兩下 **方案總管**中`AppIntentVocabulary.plist`的檔案以開啟它進行編輯。
3. `ParameterVocabularies` `Array`按一下以加入索引鍵, 將名稱設為, 並將類型設定為: **+**

    [![](implementing-sirikit-images/plist02.png "將 [名稱] 設為 ParameterVocabularies, 並將類型設定為 [陣列]")](implementing-sirikit-images/plist02.png#lightbox)
4. 展開`ParameterVocabularies`並按一下 [ **+** ] 按鈕, 並  將類型`Dictionary`設定為:

    [![](implementing-sirikit-images/plist03.png "將 [類型] 設定為 [字典]")](implementing-sirikit-images/plist03.png#lightbox)
5. 按一下 **+** 若要加入新的金鑰，請設定 **名稱** 來 `ParameterNames` 而 **類型** 到 `Array`:

    [![](implementing-sirikit-images/plist04.png "將 [名稱] 設為 ParameterNames, 並將類型設定為 [陣列]")](implementing-sirikit-images/plist04.png#lightbox)
6. 按一下以加入具有`String`類型的新機碼, 並使用值做為其中一個可用的參數名稱。 **+** `INStartWorkoutIntent.workoutName`例如:

    [![](implementing-sirikit-images/plist05.png "INStartWorkoutIntent. workoutName 鍵")](implementing-sirikit-images/plist05.png#lightbox)
7. 將金鑰新增`ParameterVocabularies`至**類型**為的`Array`索引鍵: `ParameterVocabulary`

    [![](implementing-sirikit-images/plist06.png "將 ParameterVocabulary 索引鍵新增至具有陣列類型的 ParameterVocabularies 索引鍵")](implementing-sirikit-images/plist06.png#lightbox)
8. 新增型別為的  `Dictionary`新機碼:

    [![](implementing-sirikit-images/plist07.png "加入具有字典類型的新機碼")](implementing-sirikit-images/plist07.png#lightbox)
9. 新增具有**類型**的`String`索引鍵,並指定詞彙的唯一識別碼:`VocabularyItemIdentifier`

    [![](implementing-sirikit-images/plist08.png "新增具有字串類型的 VocabularyItemIdentifier 索引鍵, 並指定唯一的識別碼")](implementing-sirikit-images/plist08.png#lightbox)
10. 新增類型為的`Array`索引  鍵:`VocabularyItemSynonyms`

    [![](implementing-sirikit-images/plist09.png "新增具有陣列類型的 VocabularyItemSynonyms 索引鍵")](implementing-sirikit-images/plist09.png#lightbox)
11. 新增型別為的  `Dictionary`新機碼:

    [![](implementing-sirikit-images/plist10.png "加入具有字典類型的新機碼")](implementing-sirikit-images/plist10.png#lightbox)
12. 新增具有**類型**的`String`索引鍵,以及應用程式所定義的詞彙:`VocabularyItemPhrase`

    [![](implementing-sirikit-images/plist11.png "新增具有字串類型的 VocabularyItemPhrase 索引鍵, 以及應用程式所定義的詞彙")](implementing-sirikit-images/plist11.png#lightbox)
13. 新增具有**類型**的`String`索引鍵,以及詞彙的語音發音:`VocabularyItemPronunciation`

    [![](implementing-sirikit-images/plist12.png "新增具有字串類型的 VocabularyItemPronunciation 索引鍵, 以及詞彙的語音發音")](implementing-sirikit-images/plist12.png#lightbox)
14. 新增類型為的`Array`索引  鍵:`VocabularyItemExamples`

    [![](implementing-sirikit-images/plist13.png "新增具有陣列類型的 VocabularyItemExamples 索引鍵")](implementing-sirikit-images/plist13.png#lightbox)
15. 新增一些`String`索引鍵, 其中包含下列詞彙的範例用法:

    [![](implementing-sirikit-images/plist14.png "使用詞彙的範例用法來新增一些字串索引鍵")](implementing-sirikit-images/plist14.png#lightbox)
16. 針對應用程式需要定義的任何其他自訂詞彙, 重複上述步驟。
17. 折迭索引鍵。 `ParameterVocabularies`
18. 新增類型為的`Array`索引  鍵:`IntentPhrases`

    [![](implementing-sirikit-images/plist15.png "新增具有陣列類型的 IntentPhrases 索引鍵")](implementing-sirikit-images/plist15.png#lightbox)
19. 新增型別為的  `Dictionary`新機碼:

    [![](implementing-sirikit-images/plist16.png "加入具有字典類型的新機碼")](implementing-sirikit-images/plist16.png#lightbox)
20. 在範例中新增具有  `String`和意圖類型的索引鍵:`IntentName`

    [![](implementing-sirikit-images/plist17.png "在範例中新增具有字串類型和意圖的 IntentName 索引鍵")](implementing-sirikit-images/plist17.png#lightbox)
21. 新增類型為的`Array`索引  鍵:`IntentExamples`

    [![](implementing-sirikit-images/plist18.png "新增具有陣列類型的 IntentExamples 索引鍵")](implementing-sirikit-images/plist18.png#lightbox)
22. 新增一些`String`索引鍵, 其中包含下列詞彙的範例用法:

    [![](implementing-sirikit-images/plist19.png "使用詞彙的範例用法來新增一些字串索引鍵")](implementing-sirikit-images/plist19.png#lightbox)
23. 針對應用程式所需的任何意圖, 重複上述步驟, 以提供的範例使用方式。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 以滑鼠右鍵按一下**方案總管**中的專案名稱, 然後選取 [**加入 > 新增專案]。> Apple > 屬性清單 > Info. plist**:

    [![](implementing-sirikit-images/plist01.w157-sml.png "加入新的資訊。 plist")](implementing-sirikit-images/plist01.w157.png#lightbox)

2. 按兩下 **方案總管**中`AppIntentVocabulary.plist`的檔案以開啟它進行編輯。
3. `ParameterVocabularies` `Array`按一下以加入索引鍵, 將名稱設為, 並將類型設定為: **+**

    [![](implementing-sirikit-images/plist02w.png "將 [名稱] 設為 ParameterVocabularies, 並將類型設定為 [陣列]")](implementing-sirikit-images/plist02w.png#lightbox)
4. 展開`ParameterVocabularies`並按一下 [ **+** ] 按鈕, 並  將類型`Dictionary`設定為:

    [![](implementing-sirikit-images/plist03w.png "將 [類型] 設定為 [字典]")](implementing-sirikit-images/plist03w.png#lightbox)
5. 按一下 **+** 若要加入新的金鑰，請設定 **名稱** 來 `ParameterNames` 而 **類型** 到 `Array`:

    [![](implementing-sirikit-images/plist04w.png "將 [名稱] 設為 ParameterNames, 並將類型設定為 [陣列]")](implementing-sirikit-images/plist04w.png#lightbox)
6. 按一下以加入具有`String`類型的新機碼, 並使用值做為其中一個可用的參數名稱。 **+** `INStartWorkoutIntent.workoutName`例如:

    [![](implementing-sirikit-images/plist05w.png "INStartWorkoutIntent. workoutName 鍵")](implementing-sirikit-images/plist05w.png#lightbox)
7. 將金鑰新增`ParameterVocabularies`至**類型**為的`Array`索引鍵: `ParameterVocabulary`

    [![](implementing-sirikit-images/plist06w.png "將 ParameterVocabulary 索引鍵新增至具有陣列類型的 ParameterVocabularies 索引鍵")](implementing-sirikit-images/plist06w.png#lightbox)
8. 新增型別為的  `Dictionary`新機碼:

    [![](implementing-sirikit-images/plist07w.png "加入具有字典類型的新機碼")](implementing-sirikit-images/plist07w.png#lightbox)
9. 新增具有**類型**的`String`索引鍵,並指定詞彙的唯一識別碼:`VocabularyItemIdentifier`

    [![](implementing-sirikit-images/plist08w.png "新增具有字串類型的 VocabularyItemIdentifier 索引鍵, 並指定詞彙的唯一識別碼")](implementing-sirikit-images/plist08w.png#lightbox)
10. 新增類型為的`Array`索引  鍵:`VocabularyItemSynonyms`

    [![](implementing-sirikit-images/plist09w.png "新增具有陣列類型的 VocabularyItemSynonyms 索引鍵")](implementing-sirikit-images/plist09w.png#lightbox)
11. 新增型別為的  `Dictionary`新機碼:

    [![](implementing-sirikit-images/plist10w.png "加入具有字典類型的新機碼")](implementing-sirikit-images/plist10w.png#lightbox)
12. 新增具有**類型**的`String`索引鍵,以及應用程式所定義的詞彙:`VocabularyItemPhrase`

    [![](implementing-sirikit-images/plist11w.png "新增具有字串類型的 VocabularyItemPhrase 索引鍵, 以及應用程式所定義的詞彙")](implementing-sirikit-images/plist11w.png#lightbox)
13. 新增具有**類型**的`String`索引鍵,以及詞彙的語音發音:`VocabularyItemPronunciation`

    [![](implementing-sirikit-images/plist12w.png "新增具有字串類型的 VocabularyItemPronunciation 索引鍵, 以及詞彙的語音發音")](implementing-sirikit-images/plist12w.png#lightbox)
14. 新增類型為的`Array`索引  鍵:`VocabularyItemExamples`

    [![](implementing-sirikit-images/plist13w.png "新增具有陣列類型的 VocabularyItemExamples 索引鍵")](implementing-sirikit-images/plist13w.png#lightbox)
15. 新增一些`String`索引鍵, 其中包含下列詞彙的範例用法:

    [![](implementing-sirikit-images/plist14w.png "使用詞彙的範例用法來新增一些字串索引鍵")](implementing-sirikit-images/plist14w.png#lightbox)
16. 針對應用程式需要定義的任何其他自訂詞彙, 重複上述步驟。
17. 折迭索引鍵。 `ParameterVocabularies`
18. 新增類型為的`Array`索引  鍵:`IntentPhrases`

    [![](implementing-sirikit-images/plist15w.png "新增具有陣列類型的 IntentPhrases 索引鍵")](implementing-sirikit-images/plist15w.png#lightbox)
19. 新增型別為的  `Dictionary`新機碼:

    [![](implementing-sirikit-images/plist16w.png "加入具有字典類型的新機碼")](implementing-sirikit-images/plist16w.png#lightbox)
20. 在範例中新增具有  `String`和意圖類型的索引鍵:`IntentName`

    [![](implementing-sirikit-images/plist17w.png "在範例中新增具有字串類型和意圖的 IntentName 索引鍵")](implementing-sirikit-images/plist17w.png#lightbox)
21. 新增類型為的`Array`索引  鍵:`IntentExamples`

    [![](implementing-sirikit-images/plist18w.png "新增具有陣列類型的 IntentExamples 索引鍵")](implementing-sirikit-images/plist18w.png#lightbox)
22. 新增一些`String`索引鍵, 其中包含下列詞彙的範例用法:

    [![](implementing-sirikit-images/plist19w.png "使用詞彙的範例用法來新增一些字串索引鍵")](implementing-sirikit-images/plist19w.png#lightbox)
23. 針對應用程式所需的任何意圖, 重複上述步驟, 以提供的範例使用方式。

-----

> [!IMPORTANT]
> 在開發期間,將會在測試裝置上向Siri註冊,而且可能需要一些時間,Siri才能納入自訂詞彙。`AppIntentVocabulary.plist` 因此, 測試人員必須等待數分鐘, 才能嘗試測試應用程式特定詞彙 (當它已更新)。

如需詳細資訊, 請參閱我們的[應用程式特定詞彙參考](~/ios/platform/sirikit/understanding-sirikit.md)和 Apple 的[指定自訂詞彙參考](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1)。

## <a name="adding-an-intents-extension"></a>新增意圖延伸模組

現在應用程式已準備好採用 SiriKit, 開發人員必須在解決方案中新增一 (或多個) 意圖延伸模組, 以處理 Siri 整合所需的意圖。

針對每個需要的意圖延伸模組, 請執行下列動作:

- 將意圖擴充功能專案新增至 Xamarin iOS 應用程式解決方案。
- 設定意圖延伸`Info.plist`模組檔案。
- 修改意圖延伸的主要類別。

如需詳細資訊, 請參閱我們[的意圖延伸模組參考](~/ios/platform/sirikit/understanding-sirikit.md)和 Apple[建立意圖延伸模組參考](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1)。

### <a name="creating-the-extension"></a>建立擴充功能

若要在解決方案中新增意圖延伸模組, 請執行下列動作:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  > 以滑鼠右鍵按一下**Solution Pad**中的**方案名稱**, 然後選取 [新增] [**新增專案**]。
2. 從對話方塊中選取 [ **iOS**  > **擴充** > 功能] [**意圖延伸**], 然後按 [**下一步]** 按鈕: 

    [![](implementing-sirikit-images/intents05.png "選取意圖延伸模組")](implementing-sirikit-images/intents05.png#lightbox)
3. 接下來, 輸入意圖延伸的**名稱**, 然後按 [**下一步]** 按鈕: 

    [![](implementing-sirikit-images/intents06.png "輸入意圖延伸模組的名稱")](implementing-sirikit-images/intents06.png#lightbox)
4. 最後, 按一下 [**建立**] 按鈕, 將意圖延伸新增至應用程式解決方案: 

    [![](implementing-sirikit-images/intents07.png "將意圖延伸新增至應用程式解決方案")](implementing-sirikit-images/intents07.png#lightbox)
5. 在 **方案總管**中, 以滑鼠右鍵按一下新建立之意圖延伸模組的 **參考** 資料夾。 檢查通用共用程式碼程式庫專案 (如上所建立的應用程式) 的名稱, 然後按一下 [**確定]** 按鈕: 

    [![](implementing-sirikit-images/intents08.png "選取通用共用程式碼程式庫專案的名稱")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  > 以滑鼠右鍵按一下**方案總管**中的**方案名稱**, 然後選取 [新增] [**新增專案**]。
2. 從對話方塊中, 選取 **[ C# Visual > iOS 擴充功能 > 意圖延伸**], 然後按 [**下一步]** 按鈕:

    [![](implementing-sirikit-images/intents05.w157-sml.png "選取意圖延伸模組")](implementing-sirikit-images/intents05.w157.png#lightbox)
3. 接下來, 輸入意圖延伸的**名稱**, 然後按一下 [**確定]** 按鈕。
4. 在 **方案總管**中, 以滑鼠右鍵按一下新建立之意圖延伸模組的 **參考** 資料夾, 然後選擇 **加入 > 參考**。 檢查通用共用程式碼程式庫專案 (如上所建立的應用程式) 的名稱, 然後按一下 [**確定]** 按鈕:

    [![](implementing-sirikit-images/intents08w.png "選取通用共用程式碼程式庫專案的名稱")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

針對應用程式將會需要的意圖延伸數 (根據上述[的應用程式架構](#architecting-the-app-for-extensions)), 重複這些步驟。

### <a name="configuring-the-infoplist"></a>正在設定資訊。 plist

針對已新增至應用程式解決方案的每個意圖延伸模組, 必須在檔案中`Info.plist`設定, 才能與應用程式搭配使用。

就像任何一般的應用程式擴充功能一樣, 應用程式也會`NSExtension`有`NSExtensionAttributes`和的現有金鑰。 意圖擴充有兩個必須設定的新屬性:

[![](implementing-sirikit-images/intents01.png "必須設定的兩個新屬性")](implementing-sirikit-images/intents01.png#lightbox)

- **IntentsSupported** -是必要的, 其中包含應用程式想要從意圖延伸模組支援的意圖類別名稱陣列。
- **IntentsRestrictedWhileLocked** -是選擇性的索引鍵, 可讓應用程式指定延伸模組的鎖定畫面行為。 其中包含意圖類別名稱陣列, 應用程式想要要求使用者登入, 才能從意圖延伸模組使用。

若要設定意圖延伸的`Info.plist`檔案, 請在 **方案總管**中按兩下該檔案以開啟它進行編輯。 接下來, 切換至**來源**視圖, 然後展開`NSExtension`編輯器`NSExtensionAttributes`中的和索引鍵:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents02.png "編輯器中的 NSExtension 和 NSExtensionAttributes 鍵")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents02w.png "編輯器中的 NSExtension 和 NSExtensionAttributes 鍵")](implementing-sirikit-images/intents02w.png#lightbox)

-----

`IntentsSupported`展開金鑰, 並新增此延伸模組將支援之任何意圖類別的名稱。 針對範例 MonkeyChat 應用程式, 它支援`INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents09.png "INSendMessageIntent 鍵")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents09w.png "INSendMessageIntent 鍵")](implementing-sirikit-images/intents09w.png#lightbox)

-----

如果應用程式選擇性地要求使用者登入裝置以使用指定的意圖, 請展開該`IntentRestrictedWhileLocked`金鑰, 並新增具有限制存取之意圖的類別名稱。 針對範例 MonkeyChat 應用程式, 使用者必須登入才能傳送聊天訊息, 因此我們已新增`INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents10.png "新增的 INSendMessageIntent 鍵")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents10w.png "新增的 INSendMessageIntent 鍵")](implementing-sirikit-images/intents10w.png#lightbox)

-----


如需可用意圖網域的完整清單, 請參閱 Apple 的[意圖定義域參考](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)。

### <a name="configuring-the-main-class"></a>設定主要類別

接下來, 開發人員必須將主要類別設定為 Siri 中意圖延伸的主要進入點。 它必須是符合`INExtension` `IINIntentHandler`委派的子類別。 例如：

```csharp
using System;
using System.Collections.Generic;

using Foundation;
using Intents;

namespace MonkeyChatIntents
{
    [Register ("IntentHandler")]
    public class IntentHandler : INExtension, IINSendMessageIntentHandling, IINSearchForMessagesIntentHandling, IINSetMessageAttributeIntentHandling
    {
        #region Constructors
        protected IntentHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override NSObject GetHandler (INIntent intent)
        {
            // This is the default implementation.  If you want different objects to handle different intents,
            // you can override this and return the handler you want for that particular intent.

            return this;
        }
        #endregion
        ...
    }
}
```

應用程式必須在意圖延伸主要類別 ( `GetHandler`方法) 上執行的一個單獨方法。 這個方法會透過 SiriKit 傳遞意圖, 應用程式必須傳回符合給定意圖類型的**意圖處理常式**。

由於範例 MonkeyChat 應用程式只會處理一個意圖, 因此它會在`GetHandler`方法中傳回本身。 如果延伸模組處理了一個以上的意圖, 開發人員就會為每個意圖類型加入一個類別, 並根據傳遞給`Intent`方法的來傳回這裡的實例。

### <a name="handling-the-resolve-stage"></a>處理解析階段

[解析] 階段是意圖延伸模組將會從 Siri 中說明和驗證傳入的參數, 並已透過使用者的交談設定的。

針對從 Siri 傳送的每個參數, 有一個`Resolve`方法。 應用程式必須為每個參數執行此方法, 應用程式可能會需要 Siri 的協助, 以取得使用者的正確答案。

在範例 MonkeyChat 應用程式的案例中, 意圖延伸模組需要一或多個收件者, 才能將訊息傳送至。 針對清單中的每個收件者, 延伸模組必須執行可具有下列結果的連絡人搜尋:

- 只會找到一個相符的連絡人。
- 找到兩個或多個相符的連絡人。
- 找不到相符的連絡人。

此外, MonkeyChat 需要訊息主體的內容。 如果使用者未提供此內容, Siri 就必須提示使用者輸入內容。

意圖延伸模組必須適當地處理每一種情況。


```csharp
[Export ("resolveRecipientsForSearchForMessages:withCompletion:")]
public void ResolveRecipients (INSendMessageIntent intent, Action<INPersonResolutionResult []> completion)
{
    var recipients = intent.Recipients;
    // If no recipients were provided we'll need to prompt for a value.
    if (recipients.Length == 0) {
        completion (new INPersonResolutionResult [] { (INPersonResolutionResult)INPersonResolutionResult.NeedsValue });
        return;
    }

    var resolutionResults = new List<INPersonResolutionResult> ();

    foreach (var recipient in recipients) {
        var matchingContacts = new INPerson [] { recipient }; // Implement your contact matching logic here to create an array of matching contacts
        if (matchingContacts.Length > 1) {
            // We need Siri's help to ask user to pick one from the matches.
            resolutionResults.Add (INPersonResolutionResult.GetDisambiguation (matchingContacts));
        } else if (matchingContacts.Length == 1) {
            // We have exactly one matching contact
            resolutionResults.Add (INPersonResolutionResult.GetSuccess (recipient));
        } else {
            // We have no contacts matching the description provided
            resolutionResults.Add ((INPersonResolutionResult)INPersonResolutionResult.Unsupported);
        }
    }

    completion (resolutionResults.ToArray ());
}

[Export ("resolveContentForSendMessage:withCompletion:")]
public void ResolveContent (INSendMessageIntent intent, Action<INStringResolutionResult> completion)
{
    var text = intent.Content;
    if (!string.IsNullOrEmpty (text))
        completion (INStringResolutionResult.GetSuccess (text));
    else
        completion ((INStringResolutionResult)INStringResolutionResult.NeedsValue);
}
```

如需詳細資訊, 請參閱我們[的解決階段參考](~/ios/platform/sirikit/understanding-sirikit.md)和 Apple 的[解決和處理意圖參考](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1)。

### <a name="handling-the-confirm-stage"></a>處理確認階段

[確認] 階段是意圖延伸模組檢查是否有所有資訊可滿足使用者的要求。 應用程式想要傳送確認資訊, 將會 Siri 即將發生的所有支援詳細資料, 以便向使用者確認這是預期的動作。

```csharp
[Export ("confirmSendMessage:completion:")]
public void ConfirmSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Verify user is authenticated and the app is ready to send a message.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Ready, userActivity);
    completion (response);
}
```

如需詳細資訊, 請參閱我們[的確認階段參考](~/ios/platform/sirikit/understanding-sirikit.md)。

### <a name="processing-the-intent"></a>處理意圖

這就是意圖延伸模組實際執行工作來完成使用者要求, 並將結果傳回到 Siri, 讓使用者可以收到通知的重點。


```csharp
public void HandleSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Implement the application logic to send a message here.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, userActivity);
    completion (response);
}

public void HandleSearchForMessages (INSearchForMessagesIntent intent, Action<INSearchForMessagesIntentResponse> completion)
{
    // Implement the application logic to find a message that matches the information in the intent.
    ...

    var userActivity = new NSUserActivity (nameof (INSearchForMessagesIntent));
    var response = new INSearchForMessagesIntentResponse (INSearchForMessagesIntentResponseCode.Success, userActivity);

    // Initialize with found message's attributes
    var sender = new INPerson (new INPersonHandle ("sarah@example.com", INPersonHandleType.EmailAddress), null, "Sarah", null, null, null);
    var recipient = new INPerson (new INPersonHandle ("+1-415-555-5555", INPersonHandleType.PhoneNumber), null, "John", null, null, null);
    var message = new INMessage ("identifier", "I am so excited about SiriKit!", NSDate.Now, sender, new INPerson [] { recipient });
    response.Messages = new INMessage [] { message };
    completion (response);
}

public void HandleSetMessageAttribute (INSetMessageAttributeIntent intent, Action<INSetMessageAttributeIntentResponse> completion)
{
    // Implement the application logic to set the message attribute here.
    ...

    var userActivity = new NSUserActivity (nameof (INSetMessageAttributeIntent));
    var response = new INSetMessageAttributeIntentResponse (INSetMessageAttributeIntentResponseCode.Success, userActivity);
    completion (response);
}
```

如需詳細資訊, 請參閱我們[的控制碼階段參考](~/ios/platform/sirikit/understanding-sirikit.md)。

## <a name="adding-an-intents-ui-extension"></a>新增意圖 UI 延伸模組

選擇性的意圖 UI 延伸模組, 讓您有機會將應用程式的 UI 和商標帶入 Siri 體驗, 讓使用者感覺能夠連接到應用程式。 應用程式可以使用此延伸模組, 將品牌以及視覺效果和其他資訊帶入文字記錄中。

[![](implementing-sirikit-images/intentsui01.png "範例意圖 UI 延伸模組輸出")](implementing-sirikit-images/intentsui01.png#lightbox)

如同意圖延伸模組, 開發人員會針對意圖 UI 延伸模組執行下列步驟:

- 將意圖 UI 延伸模組專案新增至 Xamarin iOS 應用程式解決方案。
- 設定意圖 UI 延伸`Info.plist`模組檔案。
- 修改意圖 UI 延伸模組的主要類別。

如需詳細資訊, 請參閱我們[的意圖 UI 延伸模組參考](~/ios/platform/sirikit/understanding-sirikit.md)和 Apple[提供自訂介面參考](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1)。

### <a name="creating-the-extension"></a>建立擴充功能

若要在解決方案中新增意圖 UI 延伸模組, 請執行下列動作:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  > 以滑鼠右鍵按一下**Solution Pad**中的**方案名稱**, 然後選取 [新增] [**新增專案**]。
2. 從對話方塊中, 選取 [ **iOS**  > **擴充** > 功能] [**意圖 UI 延伸**模組], 然後按 [**下一步]** 按鈕 

    [![](implementing-sirikit-images/intents11.png "選取意圖 UI 延伸模組")](implementing-sirikit-images/intents11.png#lightbox)
3. 接下來, 輸入意圖延伸的**名稱**, 然後按 [**下一步]** 按鈕: 

    [![](implementing-sirikit-images/intents12.png "輸入意圖延伸模組的名稱")](implementing-sirikit-images/intents12.png#lightbox)
4. 最後, 按一下 [**建立**] 按鈕, 將意圖延伸新增至應用程式解決方案: 

    [![](implementing-sirikit-images/intents13.png "將意圖延伸新增至應用程式解決方案")](implementing-sirikit-images/intents13.png#lightbox)
5. 在 **方案總管**中, 以滑鼠右鍵按一下新建立之意圖延伸模組的 **參考** 資料夾。 檢查通用共用程式碼程式庫專案 (如上所建立的應用程式) 的名稱, 然後按一下 [**確定]** 按鈕: 

    [![](implementing-sirikit-images/intents14.png "選取通用共用程式碼程式庫專案的名稱")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  > 以滑鼠右鍵按一下**方案總管**中的**方案名稱**, 然後選取 [新增] [**新增專案**]。
2. 從對話方塊中選取 [ **iOS**  > **擴充** > 功能] [**意圖 UI 延伸**模組], 然後按 [**下一步]** 按鈕。
3. 接下來, 輸入意圖延伸的**名稱**, 然後按一下 [**確定]** 按鈕。
4. 在 **方案總管**中, 以滑鼠右鍵按一下新建立之意圖延伸模組的 **參考** 資料夾。 檢查通用共用程式碼程式庫專案 (如上所建立的應用程式) 的名稱, 然後按一下 [**確定]** 按鈕。
    
-----

### <a name="configuring-the-infoplist"></a>正在設定資訊。 plist

設定意圖 UI 延伸模組的`Info.plist`檔案, 以使用應用程式。

就像任何一般的應用程式擴充功能一樣, 應用程式也會`NSExtension`有`NSExtensionAttributes`和的現有金鑰。 針對意圖延伸模組, 有一個必須設定的新屬性:

[![](implementing-sirikit-images/intents03.png "必須設定的一個新屬性")](implementing-sirikit-images/intents03.png#lightbox)

**IntentsSupported**是必要的, 其中包含應用程式想要從意圖延伸模組支援的意圖類別名稱陣列。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

若要設定意圖 UI 延伸模組`Info.plist`的檔案, 請在**方案總管**中按兩下該檔案以開啟它進行編輯。 接下來, 切換至**來源**視圖, 然後展開`NSExtension`編輯器`NSExtensionAttributes`中的和索引鍵:

[![](implementing-sirikit-images/intents04.png "編輯器中的 NSExtension 和 NSExtensionAttributes 鍵")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

若要設定意圖 UI 延伸模組`Info.plist`的檔案, 請在**方案總管**中按兩下該檔案以開啟它進行編輯。 展開編輯器`NSExtension`中`NSExtensionAttributes`的和索引鍵:

[![](implementing-sirikit-images/intents04w.png "在編輯器中 NSExtension 和 NSExtensionAttributes 索引鍵")](implementing-sirikit-images/intents04w.png#lightbox)

-----

`IntentsSupported`展開金鑰, 並新增此延伸模組將支援之任何意圖類別的名稱。 針對範例 MonkeyChat 應用程式, 它支援`INSendMessageIntent`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](implementing-sirikit-images/intents15.png "INSendMessageIntent 鍵")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](implementing-sirikit-images/intents15w.png "INSendMessageIntent 鍵")](implementing-sirikit-images/intents15w.png#lightbox)

-----

如需可用意圖網域的完整清單, 請參閱 Apple 的[意圖定義域參考](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)。

### <a name="configuring-the-main-class"></a>設定主要類別

將作為意圖 UI 延伸模組主要進入點的主要類別設定為 Siri。 它必須是符合`UIViewController` `IINUIHostedViewController`介面的子類別。 例如：

```csharp
using System;
using Foundation;
using CoreGraphics;
using Intents;
using IntentsUI;
using UIKit;

namespace MonkeyChatIntentsUI
{
    public partial class IntentViewController : UIViewController, IINUIHostedViewControlling
    {
        #region Constructors
        protected IntentViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }

        public override void DidReceiveMemoryWarning ()
        {
            // Releases the view if it doesn't have a superview.
            base.DidReceiveMemoryWarning ();

            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Public Methods
        [Export ("configureWithInteraction:context:completion:")]
        public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
        {
            // Do configuration here, including preparing views and calculating a desired size for presentation.

            if (completion != null)
                completion (DesiredSize ());
        }

        [Export ("desiredSize:")]
        public CGSize DesiredSize ()
        {
            return ExtensionContext.GetHostedViewMaximumAllowedSize ();
        }
        #endregion
    }
}
```

Siri 會將`INInteraction`類別實例傳遞`Configure`至意圖 UI 延伸模組`UIViewController`內實例的方法。

`INInteraction`物件提供三個重要的資訊片段給延伸模組:

1. 正在處理的意圖物件。
2. 來自`Confirm`意圖延伸之和`Handle`方法的意圖回應物件。
3. 定義應用程式與 Siri 之間互動狀態的互動狀態。

實例是與 Siri 互動的準則類別, 因為它繼承自`UIViewController`, 所以可以存取 UIKit 的所有功能。 `UIViewController`

當 Siri 呼叫`Configure` `UIViewController`它的方法時, 它會傳入 view 內容, 指出 view 控制器將會裝載于 Siri Snippit 或 Maps 介面卡中。

Siri 也會傳入完成處理常式, 應用程式必須在應用程式完成設定後, 傳回所需的視圖大小。

### <a name="design-the-ui-in-ios-designer"></a>在 iOS 設計工具中設計 UI

在 iOS 設計工具中版面配置意圖 UI 延伸模組的使用者介面。 按兩下 方案總管中的延伸`MainInterface.storyboard`模組檔案,將它開啟以供編輯。 拖曳所有必要的 UI 元素, 以建立使用者介面並儲存變更。

> [!IMPORTANT]
> 雖然您可以將互動式專案 (例如`UIButtons`或`UITextFields` ) 新增至意圖 ui 延伸`UIViewController`模組, 但這些專案在非互動中會受到嚴格禁止, 而使用者將無法與他們互動。

### <a name="wire-up-the-user-interface"></a>連接使用者介面

使用在 iOS 設計工具中建立的意圖 UI 延伸模組的使用者介面`UIViewController` , 編輯子類別`Configure`並覆寫方法, 如下所示:

```csharp
[Export ("configureWithInteraction:context:completion:")]
public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
{
    // Do configuration here, including preparing views and calculating a desired size for presentation.
    ...

    // Return desired size
    if (completion != null)
        completion (DesiredSize ());
}

[Export ("desiredSize:")]
public CGSize DesiredSize ()
{
    return ExtensionContext.GetHostedViewMaximumAllowedSize ();
}
```

### <a name="overriding-the-default-siri-ui"></a>覆寫預設的 Siri UI

意圖 UI 延伸一律會與其他 Siri 內容一起顯示, 例如應用程式圖示和 UI 頂端的名稱, 或是根據意圖, 按鈕 (例如 [傳送] 或 [取消]) 可能會顯示在底部。

有幾個實例, 應用程式預設會將 Siri 顯示的資訊取代為使用者 (例如訊息或對應), 應用程式可以在其中使用針對應用程式量身打造的預設體驗來加以取代。

如果意圖 UI 延伸模組需要覆寫預設 Siri UI 的專案, `UIViewController`子類別就必須`IINUIHostedViewSiriProviding`執行介面, 並加入宣告以顯示特定的介面元素。

將下列程式碼新增至`UIViewController`子類別, 以告知 Siri 意圖 UI 延伸已經顯示訊息內容:

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>考量

Apple 建議開發人員在設計和實施意圖 UI 延伸模組時, 考慮下列事項:

- 請**關注記憶體使用量**-因為意圖 UI 延伸模組是暫時性的, 而且只會在短時間內顯示, 所以系統會強加較緊密的記憶體條件約束, 而不是與完整應用程式搭配使用。
- **考慮最小和最大視圖大小**-確保意圖 UI 延伸模組在每個 iOS 裝置類型、大小和方向上都看起來良好。 此外, 應用程式傳回給 Siri 的所需大小可能無法被授與。
- 再次**使用彈性且**可調整的版面配置模式, 以確保 UI 在每個裝置上看起來都很棒。

## <a name="summary"></a>總結

本文涵蓋 SiriKit, 並示範如何將其新增至 Xamarin iOS 應用程式, 以提供使用者可使用 Siri 和 iOS 裝置上的 Maps 應用程式來存取的服務。




## <a name="related-links"></a>相關連結

- [ElizaChat 範例](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-elizachat)
- [SiriKit 程式設計指南](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [意圖架構參考](https://developer.apple.com/reference/intents)
- [意圖 UI 架構參考](https://developer.apple.com/reference/intentsui)
- [意圖網域參考](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
