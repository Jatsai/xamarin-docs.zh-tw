---
title: IOS 上的 ScrollView 內容潤色
description: 平台特性可讓您使用的功能只可在特定的平台，而不需要實作自訂轉譯器或影響。 本文說明如何使用 iOS 平臺特定的來控制 ScrollView 是否處理觸控手勢, 或將其傳遞至其內容。
ms.prod: xamarin
ms.assetid: 99F823DB-B379-40F0-A343-A9783C341120
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 154666cce4ad6c53949952fa93f5ad7dc89824ab
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651767"
---
# <a name="scrollview-content-touches-on-ios"></a>IOS 上的 ScrollView 內容潤色

[![下載範例](~/media/shared/download.png) 下載範例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

隱含的計時器會在觸控筆勢開始時觸發[ `ScrollView` ](xref:Xamarin.Forms.ScrollView)在 iOS 上和`ScrollView`決定，是否應該處理筆勢，或將它傳遞給其內容為基礎的計時器範圍內中的使用者動作。 根據預設，iOS`ScrollView`延遲內容的風格，但這可能會導致問題，在某些情況下使用`ScrollView`時應該不 winning 筆勢的內容。 因此，此平台特定的控制項是否`ScrollView`處理觸控筆勢，或將它傳遞給它的內容。 它由在 XAML 中設定`ScrollView.ShouldDelayContentTouches`附加屬性`boolean`值：

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

或者，它可以取用從 C# 使用 fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>`方法可讓您指定這個平台專屬只會在 iOS 上執行。 `ScrollView.SetShouldDelayContentTouches`方法，請在[ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)命名空間，是用來控制[ `ScrollView` ](xref:Xamarin.Forms.ScrollView)處理觸控筆勢，或將它傳遞給它的內容。 颾魤 ㄛ`SetShouldDelayContentTouches`方法可用來切換藉由呼叫延遲內容修飾`ShouldDelayContentTouches`傳回是否延遲內容修飾的方法：

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

結果是[ `ScrollView` ](xref:Xamarin.Forms.ScrollView)可以停用延遲接收內容的風格，因此，在此案例[ `Slider` ](xref:Xamarin.Forms.Slider)接收筆勢而不是[ `Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail)頁面的[ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

[![](scrollview-content-touches-images/scrollview-delay-content-touches.png "ScrollView 延遲內容觸及平台專屬")](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView 延遲內容觸及特定平台")

## <a name="related-links"></a>相關連結

- [PlatformSpecifics （範例）](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [建立平台特性](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
