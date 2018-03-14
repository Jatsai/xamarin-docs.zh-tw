---
title: Hello, iOS
description: "這份含有兩部分的指南將說明如何使用 Visual Studio for Mac 或 Visual Studio 來建置基本的 Xamarin.iOS 應用程式，以及了解使用 Xamarin 進行 iOS 應用程式開發的基本知識。 其中將介紹建置和部署 Xamarin.iOS 應用程式所需的工具、概念和步驟。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D3868F3A-4EED-BDDF-45AA-665102C39634
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: 269621056f0db807d8aebc8400b20d902d710b2c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2018
---
# <a name="helloios-quickstart"></a>Hello.iOS 快速入門

本指南說明如何建立可將使用者輸入的英數字元電話號碼轉譯成數字電話號碼，然後再撥打該號碼的應用程式。 最終的應用程式看起來如下：

 [ ![](hello-ios-quickstart-images/image1.png "Hello.iOS 快速入門應用程式")](hello-ios-quickstart-images/image1.png)


<a name="Requirements" />

## <a name="requirements"></a>需求

使用 Xamarin 進行 iOS 開發需要：

-  執行 macOS Sierra (10.12) 或更新版本的 Mac。
-  從 [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) \(英文\) 安裝的最新版 Xcode 和 iOS SDK。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Xamarin.iOS 可與下列設定搭配運作：

-  符合上述規格的最新版 Visual Studio for Mac。

[Xamarin.iOS Mac 安裝指南](~/ios/get-started/installation/mac.md)提供逐步安裝指示

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS 可與下列設定搭配運作：

-  位於 Windows 7 或更新版本上的最新版 Visual Studio 2015 或 2017 Professional 或更新版本，並且已與符合上述規格的 Mac 組建主機配對。

[Xamarin.iOS Windows 安裝指南](~/ios/get-started/installation/windows/index.md)會提供逐步安裝指示。

-----

開始使用之前，請下載 [Xamarin 應用程式圖示集](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac 逐步解說

本逐步解說將說明如何建立名為 Phoneword 且可將英數字元電話號碼轉譯成數字電話號碼的應用程式。

1. 從**應用程式**資料夾或**焦點**啟動 Visual Studio for Mac，以顯示啟動畫面：

  ![](hello-ios-quickstart-images/image2new.png "啟動畫面")

在啟動畫面上，按一下 [新增專案] 以建立新 Xamarin.iOS 方案：

![](hello-ios-quickstart-images/image3new.png "iOS 方案")


2. 從 [新增方案] 對話方塊，選擇 [iOS] > [應用程式] > [單一檢視應用程式] 範本，並確定已選取 C#。 按一下 [下一步]：

  ![](hello-ios-quickstart-images/image4new.png "選擇 [單一檢視應用程式]")

3. 設定應用程式。 在 [名稱] 中指定 `Phoneword_iOS`，並將其他選項維持為預設值。 按一下 [下一步]：

  ![](hello-ios-quickstart-images/image5new.png "輸入應用程式名稱")

4. 讓專案和方案名稱依原樣保留。 在此處選擇專案的位置，或將其保留為預設值：

  ![](hello-ios-quickstart-images/image6new.png "選擇專案的位置")

5. 按一下 [建立] 以產生**方案**。

6. 在 **Solution Pad** 中按兩下 **Main.storyboard** 檔案加以開啟。 這提供以視覺化方式建立 UI 的方式：

  ![](hello-ios-quickstart-images/image7new.png "iOS 設計工具")

  請注意，預設會啟用_大小類別_。 若要深入了解它們，請參閱[整合的分鏡腳本](~/ios/user-interface/storyboards/unified-storyboards.md)指南。

8. 在 [Toolbox Pad] 中，在搜尋列中輸入「標籤」，然後將**標籤**拖曳至設計介面 (中央區域)：

  ![](hello-ios-quickstart-images/image8new.png "將標籤拖曳至設計介面的中央區域")

  > [!NOTE]
> **注意：**您隨時都可瀏覽至[檢視] > [面板] 來顯示 **Properties Pad** 或**工具箱**。

9. 捕捉「拖曳控制項」的控點 (控制項周圍的圓形)，然後將標籤加寬：

  ![](hello-ios-quickstart-images/image9.png "將標籤加寬")


10. 利用設計介面上選取的**標籤**，使用 **Properties Pad** 來將**標籤**的**文字**屬性變更為 "Enter a Phoneword:"

  ![](hello-ios-quickstart-images/image10.png "將標籤設定為 Enter a Phoneword")

11. 在工具箱內搜尋「文字欄位」，然後將**文字欄位**從**工具箱**拖曳至設計介面，並放置於**標籤**下方。 調整寬度，直到**文字欄位**與**標籤**等寬為止：

  ![](hello-ios-quickstart-images/image12new.png "使文字欄位與標籤等寬")


12. 在於設計介面上選取 [文字欄位] 的情況下，將 [文字欄位] 的 [名稱] 屬性 (位於 **Properties Pad** 的 [身分識別] 區段) 變更為 `PhoneNumberText` 並將 [文字] 屬性變更為 "1-855-XAMARIN"：

  ![](hello-ios-quickstart-images/image13new.png "將標題屬性變更為 1-855-XAMARIN")


13. 將**按鈕**從**工具箱**拖曳至設計介面，並放置於**文字欄位**下方。 調整寬度，使**按鈕**與**文字欄位**和**標籤**等寬：

  ![](hello-ios-quickstart-images/image14new.png "調整寬度，使按鈕與文字欄位和標籤等寬")


14. 利用設計介面上選取的**按鈕**，在 **Properties Pad** 的 [身分識別] 區段中，將**名稱**屬性變更為 `TranslateButton`。 將**標題**屬性變更為 "Translate"：

  ![](hello-ios-quickstart-images/image15new.png "將標題屬性變更為 Translate")


15. 重複執行前兩個步驟，然後將**按鈕**從**工具箱**拖曳至設計介面，並放置於第一個**按鈕**下方。 調整寬度，使**按鈕**與第一個**按鈕**等寬：

  ![](hello-ios-quickstart-images/image16new.png "調整寬度，使按鈕與第一個按鈕等寬")


16. 利用設計介面上選取的第二個**按鈕**，在 **Properties Pad** 的 [身分識別] 區段中，將**名稱**屬性變更為 `CallButton`。 將**標題**屬性變更為 "Call"：

  ![](hello-ios-quickstart-images/image17new.png "將標題屬性變更為 Call")

  瀏覽至 [檔案] > [儲存] 或按 **⌘ + s** 來儲存變更。


17. 您必須將某些邏輯新增至應用程式，才能將電話號碼從英數字元轉譯為數字。 以滑鼠右鍵按一下 **Solution Pad** 中的 **Phoneword_iOS** 專案，然後選擇 [新增] > [新增檔案] 或按 **⌘ + n**，將新檔案新增至專案：

  ![](hello-ios-quickstart-images/image18.png "將檔案新增至專案")


18. 在 [新增檔案] 對話方塊中，選取 [一般] > [空類別] 並將新檔案命名為 `PhoneTranslator`：

  ![](hello-ios-quickstart-images/image19.png "選取 [空類別] 並將新檔案命名為 PhoneTranslator")


19. 這會建立新的空白 C# 類別。 移除所有範本程式碼，並使用下列程式碼來取代：

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword_iOS
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    儲存 **PhoneTranslator.cs** 檔案並將它關閉。

20. 新增連接使用者介面的程式碼。 若要執行此作業，在 **Solution Pad** 中按兩下 **ViewController.cs** 加以開啟：

  ![](hello-ios-quickstart-images/image20new.png "新增連接使用者介面的程式碼")


21. 從連接 `TranslateButton` 開始。 在 **ViewController** 類別中，尋找 `ViewDidLoad` 方法，並在 `base.ViewDidLoad()` 呼叫下方新增下列程式碼：

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(
            PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call ", UIControlState.Normal);
            CallButton.Enabled = false;
        } else {
            CallButton.SetTitle ("Call " + translatedNumber,
                UIControlState.Normal);
            CallButton.Enabled = true;
        }
    };
    ```

    如果檔案的命名空間不同，請包含 `using Phoneword_iOS;`。

22. 新增程式碼來回應按下第二個名為 `CallButton` 之按鈕的使用者。 將下列程式碼放置於 `TranslateButton` 的程式碼底下，然後在檔案頂端新增 `using Foundation;`：

    ```csharp
        CallButton.TouchUpInside += (object sender, EventArgs e) => {
            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl ("tel:" + translatedNumber);

            // ...otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl (url)) {
                var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                PresentViewController (alert, true, null);
            }
        };
    ```


23. 儲存變更，然後選擇 [建置] > [全部建置] 或按 **⌘ + B** 來建置應用程式。如果應用程式編譯，成功的訊息將會出現在 IDE 頂端：

  ![](hello-ios-quickstart-images/image21.png "成功的訊息將會出現在 IDE 頂端")

  如果發生錯誤，請完成上述步驟，並更正任何錯誤，直到應用程式建置成功為止。

27. 最後，在 **iOS 模擬器**中測試應用程式。 在 IDE 的左上角，選擇第一個下拉式清單中的 [偵錯]，並從第二個下拉式清單選擇 [iPhone 8 Plus iOS x.x]，然後按下 [啟動] \(類似 [播放] 按鈕的三角形按鈕\)：

  ![](hello-ios-quickstart-images/image27new.png "按 [啟動]")


  > [!NOTE]
> **注意：**目前，由於 Apple 的要求，因此可能需要具有開發憑證或*簽署身分識別*，才能針對裝置或模擬器建置程式碼。 請依照[裝置佈建指南](~/ios/get-started/installation/device-provisioning/manual-provisioning.md)中的步驟來設定此項。

28. 這樣將會在 iOS 模擬器內啟動應用程式：

  ![](hello-ios-quickstart-images/image28.png "在 iOS 模擬器中執行的應用程式")

  iOS 模擬器不支援撥打電話；將改為讓您在嘗試進行撥號時看到警示對話方塊：

  ![](hello-ios-quickstart-images/image29.png "嘗試進行撥號時的警示對話方塊")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-walkthrough"></a>Visual Studio 逐步解說

本逐步解說將說明如何建立名為 Phoneword 且可將英數字元電話號碼轉譯成數字電話號碼的應用程式。

**附註**：這個逐步解說使用 Windows 10 虛擬機器上的 Visual Studio Enterprise 2017。 您的設定可以與之不同，只要符合上述需求即可，但請注意，某些螢幕擷取畫面看起來可能與您的設定不同。

> [!NOTE]
> **繼續進行本逐步解說之前**，您「必須」已經從 Visual Studio 連線到您的 Mac。 這是因為 Xamarin.iOS 依賴 Apple 的工具來建置並啟動 iOS 設計工具和應用程式。 若要進行設定，請依照[連線到 Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 指南中的步驟執行。

1. 從 [開始] 功能表啟動 Visual Studio：

  ![](hello-ios-quickstart-images/image001-.png "[開始] 畫面")

  在 [新增方案] 下方的搜尋方塊中，輸入_單一檢視應用程式_，然後選取 [單一檢視應用程式 (iPhone)] 以建立新的 Xamarin.iOS 方案：

  ![](hello-ios-quickstart-images/image002-.png "新增單一檢視應用程式")


2. 將專案和方案命名為 `Phoneword`，如下所示：

  ![](hello-ios-quickstart-images/vs-image3.png "將專案命名為 PhonewordiOS，並將新的方案命名為 Phoneword")


3. 按 [確定] 以建立新專案

4. 確認工具列中的 Xamarin Mac Agent 圖示為綠色。

    ![確認工具列中的 Xamarin Mac Agent 圖示為綠色](hello-ios-quickstart-images/vs-image4.png)

    如果它不是這樣，表示沒有任何連線連線到您的 Mac 組建主機，請依照[設定指南](~/ios/get-started/installation/windows/connecting-to-mac/index.md)中的步驟執行以連線。


5. 在 [方案總管] 中按兩下 **Main.storyboard** 檔案，以在 iOS 設計工具中開啟該檔案：

  ![](hello-ios-quickstart-images/vs-image7.png "iOS 設計工具")

6. 開啟 [工具箱] 索引標籤，在搜尋列中輸入「標籤」，然後將**標籤**拖曳至設計介面 (中央區域)：

  ![](hello-ios-quickstart-images/vs-image8.png "將標籤拖曳至設計介面的中央區域")


7. 接著，抓住「拖曳控制項」的控點，然後將標籤加寬：

  ![](hello-ios-quickstart-images/vs-image9.png "將標籤加寬")


8. 利用設計介面上選取的**標籤**，使用**屬性視窗**來將**標籤**的**文字**屬性變更為 "Enter a Phoneword:"

  ![](hello-ios-quickstart-images/vs-image10.png "將標籤的文字屬性變更為 `Enter a Phoneword`")

  > [!NOTE]
> **注意：**您隨時都可瀏覽至 [檢視] 功能表來顯示 [屬性] 或 [工具箱]。


9. 在工具箱內搜尋「文字欄位」，然後將**文字欄位**從**工具箱**拖曳至設計介面，並放置於**標籤**下方。 調整寬度，直到**文字欄位**與**標籤**等寬為止：

  ![](hello-ios-quickstart-images/vs-image12.png "調整寬度，直到文字欄位與標籤等寬為止")


10. 利用設計介面上選取的**文字欄位**，在**屬性** 的 [身分識別] 區段中，將**文字欄位**的**名稱**屬性變更為 `PhoneNumberText`，並將**文字**屬性變更為 "1-855-XAMARIN"：

  ![](hello-ios-quickstart-images/vs-image13.png "將文字屬性變更為 1-855-XAMARIN")


11. 將**按鈕**從**工具箱**拖曳至設計介面，並放置於**文字欄位**下方。 調整寬度，使**按鈕**與**文字欄位**和**標籤**等寬：

  ![](hello-ios-quickstart-images/vs-image14.png "調整寬度，使按鈕與文字欄位和標籤等寬")


12. 利用設計介面上選取的**按鈕**，在**屬性**的 [身分識別] 區段中，將**名稱**屬性變更為 `TranslateButton`。 將**標題**屬性變更為 "Translate"：

  ![](hello-ios-quickstart-images/vs-image15.png "將標題屬性變更為 Translate")


13. 重複執行前兩個步驟，然後將**按鈕**從**工具箱**拖曳至設計介面，並放置於第一個**按鈕**下方。 調整寬度，使**按鈕**與第一個**按鈕**等寬：

  ![](hello-ios-quickstart-images/vs-image16.png "調整寬度，使按鈕與第一個按鈕等寬")


14. 利用設計介面上選取的第二個**按鈕**，在**屬性**的 [身分識別] 區段中，將**名稱**屬性變更為 `CallButton`。 將**標題**屬性變更為 "Call"：

  ![](hello-ios-quickstart-images/vs-image17.png "將標題屬性變更為 Call")

  瀏覽至 [檔案] > [全部儲存] 或按 **Ctrl + s** 來儲存變更。

15. 新增一些將電話號碼從英數字元轉譯為數字的程式碼。 若要執行此作業，先以滑鼠右鍵按一下 [方案總管] 中的 **Phoneword** 專案，然後選擇 [新增] > [新增項目] 或按 **Ctrl + Shift + A**，將新檔案新增至專案：

  ![](hello-ios-quickstart-images/vs-image18.png "新增一些將電話號碼從英數字元轉譯為數字的程式碼")


16. 在 [新增檔案] 對話方塊中，選取 [套用] > [類別] 並將新檔案命名為 `PhoneTranslator`：

  ![](hello-ios-quickstart-images/vs-image19.png "新增名為 PhoneTranslator 的類別")

  > [!IMPORTANT]
> 確定您選取的是圖示中具有 C# 的「類別」範本。 否則您可能無法參考這個新類別。


17. 這會建立新的 C# 類別。 移除所有範本程式碼，並使用下列程式碼來取代：

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

  儲存 **PhoneTranslator.cs** 檔案並將它關閉。

18. 在 [方案總管] 中按兩下 **ViewController.cs** 加以開啟，如此就能新增該邏輯來處理與按鈕的互動：

  ![](hello-ios-quickstart-images/vs-image20.png "新增來處理與按鈕互動的邏輯")


19. 從連接 `TranslateButton` 開始。 在 **ViewController** 類別中，尋找 `ViewDidLoad` 方法。 將下列按鈕程式碼新增至 `ViewDidLoad` 內 (位於 `base.ViewDidLoad()` 呼叫下方)：

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {

        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call", UIControlState.Normal);
            CallButton.Enabled = false;
            }
        else {
            CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
            CallButton.Enabled = true;
            }
    };
    ```
    如果檔案的命名空間不同，請包含 `using Phoneword;`。

20. 新增程式碼來回應按下第二個名為 `CallButton` 之按鈕的使用者。 將下列程式碼放置於 `TranslateButton` 的程式碼底下，然後在檔案頂端新增 `using Foundation;`：

    ```csharp
    CallButton.TouchUpInside += (object sender, EventArgs e) => {
        var url = new NSUrl ("tel:" + translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app,
            // otherwise show an alert dialog

        if (!UIApplication.SharedApplication.OpenUrl (url)) {
                        var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                        alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        PresentViewController (alert, true, null);
                    }
    };
    ```


21. 儲存變更，然後選擇 [建置] > [建置方案] 或按 **Ctrl + Shift + B** 來建置應用程式。如果應用程式編譯，成功的訊息將會出現在 IDE 底部：

  ![](hello-ios-quickstart-images/vs-image21.png "成功的訊息將會出現在 IDE 底部")

  如果發生錯誤，請完成上述步驟，並更正任何錯誤，直到應用程式建置成功為止。

22. 最後，在**遠端 iOS 模擬器**中測試應用程式。 在 IDE 工具列中，從下拉式功能表選擇 [偵錯] 和 [iPhone 8 Plus iOS x.x]，然後按 [啟動] \(類似 [播放] 按鈕的綠色三角形\)：

  ![](hello-ios-quickstart-images/vs-image27.png "按 [啟動]")

23. 這樣將會在 iOS 模擬器內啟動應用程式：

  ![](hello-ios-quickstart-images/vs-image28.png "在 iOS 模擬器中執行的應用程式")

  iOS 模擬器不支援撥打電話；將改為在嘗試進行撥號時顯示警示對話方塊：

  ![](hello-ios-quickstart-images/vs-image29.png "警示對話方塊將會在嘗試進行撥號時顯示")

-----

恭喜您完成第一個 Xamarin.iOS 應用程式！

現在可在 [Hello, iOS 深度剖析](~/ios/get-started/hello-ios/hello-ios-deepdive.md)中仔細分析本指南所示的工具與技能。


## <a name="related-links"></a>相關連結

- [Xamarin 應用程式圖示和啟動影像 (範例)](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)
- [Hello, iOS (範例)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) \(英文\)
- [iOS 人性化介面指導方針](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html) \(英文\)
- [iOS 佈建入口網站](https://developer.apple.com/ios/manage/overview/index.action) \(英文\)