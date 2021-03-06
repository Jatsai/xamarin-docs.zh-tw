---
title: 了解 SiriKit 概念
description: 本檔說明在 Xamarin iOS 應用程式中使用 SiriKit 所需的重要概念。 例如, 它會討論意圖和意圖 UI 延伸、SiriKit 許可權、設計絕佳經驗等等。
ms.prod: xamarin
ms.assetid: 99EC5C1E-484F-4371-8555-58C9F60DE37F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2017
ms.openlocfilehash: b9e8ad848204f7db785327093cd4b7ed9aa6de81
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654063"
---
# <a name="understanding-sirikit-concepts"></a>了解 SiriKit 概念

_本文涵蓋在 Xamarin iOS 應用程式中使用 SiriKit 時所需的重要概念。_


SiriKit 是 iOS 10 的新手, 可讓 Xamarin iOS 應用程式提供使用者可使用 Siri 和 iOS 裝置上的 Maps 應用程式來存取的服務。 這項功能是使用新的**意圖**和**意圖 UI**架構, 在一或多個應用程式延伸模組中提供。

SiriKit 可讓 iOS 應用程式在 iOS 裝置上使用 Siri 和 Maps 應用程式, 提供可供使用者存取的服務, 並使用應用程式延伸模組和新的**意圖**和**意圖 UI**架構。

Siri 適用于**網域**的概念、相關工作的已知動作群組。 應用程式與 Siri 之間的每個互動都必須屬於其中一個已知的服務網域, 如下所示:

- 音訊或影片通話。
- 預約。
- 管理 workouts。
- 關鍵.
- 正在搜尋相片。
- 傳送或接收付款。

當使用者提出包含其中一個應用程式延伸模組服務的 Siri 要求時, SiriKit 會將描述使用者要求的**意圖**物件以及任何支援的資料傳送給此延伸模組。 然後, 應用程式延伸模組會針對指定的**意圖**產生適當的**回應**物件, 詳述延伸模組如何處理要求。

## <a name="the-intents-and-intents-ui-extensions"></a>意圖和意圖 UI 延伸模組

Siri 和 Maps 應用程式會透過兩種不同類型的應用程式擴充功能, 與應用程式的服務進行互動:

- **意圖延伸**模組-提供 Siri 和對應應用程式的內容, 並執行滿足任何支援意圖所需的工作。
- **意圖 UI 延伸**模組-提供自訂 UI, 以顯示應用程式在 Siri 或 Maps 內的內容。

應用程式必須提供意圖延伸模組來支援 SiriKit, 而且它會負責提供 Siri 和對應可呈現給使用者的資訊, 以及處理意圖。

建立意圖 UI 延伸模組是選擇性的, 因為 Siri 通常會處理所有使用者互動, 而且具有標準的內建 UI, 可在每個支援的網域中呈現資訊。 藉由提供意圖 UI 延伸模組, 應用程式可以使用**意圖 ui**架構來呈現豐富的自訂使用者介面, 其中包含應用程式的商標和其他資訊。

## <a name="siri-and-the-maps-app-role"></a>Siri 和 Maps 應用程式角色

使用者的語音要求是由 Siri 處理的語言, 並以語義方式分析, 這會將這些要求轉換成意圖延伸模組可以處理的可操作意圖。

對應會使用應用程式的意圖延伸模組, 在對應介面中顯示資訊以回應使用者的動作。 例如, 要求附近餐廳或取得應用程式的餐廳評論。

Siri 和 Maps 都會使用標準系統介面來管理使用者的所有互動, 並顯示結果。 [應用程式延伸模組] 角色主要是用來提供要顯示的資料。 (選擇性) 應用程式可以提供意圖 UI 延伸模組, 並顯示自訂 UI 以增強預設系統介面。

## <a name="interacting-with-siri-via-sirikit"></a>透過 SiriKit 與 Siri 互動

本節將概述 SiriKit 如何讓使用者使用 Siri 與應用程式互動。 基於此範例的目的, 我們將使用假的 MonkeyChat 應用程式:

[![](understanding-sirikit-images/monkeychat01.png "MonkeyChat 圖示")](understanding-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat 會保留自己的使用者朋友的連絡人書籍, 每個都與一個螢幕名稱相關聯 (例如 Bobo), 並允許使用者透過其螢幕名稱將文字聊天傳送給每位朋友。

使用者可能會起始與應用程式互動的方式有很多種, 因為不同的人可能會在許多不同的表單中提出相同的要求。

例如, 如果使用者想要將訊息傳送至其 friend Bobo, 他們可能會與 Siri 進行下列交談:

_使用者嘿, Siri, 傳送 MonkeyChat 訊息。_<br />
_Siri對誰？_<br />
_使用者Bobo._<br />
_Siri您要 Bobo 什麼？_<br />
_使用者請傳送更多香蕉。_<br />

另一個人可能會使用不同的交談提出相同的要求:

_使用者在 MonkeyChat 上將訊息傳送至 Bobo。_<br />
_Siri您要 Bobo 什麼？_<br />
_使用者請傳送更多香蕉。_<br />

而另一位使用者可能會提出更短的要求:

_使用者MonkeyChat Bobo 請傳送更多香蕉。_<br />
_Siri好, 傳送訊息, 請在 Monkeychat 上傳送更多香蕉給 Bobo。_<br />

或甚至是以不同的語言提出相同的要求:

_使用者MonkeyChat Bobo s'il vous plaît envoyer 加上 de bananes。_<br />
_SiriOui、envoi message s'il vous plaît envoyer 加 de bananes à Bobo sur Monkeychat。_<br />

但另一位使用者在交談中可能非常詳細:

_使用者嘿, 您好, Siri, 可以啟動 MonkeyChat 應用程式來傳送文字, 其中包含「請傳送更多香蕉」訊息。_<br />
_Siri對誰？_<br />
_使用者我的最佳 pal Bobo。_<br />

此外, Siri 可能會根據要求的方式來回應要求, 其中有許多方法:

- 藉**由按住 [首頁] 按鈕**, Siri 會提供更多視覺回應, 其中包含有限的口頭意見反應。
- 「**嗨, Siri** 」-Siri 會更具語言, 並提供較少的視覺回應。

Siri 也會進行微調, 以符合使用者的協助工具需求, 並且會根據這些需求進行互動和回應。

無論提出要求的方式或 Siri 如何回應要求, Siri 都會處理與使用者的交談, 以及應用程式 (透過其延伸模組) 提供的功能。

當使用者提出 Siri 的口頭要求時, 以下是 Siri 將遵循的步驟:

[![](understanding-sirikit-images/monkeychat02.png "Siri 將遵循的步驟")](understanding-sirikit-images/monkeychat02.png#lightbox)

1. 首先, Siri 會採用使用者**語音**的音訊, 並將其轉換成文字。
2. 接下來, 文字會轉換成**意圖**, 也就是使用者要求的結構化標記法。
3. 根據意圖, Siri 會採取**動作**來執行使用者的要求。
4. 最後, Siri 會根據所採取的動作, 向使用者顯示**回應**(視覺效果和口頭)。

應用程式有三種主要方式可以參與使用者與 Siri 的對話:

[![](understanding-sirikit-images/monkeychat03.png "應用程式可以使用 Siri 參與使用者對話的三種主要方式")](understanding-sirikit-images/monkeychat03.png#lightbox)

1. **詞彙**-這是應用程式告訴 Siri 必須知道的字詞來與它互動的方式。
2. **應用程式邏輯**-這些是應用程式將根據指定意圖採取的動作和回應。
3. **使用者介面**-這是選擇性的自訂使用者介面, 應用程式可以在其中提供其回應。

### <a name="example"></a>範例

針對上述資訊, 請檢查下列交談如何與 MonkeyChat 應用程式互動:

_使用者嘿, Siri, 將訊息傳送至 MonkeyChat 上的 Bobo。_<br />
_Siri您要 Bobo 什麼？_<br />
_使用者請傳送更多香蕉。_<br />

應用程式在交談中採取的第一個角色是協助 Siri 瞭解使用者的語音:

[![](understanding-sirikit-images/monkeychat04.png "協助 Siri 瞭解使用者的語音")](understanding-sirikit-images/monkeychat04.png#lightbox)

Siri 在其資料庫中的名稱不是 "Bobo", 但應用程式會透過其詞彙與 Siri 共用這項資訊。 應用程式也可協助 Siri 辨識 Bobo 為收件者, 因為它將其指定為 Siri 為*連絡人*。

Siri 知道除了收件者以外, 需要更多的訊息, 因此它會快速檢查應用程式延伸, 以查看訊息是否需要內容。 由於 MonkeyChat, Siri 會回應具有下列問題的使用者: *「您要 Bobo 什麼？」*

在上述範例中, 使用者已回應「*請傳送更多香蕉*」, Siri 將會組合成結構化的**意圖**:

[![](understanding-sirikit-images/monkeychat05.png "Siri 會將使用者的回應組合成結構化的意圖")](understanding-sirikit-images/monkeychat05.png#lightbox)

結構化的意圖將包含下列資訊:

- **Domain**訊息
- **意圖:** sendMessage
- **接收者**Bobo
- **內容：** 請傳送更多香蕉

每個網域都有一組可在其中執行的已知*動作*, 並根據網域和動作, 傳送至應用程式的意圖中可能包含零到多個參數。

然後, 此意圖會傳送至應用程式擴充以進行處理。 由於處理意圖的結果, 應用程式會產生**IntentResponse** , 其會與意圖配套, 並包含描述應用程式對意圖執行之工作的參數。

每個 IntentResponse 也會包含**回應碼**, 告知 Siri 應用程式是否能夠完成要求。 有些網域也有非常特定的錯誤回應碼, 可以傳送。

最後, IntentResponse 會包含 (像`NSUserActivity`是用來支援手中的)。 如果`NSUserActivity`回應要求他們離開 Siri 環境, 並輸入應用程式來完成應用程式, 將會使用此專案來啟動。 

Siri 會自動建立適當`NSUserActivity`的來啟動應用程式, 並收取使用者在 Siri 環境中離開的位置。 不過, 如果需要的話, 應用程式`NSUserActivity`可以提供自己的自訂資訊。

在應用程式處理意圖並將回應傳回給 Siri 之後, 它會向使用者顯示結果 (verbally 和視覺效果):

[![](understanding-sirikit-images/monkeychat06.png "呈現給使用者的結果 verbally 和視覺效果")](understanding-sirikit-images/monkeychat06.png#lightbox)

Siri 有數個內建回應使用者介面, 適用于應用程式可用的每個網域。 不過, 由於 MonkeyChat 已提供選擇性意圖 UI 延伸模組, 因此它會用來向上述範例中的使用者呈現交談結果。

## <a name="the-intent-lifecycle"></a>意圖生命週期

處理意圖時, 應用程式延伸模組需要執行三項主要工作:

[![](understanding-sirikit-images/monkeychat07.png "意圖生命週期")](understanding-sirikit-images/monkeychat07.png#lightbox)

1. 應用程式必須**解析**事件上的每個參數。 因此, 應用程式會呼叫解析多次 (每個參數一次), 有時也會在同一個參數上多次, 直到應用程式和使用者同意要求的內容為止。
2. 應用程式必須**確認**它可以處理要求的意圖, 並告訴 Siri 預期的結果。
3. 最後, 應用程式必須**處理**意圖, 並執行步驟以達成要求的結果。

### <a name="the-resolve-stage"></a>解決階段

[解析] 階段可協助 Siri 瞭解使用者所提供的值, 並確保使用者實際的意義是應用程式處理意圖時所發生的情況。 

這個階段也會讓應用程式在與使用者交談期間, 影響 Siri 行為的機會。 若要這樣做, 應用程式將會提供**解決回應**。 Siri 瞭解的不同資料類型有幾個預先定義的回應。

應用程式最常見的解決方法是**成功**, 這表示應用程式會將參數中的特定資料 (例如使用者畫面名稱) 與它所知道的一段資訊進行比對。

有時候, 應用程式可能需要確認指定的要求符合它所知道的正確資訊片段。 在這些情況下, 它會傳送**ConfirmationRequired**回應, 對使用者提出「是」或「否」的問題, 例如「*傳送訊息給 Bobo 很棒嗎？* 」

在其他情況下, 應用程式可能會要求使用者從簡短的選項清單中挑選。 在此情況下, 應用程式將會  提供二到十個選項的清單, 讓使用者從中選擇, 如下所示: 

```csharp
Who do you want to message?

* Bobo the Great
* Bobo Jr.
* Little Bobo
```

Siri 會處理進行選取的使用者 (verbally) 或與 Siri UI 互動, 並將結果傳送回應用程式。

在其他情況下, 應用程式可能沒有足夠的資訊來解析參數, 或可能有太多相符專案, 無法使用去除混淆 (例如, 其名稱中有 Bobo 的80使用者) 來解決。 在此情況下, 應用程式會傳送**NeedsMoreDetails**回應, 而 Siri 會提示使用者更明確。

如果使用者未提供處理意圖所需的值, 它可以傳送**NeedsValue**回應, 讓 Siri 提示使用者輸入值。

如果應用程式不支援使用者針對特定參數提供的值, 它可以傳送**UnsupportedWithReason**回應, 以提供不支援此值的原因。 然後, Siri 會提示使用者提供完全新的值, 並將其指定為必要的原因。

最後, 使用**NotRequired**回應來告訴 Siri, 應用程式不需要指定參數的值。 如果使用者仍然提供, 則 Siri 會忽略它。

### <a name="the-confirm-stage"></a>確認階段

確認階段有兩個用途:

- 為了讓 Siri 知道處理意圖的預期結果, 讓 Siri 能夠告訴使用者會發生什麼事。
- 提供一個機會, 檢查應用程式可能需要完成使用者提出的要求, 例如, 在銀行中有足夠的金錢來提出要求的付款。

應用程式將會從確認步驟提供**意圖回應**, 其中應該填入應用程式可使用的資訊, 讓 Siri 可以有效地與使用者溝通。

根據網域和動作類型, Siri 可能會提示使用者進行確認, 例如在傳送付款或預約要求之前。

### <a name="the-handle-stage"></a>控制碼階段

控制碼階段是使用意圖的最重要部分, 因為它是應用程式藉由執行要求執行的工作來完成使用者要求的重點。

就像在確認階段一樣, 應用程式必須盡可能提供結果的相關資訊, 讓 Siri 可以與使用者產生關聯。 有時這種資訊會以視覺化方式呈現, 或在其他時間 Siri, 只會向使用者說出。 

有時候, 應用程式可能需要額外的時間來處理指定的要求, 例如網路呼叫延遲, 或 live person 是否需要履行要求 (如完成訂單或推動汽車到使用者的位置)。 當 Siri 正在等候來自應用程式的回應時, 它會向使用者顯示等待中的 UI, 告訴他們應用程式正在處理要求。

在理想的情況下, 應用程式應該在兩到三秒內, 最多提供 Siri 的回應。 如果應用程式知道給定的回應將需要較長的處理時間, 則必須將**InProgress**回應碼傳送至 Siri。 然後, Siri 會通知使用者, 應用程式會在背景中處理要求, 而且即使離開 Siri 環境仍會繼續執行此動作。

## <a name="adding-sirikit-to-the-app"></a>將 SiriKit 新增至應用程式

在 iOS 10 中使用 SiriKit, Apple 已建立兩個新的擴充點:

- **意圖延伸**模組-提供應用程式內容的 Siri, 並執行滿足任何支援意圖所需的工作。
- **意圖 UI 延伸**模組-提供將針對 Siri 內的應用程式內容顯示的自訂 UI。 

還有一個 API 可提供 Siri 的單字和片語, 以協助辨識的形式如下:

- **應用程式詞彙**-應用程式的每個使用者通用的單字和片語。
- **使用者詞彙**-對特定應用程式使用者而言是唯一的單字和片語。

## <a name="the-intents-extension"></a>意圖延伸模組

意圖延伸模組負責處理應用程式與 Siri 之間的主要互動, 如下所示:

[![](understanding-sirikit-images/intents01.png "意圖延伸模組")](understanding-sirikit-images/intents01.png#lightbox)

意圖延伸模組可支援一或多個意圖, 由開發人員決定要如何在應用程式中執行 SiriKit。 開發人員也可以為每個需要處理的意圖新增不同的意圖延伸模組。  話雖如此, Apple 要求開發人員限制意圖延伸的數目, 讓 Siri 不會對應用程式開啟多個進程, 這需要更多的記憶體和時間來處理。

開發人員也應該注意, 在 Siri 為作用中時, 意圖延伸模組將會在背景中執行。 這可讓 Siri 主動與使用者進行交談, 同時仍會與延伸模組進行通訊, 以處理要求的相關資訊。

## <a name="privacy-and-security-considerations"></a>隱私權和安全性考慮

Apple 已採取絕佳的措施, 確保使用者私人資訊在使用 Siri 時是安全的, 因此, 有數個互動需要使用者登入 iOS 裝置。 例如, 在要求您提出或付款時。

此外, 應用程式可能會想要限制使用者登入裝置的特定行為。 在這些情況下, 應用程式可以要求**限制的鎖定**行為。 這會透過檔案中`Info.plist`的設定來完成。

本機驗證架構適用于意圖延伸, 因此應用程式可以要求使用者提供額外的驗證資訊, 即使裝置已解除鎖定也一樣。

最後, Apple Pay 適用于意圖延伸, 因此應用程式可以使用 Apple Pay 來完成交易, 而內建 Apple Pay 表會出現在 Siri 介面上方。

此外, Apple 想要確保使用者知道何時將資訊傳送至協力廠商應用程式, 因此, 使用者在提出要求時,**必須**以應用程式的特定名稱 (如應用程式套件組合顯示名稱中所指定) 來表示。

Apple 已設計 Siri 來與使用者進行自然、流暢的交談, 因此, 應用程式的套件組合名稱可用於語音的許多部分, 不論其自然在使用者的要求中。

使用者會執行的其中一項常見事項, 就是「verbify」應用程式的名稱, 換句話說, 採用應用程式名稱, 並使用它作為要求中的動詞。 例如, 「 *MonkeyChat Bobo, 這是絕佳的香蕉。* 」

## <a name="the-intents-ui-extension"></a>意圖 UI 延伸模組

意圖 UI 延伸提供機會將應用程式的 UI 和商標帶入 Siri 體驗, 讓使用者感覺能夠連接到應用程式。 透過此延伸模組, 應用程式可以將品牌以及視覺效果和其他資訊帶入文字記錄中。

[![](understanding-sirikit-images/intents02.png "範例意圖 UI 延伸模組輸出")](understanding-sirikit-images/intents02.png#lightbox)

意圖 UI 延伸模組一律`UIViewController`會傳回, 而且應用程式可以將它喜歡的任何專案新增至 view controller 內部, 例如顯示超出初始回應的其他資訊。 [意圖] UI 也可以使用長時間執行事件的狀態來更新使用者, 例如, 要將一間分享車輛觸及其位置所需的時間。

意圖 UI 延伸一律會與其他 Siri 內容一起顯示, 例如應用程式圖示和 UI 頂端的名稱, 或是根據意圖, 按鈕 (例如 [傳送] 或 [取消]) 可能會顯示在底部。

有幾個實例, 應用程式預設會將 Siri 顯示的資訊取代為使用者 (例如訊息或對應), 應用程式可以在此將預設體驗取代為應用程式量身打造的經驗。

> [!IMPORTANT]
> 雖然您可以將互動式專案 (例如`UIButtons`或`UITextFields` ) 新增至意圖 ui 延伸`UIViewController`模組, 但這些專案在非互動中會受到嚴格禁止, 而使用者將無法與他們互動。

應用程式完全選擇性地提供意圖 UI 延伸模組, 因為 Siri 包含每個意圖類型的一組預設 UI。 此外, 意圖 UI 介面僅適用于 Apple 認為對使用者很有説明的特定意圖。

## <a name="adding-sirikit-vocabulary"></a>新增 SiriKit 詞彙

執行 SiriKit 的最後一項工作是在應用程式中提供必要的詞彙。 許多應用程式都有獨特的方式來描述使用者的資訊, 以及使用者將提供資訊給應用程式的獨特方式。

因此, Siri 需要應用程式的協助, 才能瞭解應用程式特有的單字和片語。 其中一些片語會是應用程式的一部分, 讓每個使用者都知道並瞭解它們。 但有些則對應用程式的特定使用者而言是唯一的。

### <a name="app-specific-vocabulary"></a>應用程式特定詞彙

應用程式特定詞彙會定義所有應用程式使用者都知道的特定單字和片語, 例如車輛類型或健身名稱。 因為這些是應用程式的一部分, 所以會在檔案中`AppIntentVocabulary.plist`定義為主要應用程式套件組合的一部分。 此外, 應將這些單字和片語當地語系化。

詞彙`AppIntentVocabulary.plist`檔案有幾個部分:

- **範例應用程式使用**-這些會針對使用者可以對應用程式提出的要求提供一組常見的使用案例。 例如： *「開始使用 MonkeyFit 的健身」。*
- **參數**-這些會提供應用程式專屬的一組非標準參數類型。 例如, MonkeyFit 應用程式的健身名稱。 其中包括:
    - **片語**-允許此應用程式定義應用程式的唯一詞彙。 例如: MonkeyFit 應用程式的 "Bananarific" 健身類型。 
    - **發音**-提供發音提示, 將其 Siri 為指定片語的簡單語音拼法。 例如, "ba nana ri 特定"。
    - **範例**-提供在應用程式中使用指定片語的範例。 例如, *"Start a Bananarific In MonkeyFit"* 。

如需詳細資訊, 請參閱 Apple 的[應用程式詞彙檔案格式參考](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1)。

### <a name="user-specific-vocabulary"></a>使用者特定詞彙

使用者特定詞彙將提供對應用程式的個別使用者而言是唯一的單字或片語。 這些將在執行時間從主要應用程式 (而非應用程式延伸模組) 提供, 做為使用者的最重要使用優先順序所排序的一組詞彙, 並在清單開頭包含最重要的詞彙。

請參閱上述的 MonkeyChat 應用程式範例。 MonkeyChat 會保留所有使用者連絡人的清單, 並透過使用者特定詞彙傳送至 Siri。 它也會保留使用者已 messaged 的10個最新連絡人清單, 而且每位使用者都有一組我的最愛連絡人。 在此範例中, 我的最愛連絡人應該在使用者特定詞彙的開頭, 接著是最近的連絡人, 再加上使用者連絡人的其餘部分。

使用者特定詞彙支援下列類型的資訊:

- 連絡人名稱。
- 健身名稱。
- 相片專輯名稱。
- 相片關鍵字。

如果應用程式依賴 iOS 通訊錄, 應用程式就不需要採取任何動作, 因為此資訊已可供 Siri。 應用程式只有在應用程式擁有自己唯一的連絡人資料庫時, 才需要提供連絡人名稱。

在設計詞彙時, 只提供使用者知道和在意的必要值。 避免提供如電話號碼或電子郵件地址等資訊。

應用程式也需要在使用者特定詞彙變更時立即更新 Siri。 使用者習慣從 Siri 要求資訊, 並將其新增至其 iOS 裝置。 例如, 如果使用者在應用程式中新增連絡人, 請在使用者儲存該資訊後立即將其傳送至 Siri。

更重要的是, 應用程式_必須_立即刪除 Siri 詞彙中的資訊, 因為如果使用者刪除了一項資訊, 但 Siri 在一段時間內仍辨識了該資料, 則可能會變得很不滿意。

> [!IMPORTANT]
> 如果使用者選擇重設應用程式或登出, 應用程式應該從 Siri 中移除所有使用者特定的詞彙。

## <a name="sirikit-permissions"></a>SiriKit 許可權

SiriKit 的最後一項是以許可權為中心。 就像使用 iOS 的其他功能 (例如相片、相機或連絡人), 使用者必須授與明確的許可權, 應用程式才能與 Siri 交談。

應用程式可以提供字串, 定義它將提供給 Siri 的資訊, 並提供為什麼使用者應該授與此存取權的原因。 

Apple 建議應用程式在使用者第一次開啟應用程式並升級至 iOS 10 之後, 會要求使用者使用 Siri 的許可權。 如此一來, 使用者就能瞭解 Siri 整合, 並可在提出第一個要求之前預先核准使用方式。

## <a name="sirikit-and-maps"></a>SiriKit 和 Maps

SiriKit 是 iOS 不可或缺的一部分, 而且會使用已新增至 iOS 10 的較大意圖架構。 意圖架構是設計用來與系統的其他部分共用一般和共用的動作和意圖。

意圖架構不只是 Siri 整合, 也提供其他功能, 例如連絡人整合, 其中應用程式可以成為特定連絡人的預設電話語音或訊息應用程式。 意圖也提供與 CallKit 的深入整合, 為使用者提供最佳的 VOIP 體驗。

IOS 10 中的 Maps 應用程式已新增一些功能, 例如, 使用者可以直接在 Maps UI 內預訂某項的內容。 SiriKit 提供具有地圖的通用擴充點, 因此可以在 Siri 和對應之間共用您的共用 (和其他) 意圖。 

這表示如果應用程式已採用 SiriKit 擴充功能, 則也會免費取得 Maps 整合。 

## <a name="designing-a-great-siri-experience"></a>設計絕佳的 Siri 體驗

在將應用程式整合到 Siri 時, 設計絕佳的使用者體驗, 與設計絕佳的應用程式使用者介面不同。 不同于使用者直接在螢幕上與應用程式互動的一般情況, 使用 Siri 時, 有很多時候都看不到視覺化介面。 例如, 當使用者開始使用 *"嗨 Siri"* 的交談時。

### <a name="how-siri-helps-the-developer"></a>Siri 如何協助開發人員

在設計應用程式與 Siri 的互動時, 應用程式會建立*對話式介面*, 這表示內容衍生自 Siri 與使用者代表應用程式的交談。

如果沒有視覺參考, 使用者就必須追蹤其標頭中所顯示的資訊。 因此, Siri 會提供達成使用者希望完成之工作所需的最少資訊。

對話式介面會由使用者和 Siri 在交談期間的問題和回應成形。 因此, 請務必考慮 Siri 在設計此介面時, 如何詢問問題和回應。

針對建立訊息的使用者, 請採取下列範例, Siri 可能會回應「*準備送*出」這個問題。 使用者可以用許多不同的方式回應, 例如「*傳送給 It* *」、「取消」* , 或甚至與這個問題完全無關的東西。 無論交談的播放方式為何, Siri 都會為應用程式處理它, 而且只會在有相關資訊可供使用時, 將其傳送給它。

有幾種不同的方式可讓使用者起始與 Siri 的交談:

- 藉由挑選裝置, 按下 [首頁] 按鈕。 在這種情況下, Siri 會呈現更多視覺介面和較不口頭的回應。
- 藉由說「*嗨, Siri* 」並開始實習的免費交談。 在這種情況下, Siri 將會比較少視覺效果且更具口頭。
- 使用啟用 bluetooth 的聽覺協助工具, 其中 UI 會針對具有特殊需求的使用者量身打造。
- 使用汽車 Play, 使用者必須將分散注意力保持在最少的位置, 以專注于駕駛。

### <a name="how-the-developer-helps-siri"></a>開發人員如何協助 Siri

將應用程式與 Siri 整合時, 開發人員必須經常測試這項整合, 並確保它們會以許多不同的方式要求相同的資訊或工作, 以進行許多不同的要求。

因為沒有任何人認為這麼做, 所以開發人員務必盡可能取得不同的 Beta 測試人員, 以協助微調 Siri 整合。 使用者可能會要求資訊, 或以開發人員從未這麼做的方式提出要求, 而這項微調可以協助確保最大的使用者群組有使用其應用程式搭配 Siri 的絕佳體驗。

在不同的情況和環境中進行測試。 以各種方式起始與 Siri 的交談, 以確保這些交談保持流暢且自然。 在使用者不太可能使用應用程式的位置中進行測試, 例如在擁擠的 gym 中。

請確定應用程式提供 Siri 所需的所有資訊, 以適當地向使用者呈現要求和結果。 尤其是在手中的免費情況下使用 Siri 時更是如此。

### <a name="siri-design-guidelines"></a>Siri 設計指導方針

請務必記住, Siri 與代表應用程式的使用者有交談。 開發人員想要不確定此交談會盡可能保持流暢且自然的狀態。

與任何重要的交談一樣, 開發人員必須確保下列事項:

- 應用程式已準備好進行交談。
- 應用程式會接聽使用者嘗試完成的確切動作。
- 應用程式會在適當的時間詢問適當的問題。
- 應用程式會使用使用者所搜尋的資訊來回應要求。

#### <a name="preparing-for-the-conversation"></a>準備對話

第一件要記住的是, 應用程式的使用者不會與開發人員完全一樣。 它們可能來自不同的背景、說話不同的語言, 或在使用應用程式時有特殊需求。

此外, 由於開發人員的設計和建立應用程式, 他們對應用程式及其內部工作和函式 (一般使用者不會有) 有深入的瞭解。 因此, 開發人員可能會要求 Siri 的要求與一般使用者不同。

這就是為什麼有許多不同的人可以透過 Siri 與應用程式互動的原因。 使用者可能會透過 Siri 提出應用程式的要求, 因為開發人員從未想到過, 也不是以開發人員的想法來思考。

#### <a name="ensure-the-app-is-a-good-listener"></a>確保應用程式是良好的接聽者

開發人員必須確保應用程式是不錯的接聽者, 並取得符合使用者期望的交談細節。 但也可能未提供應用程式完成所要求之工作所需的所有資訊。

有數種方式可讓應用程式處理這種情況:

- 為**遺漏值挑選好的預設**值-例如, 如果使用者目前的位置未指定要從何處挑選, 則可能會預設為使用中的共用應用程式。
- 使用應用程式在使用者上收集的特定資訊來進行建議的**猜測**, 應用程式可能會對遺失的資訊進行猜測, 例如從使用者的連絡人資訊填入遺失的行動電話號碼。 不過, 請小心避免不良的意外, 例如挑選最昂貴的選項等等。
- **提示輸入詳細資訊**-應用程式可以讓 Siri 提示使用者提供遺漏值。 不過, 這裡的關鍵在於讓對話保持簡單和點。 如果使用者必須回答幾個問題來完成其要求, 他們很快就會變得很沮喪。
- **正常處理錯誤資訊**-使用者可能會提供應用程式未預期的值, 或無法在指定的內容中處理。 請確定應用程式將這種情況與使用者產生關聯, 使其清楚且容易更正。

當應用程式呈現有問題的單一值時, 處理這種情況的慣用方法是讓 Siri 要求使用者確認。 例如, 「*您的意思是 Bobo 很棒嗎？* 」, 他們可以透過簡單的「是」或「無」解答來回複。

當單一值有幾個可能的選擇可以正確時, 就會發生混淆, 這是慣用的處理方法。 在這種情況下, Siri 可以提示使用者最多十個可能的選項供您選擇。 例如：

```csharp
Who do you want to send the message to?

* Bobo the Great!
* Bobo Jr.
* Little Bobo
```

如果仍有問題, 請讓 Siri 提示使用者針對指定的值提供全新、更具體的答案。

#### <a name="request-final-confirmation"></a>要求最後確認

在應用程式實際執行工作以完成使用者的要求之前, Siri 會檢查應用程式延伸模組, 以確保一切都已就緒。 例如, 使用者的帳戶中是否有足夠的金錢來提出要求的付款？

此外, 應用程式必須確保它能夠提供 Siri 的所有資訊, 以便將它呈現給使用者, 並確認要執行的工作符合其預期。

一旦使用者確認要求, 而且應用程式已執行它, 應用程式就必須再次確定它已將所有結果提供給 Siri, 讓它可以與使用者建立關聯。

#### <a name="responding-to-the-request"></a>回應要求

Siri 有數個內建的使用者介面, 可用於其所知道的每個網域和動作。 不過, 在適當的情況下, 應用程式可以提供自訂意圖 UI 延伸模組, 藉由呈現應用程式的品牌和 UI, 或比要求中所提供的資訊更豐富使用者體驗。

話雖如此, 在設計 Siri 的自訂介面時, 應該使用擋板。 一般而言, 使用者會想要儘快完成特定工作, 而且不需要以不必要的資訊進行多載。

也請小心, 以確保自訂 UI 在所有不同的 iOS 裝置和方向 (也就是使用者可能會使用裝置) 中正確地尋找和回應。

適當時, 請使用 SiriKit API 來隱藏已出現在預設 Siri UI 中的任何多餘資訊。 此外, 請確定應用程式仍提供資訊給 Siri, 讓它可以在手中的免費情況下呈現它 verbally。

在某些情況下, Siri 會啟動應用程式以滿足使用者的要求, 例如呈現使用者所要求的相片。 在這些情況下, 請不要令使用者驚訝。 顯示預期的資訊, 而不需要進行中繼步驟或進一步的互動。 絕對不要顯示資訊或執行使用者不需要的工作。

### <a name="polishing-the-design"></a>修改設計

Apple 建議的幾個步驟會將對話介面的設計波蘭文。 第一, 是提供清楚、簡潔的詞彙和使用案例範例來 Siri。

使用者探索應用程式的其中一種方式, 是使用 Siri 起始交談並詢問「*您可以怎麼做？* 」 Siri 會顯示其可執行檔幾項不同專案, 包括開發人員的應用程式, 以及透過其`plist`檔案提供的範例主圖使用案例。

如何撰寫良好的範例使用案例:

- 請確定範例包含應用程式名稱。
- 讓範例保持簡短和點對點。
- 針對應用程式支援的每個意圖, 提供多個範例。
- 根據應用程式最常見的使用案例, 排列兩者中的意圖和範例的優先順序。
- 請確定應用程式提供當地語系化的範例。
- 請確定所提供的每個範例在應用程式內都如預期般運作。
- 避免在範例中定址 Siri, 因此請勿包含 *"嗨 Siri ..."* 之類的文字。
- 避免任何不必要的 pleasantries, 例如「*請*」或「*感謝您*」。

請採取適當的時間來探索和實驗應用程式如何塑造 Siri 與使用者的交談。 在整個過程中, 請務必與一般使用者溝通, 因為他們與應用程式的互動和期望可能會隨著時間而改變。

請務必記得在不同的情況下測試應用程式, 以及使用 Siri 叫用對話的所有不同方法。 在真實世界的位置測試使用者可能使用應用程式, 而不是從辦公室和桌面。

努力與 Siri (代表應用程式) 交談非常流暢、自然且「覺得直接」。 

## <a name="summary"></a>總結

本文涵蓋使用 SiriKit 所需的重要概念, 並顯示它可以與 Xamarin iOS 應用程式互動, 以提供使用者可使用 Siri 和 iOS 裝置上的 Maps 應用程式來存取的服務。




## <a name="related-links"></a>相關連結

- [ElizaChat 範例](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-elizachat)
- [SiriKit 程式設計指南](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [意圖架構參考](https://developer.apple.com/reference/intents)
- [意圖 UI 架構參考](https://developer.apple.com/reference/intentsui)
