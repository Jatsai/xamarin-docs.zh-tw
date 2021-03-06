---
title: 在 Xamarin 中購買可耗用的產品
description: 本檔說明 Xamarin 中的可耗用產品。 可耗用的產品是一項功能, 例如遊戲中的貨幣。
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 818412cef7359e134dea51e76567aa65bba8e613
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651639"
---
# <a name="purchasing-consumable-products-in-xamarinios"></a>在 Xamarin 中購買可耗用的產品

因為沒有「還原」需求, 所以可供取用的產品是最容易實行的。 它們適用于遊戲中的貨幣或單一使用功能的產品。 使用者可以重新購買可重複取用的產品。

## <a name="built-in-product-delivery"></a>內建產品傳遞

本檔所附的範例程式碼會示範內建產品–產品識別碼已硬式編碼到應用程式中, 因為它們緊密結合到在付款後「解除鎖定」功能的程式碼。 購買程式可以視覺化如下:   
   
[![購買程式視覺效果](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 基本工作流程如下:   
   
 1. 應用程式會將`SKPayment`新增至佇列。 如有需要, 系統會提示使用者輸入其 Apple ID, 並要求您確認付款。   
   
 2. StoreKit 會將要求傳送至伺服器以進行處理。   
   
 3. 當交易完成時, 伺服器會以交易回條回應。   
   
 4. 子`SKPaymentTransactionObserver`類別會接收回條並加以處理。   
   
 5. 應用程式會啟用產品 (藉由`NSUserDefaults`更新或一些其他機制), 然後呼叫 StoreKit 的`FinishTransaction`。

還有另一種類型的工作流程–*伺服器提供的產品*, 本檔稍後會加以討論 (請參閱*收據驗證和伺服器提供的產品*一節)。

## <a name="consumable-products-example"></a>可耗用產品範例

[InAppPurchaseSample 程式碼](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit)包含一個名為*耗材*的專案, 它會實作為基本「遊戲中貨幣」 (稱為「猴子點數」)。 此範例示範如何執行兩個應用程式內購買產品, 讓使用者能夠依自己的意願購買「猴子點數」, 而在實際的應用程式中, 也有一些方式可以耗費他們!   
   
   
   
 應用程式會顯示在這些螢幕擷取畫面中-每次購買時, 會將更多「猴子點數」新增至使用者的餘額:   
   
   
   
 [![每次購買都會為使用者餘額增加更多的猴子點數](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 自訂類別、StoreKit 和 App Store 之間的互動如下所示:   
   
   
   
 [![自訂類別、StoreKit 和 App Store 之間的互動](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController 方法

除了用來抓取產品資訊所需的屬性和方法以外, view controller 還需要額外的通知觀察者接聽購買相關通知。 這些只`NSObjects`是會在和`ViewWillDisappear`中`ViewWillAppear`分別註冊和移除的。

```csharp
NSObject succeededObserver, failedObserver;
```

此函式也會建立`SKProductsRequestDelegate`子類別`InAppPurchaseManager`(), `SKPaymentTransactionObserver`接著建立並註冊 ( `CustomPaymentObserver`)。   
   
   
   
 處理應用程式內購買交易的第一個部分, 是在使用者想要購買某個專案時, 處理按下按鈕的動作, 如下列範例應用程式中的程式碼所示:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 使用者介面的第二個部分是處理交易成功的通知, 在此情況下是藉由更新顯示的餘額:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

使用者介面的最後部分會在因為某些原因而取消交易時顯示訊息。 在範例程式碼中, 訊息只會寫入至 [輸出] 視窗:

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

除了 view controller 上的這些方法外, 可取用的`SKProductsRequestDelegate`產品購買交易也需要`SKPaymentTransactionObserver`和上的程式碼。

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager 方法

範例程式碼會在 InAppPurchaseManager 類別上執行數個購買相關的方法, 包括`PurchaseProduct` `SKPayment`建立實例並將它新增至佇列以進行處理的方法:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

將付款新增至佇列是非同步作業。 應用程式會在 StoreKit 處理交易並將其傳送至 Apple 的伺服器時, 重新獲得控制權。 此時, iOS 會驗證使用者是否已登入 App Store, 並在必要時提示她輸入 Apple ID 和密碼。   
   
   
   
 假設使用者已成功向 App Store 驗證, 並同意交易, `SKPaymentTransactionObserver`將會收到 StoreKit 的回應, 並呼叫下列方法來完成交易並加以完成。

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

最後一個步驟是確定您已藉由呼叫`FinishTransaction`下列方式, 通知 StoreKit 您已成功完成交易:

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

交付產品後, `SKPaymentQueue.DefaultQueue.FinishTransaction`必須呼叫以從付款佇列中移除交易。

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>SKPaymentTransactionObserver (CustomPaymentObserver) 方法

當 StoreKit 收到`UpdatedTransactions`來自 Apple 伺服器的回應時, 會呼叫方法, 並傳遞`SKPaymentTransaction`物件的陣列供您的程式碼檢查。 方法會迴圈處理每個交易, 並根據交易狀態來執行不同的函數 (如下所示):

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
              theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
              theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

本`CompleteTransaction`章節稍早涵蓋了方法–它會將購買詳細資料儲存`NSUserDefaults`至, 並使用 StoreKit 完成交易, 最後通知 UI 進行更新。

### <a name="purchasing-multiple-products"></a>購買多項產品

如果您的應用程式中有合理的購買多項產品, `SKMutablePayment`請使用類別並設定 Quantity 欄位:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

處理已完成交易的程式碼也必須查詢 Quantity 屬性, 才能正確完成購買作業:

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

當使用者購買多個數量時, StoreKit 確認警示會反映數量、單價和費用的總價格, 如下列螢幕擷取畫面所示:

[![確認購買](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>處理網路中斷

應用程式內購買需要有運作中的網路連線, StoreKit 才能與 Apple 的伺服器通訊。 如果網路連線無法使用, 則應用程式內購買將無法使用。

### <a name="product-requests"></a>產品要求

如果在進行`SKProductRequest`時無法使用網路`RequestFailed` , 將會呼叫子`SKProductsRequestDelegate`類別 ( `InAppPurchaseManager`) 的方法, 如下所示:

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

然後, ViewController 會接聽通知並在 [購買] 按鈕中顯示一則訊息:

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

由於行動裝置上的網路連線可能是暫時性的, 因此應用程式可能會想要使用 SystemConfiguration 架構來監視網路狀態, 然後在網路連接可用時重試。 請參閱 Apple 的或使用它的。

### <a name="purchase-transactions"></a>購買交易

StoreKit 付款佇列會在可能的情況下儲存和轉送購買要求, 因此, 網路中斷的影響會根據在購買程式期間網路失敗的時間而有所不同。   
   
   
   
 如果在交易`SKPaymentTransactionObserver`期間發生錯誤, 子類別 ( `CustomPaymentObserver`) 會有稱為`SKPaymentTransaction`的`UpdatedTransactions`方法, 而類別將會處於失敗狀態。

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
               theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
               theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`FailedTransaction`方法會偵測錯誤是否是因為使用者取消所造成, 如下所示:

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

即使交易失敗, `FinishTransaction`也必須呼叫方法以從付款佇列中移除交易:

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

然後, 範例程式碼會傳送通知, 讓 ViewController 可以顯示訊息。 如果使用者取消交易, 應用程式不應該顯示額外的訊息。 可能會發生的其他錯誤碼包括:

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>處理限制

IOS 的 [**設定] > [一般 > 限制**] 功能, 可讓使用者鎖定其裝置的某些功能。   
   
   
   
 您可以查詢使用者是否允許透過`SKPaymentQueue.CanMakePayments`方法進行應用程式內購買。 如果這會傳回 false, 則使用者將無法存取應用程式內購買。 如果嘗試購買, StoreKit 會自動向使用者顯示錯誤訊息。 藉由檢查此值, 您的應用程式可以改為隱藏 [購買] 按鈕, 或採取其他動作來協助使用者。   
   
   
   
 在檔案中, `CanMakePayments`方法會包裝 StoreKit 函數, 如下所示: `InAppPurchaseManager.cs`

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

若要測試此方法, 請使用 iOS 的**限制**功能來停**用應用程式內購買**:   
   
   
   
 [![使用 iOS 的限制功能來停用應用程式內購買](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 此範例程式碼`ConsumableViewController`會在`CanMakePayments`停用的按鈕上顯示**AppStore 停用**的文字, 以回應傳回 false。

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

當應用程式**內購買**功能受到限制時, 應用程式看起來會像這樣– [購買] 按鈕已停用。   
   
   
   
 [![當應用程式內購買功能受到限制時, 應用程式看起來會像是停用 [購買] 按鈕](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

當為 false 時`CanMakePayments`仍會要求產品資訊, 因此應用程式仍可取得並顯示價格。 這表示, 如果我們從`CanMakePayments`程式碼中移除核取按鈕仍在作用中的檢查, 但在嘗試購買時, 使用者會看到**不允許應用程式內購買**的訊息 (當付款佇列為時, StoreKit 會產生已存取):   
   
   
   
 [![不允許應用程式內購買](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 實際的應用程式可能會採用不同的方法來處理限制, 例如完全隱藏按鈕, 而且可能會提供比 StoreKit 自動顯示的警示更詳細的訊息。

