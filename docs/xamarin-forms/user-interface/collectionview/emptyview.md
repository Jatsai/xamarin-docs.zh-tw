---
title: Xamarin. Forms CollectionView EmptyView
description: 在 CollectionView 中, 您可以指定空的視圖, 以在沒有資料可供顯示時提供意見反應給使用者。 空白的視圖可以是字串、視圖或多個視圖。
ms.prod: xamarin
ms.assetid: 6CEBCFE6-5577-4F68-9709-431062609153
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: c6a2a53f267a7f6764ec441944193e8c5ecd9189
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739195"
---
# <a name="xamarinforms-collectionview-emptyview"></a>Xamarin. Forms CollectionView EmptyView

![](~/media/shared/preview.png "此 API 目前是發行前版本")

[![下載範例](~/media/shared/download.png) 下載範例](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)定義下列屬性, 當沒有可顯示的資料時, 可以用來提供使用者意見反應:

- [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView), 屬於類型`object`, [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource)當屬性為`null`時, 或由`ItemsSource`屬性所指定的集合為`null`或空白時, 將顯示的字串、系結或視圖。 預設值為 `null`。
- [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate), 屬於類型[`DataTemplate`](xref:Xamarin.Forms.DataTemplate), 這是用來格式化指定`EmptyView`的範本。 預設值為 `null`。

這些屬性是由[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)物件所支援, 這表示屬性可以是資料系結的目標。

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [當`CollectionView`](xref:Xamarin.Forms.CollectionView)上的篩選作業未產生任何資料, 並在從 web 服務抓取資料時顯示使用者意見反應時, 設定屬性的主要使用案例會顯示使用者意見反應。

> [!NOTE]
> 如有需要, 可以將[屬性設定為包含互動式內容的視圖。`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)

如需資料範本的詳細資訊，請參閱 [Xamarin.Forms 資料範本](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)。

## <a name="display-a-string-when-data-is-unavailable"></a>當資料無法使用時顯示字串

`null` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null` `ItemsSource`屬性可以設定為字串, 當屬性為時, 或由屬性所指定的集合為或空白時, 就會顯示。 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 下列 XAML 顯示此案例的範例:

```xaml
<CollectionView ItemsSource="{Binding EmptyMonkeys}"
                EmptyView="No items to display" />
```

對等的 C# 程式碼是：

```csharp
CollectionView collectionView = new CollectionView
{
    EmptyView = "No items to display"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "EmptyMonkeys");
```

結果是, 因為資料系結集合是`null`, 所以會顯示設定[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)為屬性值的字串:

[ ![CollectionView 垂直清單的螢幕擷取畫面, 其中包含文字空白視圖, 在 IOS 和 Android 上]的(emptyview-images/null-itemssource.png "CollectionView 垂直清單中, 文字為空白視圖")](emptyview-images/null-itemssource-large.png#lightbox "CollectionView 具有空白視圖的垂直清單")

## <a name="display-views-when-data-is-unavailable"></a>資料無法使用時顯示視圖

`null` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null` `ItemsSource`屬性可以設定為 view, 當屬性為時, 或由屬性所指定的集合為或空白時, 就會顯示。 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 這可以是單一視圖, 或包含多個子視圖的視圖。 下列 XAML 範例顯示`EmptyView`將屬性設定為包含多個子視圖的視圖:

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </CollectionView.EmptyView>
    </CollectionView>
</StackLayout>
```

對等的 C# 程式碼是：

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new StackLayout
    {
        Children =
        {
            new Label { Text = "No results matched your filter.", ... },
            new Label { Text = "Try a broader filter?", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

[`SearchBar`](xref:Xamarin.Forms.SearchBar)當[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)執行時,所顯示的集合會針對儲存在屬性中的搜尋`FilterCommand`詞彙進行篩選。 如果篩選作業未產生任何資料, 則[`StackLayout`](xref:Xamarin.Forms.StackLayout)會顯示 [ [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)設定為] 屬性值:

[![在 IOS 和 Android 上以自訂空白視圖 CollectionView 垂直清單的螢幕擷取畫面](emptyview-images/filter-multiple-views.png "具有自訂空白視圖的 CollectionView 垂直清單")](emptyview-images/filter-multiple-views-large.png#lightbox "具有自訂空白視圖的 CollectionView 垂直清單")

## <a name="display-a-templated-custom-type-when-data-is-unavailable"></a>當資料無法使用時, 顯示樣板化自訂類型

`null` [`ItemsSource`](xref:Xamarin.Forms.ItemsView.ItemsSource) `null` `ItemsSource`屬性可以設定為自訂類型, 其範本會在屬性為時顯示, 或由屬性所指定的集合為或空白。 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) 屬性可以設定[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)為, 以`EmptyView`定義的外觀。 [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate) 下列 XAML 顯示此案例的範例:

```xaml
<StackLayout Margin="20">
    <SearchBar x:Name="searchBar"
               SearchCommand="{Binding FilterCommand}"
               SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
               Placeholder="Filter" />
    <CollectionView ItemsSource="{Binding Monkeys}">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </CollectionView.ItemTemplate>
        <CollectionView.EmptyView>
            <views:FilterData Filter="{Binding Source={x:Reference searchBar}, Path=Text}" />
        </CollectionView.EmptyView>
        <CollectionView.EmptyViewTemplate>
            <DataTemplate>
                <Label Text="{Binding Filter, StringFormat='Your filter term of {0} did not match any records.'}"
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </DataTemplate>
        </CollectionView.EmptyViewTemplate>
    </CollectionView>
</StackLayout>
```

對等的 C# 程式碼是：

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = new FilterData { Filter = searchBar.Text },
    EmptyViewTemplate = new DataTemplate(() =>
    {
        return new Label { ... };
    })
};
```

型別會`Filter`定義屬性, 以及對應[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)的: `FilterData`

```csharp
public class FilterData : BindableObject
{
    public static readonly BindableProperty FilterProperty = BindableProperty.Create(nameof(Filter), typeof(string), typeof(FilterData), null);

    public string Filter
    {
        get { return (string)GetValue(FilterProperty); }
        set { SetValue(FilterProperty, value); }
    }
}
```

屬性會設定`FilterData`為物件, 而`Filter`屬性資料會系結至[`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)屬性。 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`SearchBar`](xref:Xamarin.Forms.SearchBar)當[`CollectionView`](xref:Xamarin.Forms.CollectionView) `Filter`執行時,所顯示的集合會針對儲存在屬性中的搜尋`FilterCommand`詞彙進行篩選。 如果篩選作業不會產生任何資料, [`Label`](xref:Xamarin.Forms.Label)則會顯示[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)在中定義的 (設定[`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)為屬性值):

[![具有空白視圖範本之 CollectionView 垂直清單的螢幕擷取畫面, 在 IOS 和 Android 上]的(emptyview-images/emptyviewtemplate.png "CollectionView 具有空白視圖範本的垂直清單")](emptyview-images/emptyviewtemplate-large.png#lightbox "具有空白視圖範本的 CollectionView 垂直清單")

> [!NOTE]
> 當資料無法使用時, 顯示樣板化自訂類型[`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)時, 可以將屬性設定為包含多個子視圖的視圖。

## <a name="choose-an-emptyview-at-runtime"></a>在執行時間選擇 EmptyView

當資料無法使用時, 將[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)會顯示為的視圖, 可以定義為[`ContentView`](xref:Xamarin.Forms.ContentView)中[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)的物件。 接著, 您可以在執行時間根據某些`ContentView`商務邏輯, 將屬性設定為特定的。`EmptyView` 下列 XAML 範例顯示此案例的範例:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.EmptyViewSwapPage"
             Title="EmptyView (swap)">
    <ContentPage.Resources>
        <ContentView x:Key="BasicEmptyView">
            <StackLayout>
                <Label Text="No items to display."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
        <ContentView x:Key="AdvancedEmptyView">
            <StackLayout>
                <Label Text="No results matched your filter."
                       Margin="10,25,10,10"
                       FontAttributes="Bold"
                       FontSize="18"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
                <Label Text="Try a broader filter?"
                       FontAttributes="Italic"
                       FontSize="12"
                       HorizontalOptions="Fill"
                       HorizontalTextAlignment="Center" />
            </StackLayout>
        </ContentView>
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <StackLayout Orientation="Horizontal">
            <Label Text="Toggle EmptyViews" />
            <Switch Toggled="OnEmptyViewSwitchToggled" />
        </StackLayout>
        <CollectionView x:Name="collectionView"
                        ItemsSource="{Binding Monkeys}">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    ...
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

此 XAML 會在[`ContentView`](xref:Xamarin.Forms.ContentView)頁面層級[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)中定義兩個物件, [`Switch`](xref:Xamarin.Forms.Switch)其中物件會`ContentView` [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)控制要將哪個物件設定為屬性值。 當切換時`OnEmptyViewSwitchToggled` , 事件處理常式`ToggleEmptyView`會執行方法 [`Switch`](xref:Xamarin.Forms.Switch) :

```csharp
void ToggleEmptyView(bool isToggled)
{
    collectionView.EmptyView = isToggled ? Resources["BasicEmptyView"] : Resources["AdvancedEmptyView"];
}
```

[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView) [`ContentView`](xref:Xamarin.Forms.ContentView) `collectionView`方法會根據[`Switch.IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)屬性的值[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), 將物件的屬性設定為儲存在中的兩個物件的其中一個。 `ToggleEmptyView` [`SearchBar`](xref:Xamarin.Forms.SearchBar)當[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)執行時,所顯示的集合會針對儲存在屬性中的搜尋`FilterCommand`詞彙進行篩選。 如果篩選作業未產生任何資料, 則`ContentView`會顯示設定`EmptyView`為屬性的物件:

[ ![CollectionView 垂直清單的螢幕擷取畫面, 其中包含交換空的視圖, 在 IOS 和 Android 上,]已(emptyview-images/swap.png "交換空白視圖的 CollectionView 垂直清單")]已(emptyview-images/swap-large.png#lightbox "交換空白視圖的 CollectionView 垂直清單")

如需資源字典的詳細資訊, 請參閱[Xamarin. Forms 資源字典](~/xamarin-forms/xaml/resource-dictionaries.md)。

## <a name="choose-an-emptyviewtemplate-at-runtime"></a>在執行時間選擇 EmptyViewTemplate

您[`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)可以透過[`CollectionView.EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)將屬性設定為[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)物件, 在執行時間根據其值來選擇的外觀:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:CollectionViewDemos.Controls">
    <ContentPage.Resources>
        <DataTemplate x:Key="AdvancedTemplate">
            ...
        </DataTemplate>

        <DataTemplate x:Key="BasicTemplate">
            ...
        </DataTemplate>

        <controls:SearchTermDataTemplateSelector x:Key="SearchSelector"
                                                 DefaultTemplate="{StaticResource AdvancedTemplate}"
                                                 OtherTemplate="{StaticResource BasicTemplate}" />
    </ContentPage.Resources>

    <StackLayout Margin="20">
        <SearchBar x:Name="searchBar"
                   SearchCommand="{Binding FilterCommand}"
                   SearchCommandParameter="{Binding Source={x:Reference searchBar}, Path=Text}"
                   Placeholder="Filter" />
        <CollectionView ItemsSource="{Binding Monkeys}"
                        EmptyView="{Binding Source={x:Reference searchBar}, Path=Text}"
                        EmptyViewTemplate="{StaticResource SearchSelector}" />
    </StackLayout>
</ContentPage>
```

對等的 C# 程式碼是：

```csharp
SearchBar searchBar = new SearchBar { ... };
CollectionView collectionView = new CollectionView
{
    EmptyView = searchBar.Text,
    EmptyViewTemplate = new SearchTermDataTemplateSelector { ... }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

屬性會設定[`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)為屬性, 而[`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)屬性會設定為`SearchTermDataTemplateSelector`物件。 [`EmptyView`](xref:Xamarin.Forms.ItemsView.EmptyView)

[`SearchBar`](xref:Xamarin.Forms.SearchBar)當[`CollectionView`](xref:Xamarin.Forms.CollectionView) [`SearchBar.Text`](xref:Xamarin.Forms.SearchBar.Text)執行時,所顯示的集合會針對儲存在屬性中的搜尋`FilterCommand`詞彙進行篩選。 如果篩選作業未產生任何資料, 則[`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `SearchTermDataTemplateSelector` [`EmptyViewTemplate`](xref:Xamarin.Forms.ItemsView.EmptyViewTemplate)物件所選擇的會設定為屬性並顯示。

下列範例顯示`SearchTermDataTemplateSelector`類別:

```csharp
public class SearchTermDataTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate OtherTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        string query = (string)item;
        return query.ToLower().Equals("xamarin") ? OtherTemplate : DefaultTemplate;
    }
}
```

類別會定義`DefaultTemplate`設定`OtherTemplate`為不同資料範本的和[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)屬性。 `SearchTermTemplateSelector` 當搜尋查詢`DefaultTemplate`不等於 "xamarin" 時, 覆寫會傳回,以向使用者顯示訊息。`OnSelectTemplate` 當搜尋查詢等於 "xamarin" `OnSelectTemplate`時, 覆`OtherTemplate`寫會傳回, 以向使用者顯示基本訊息:

[CollectionView 中的![CollectionView 執行時間空白視圖範本選取專案的螢幕擷取畫面選取的 IOS 和 Android](emptyview-images/datatemplateselector.png "runtime 空白視圖範本")](emptyview-images/datatemplateselector-large.png#lightbox "CollectionView 中的執行時間空白視圖範本選取專案")

如需資料範本選取器的詳細資訊, 請參閱[建立 Xamarin DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)。

## <a name="related-links"></a>相關連結

- [CollectionView (範例)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Xamarin. 表單資料範本](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)
- [Xamarin. 表單資源字典](~/xamarin-forms/xaml/resource-dictionaries.md)
- [建立 Xamarin 表單 DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
