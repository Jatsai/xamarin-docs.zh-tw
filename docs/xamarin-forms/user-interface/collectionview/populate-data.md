---
title: 填入資料的 Xamarin.Forms CollectionView
description: CollectionView 填入資料，藉由設定它的 ItemsSource 屬性至任何實作 IEnumerable 的集合。
ms.prod: xamarin
ms.assetid: E1783E34-1C0F-401A-80D5-B2BE5508F5F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/15/2019
ms.openlocfilehash: 57012202d981b96dba42f3017a19f2e32e4982ec
ms.sourcegitcommit: a7170494e1975f0f1be547a45444752fd8e57819
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2019
ms.locfileid: "58507184"
---
# <a name="populate-xamarinforms-collectionview-with-data"></a>填入資料的 Xamarin.Forms CollectionView

![預覽](~/media/shared/preview.png)

[![下載範例](~/media/shared/download.png)下載範例](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView`目前為預覽狀態，且缺少其中一些規劃功能。 此外，實作完成時，可能會變更的 API。

`CollectionView` 會定義下列屬性，定義要顯示的資料和它的外觀：

- `ItemsSource`型別的`IEnumerable`，指定要顯示的項目集合而且具有預設值是`null`。
- `ItemTemplate`型別的[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)，指定要套用至要顯示的項目集合中的每個項目範本。

這些屬性都會受到[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)物件，這表示，屬性可以是資料繫結的目標。

## <a name="populate-a-collectionview-with-data"></a>填入資料 CollectionView

A`CollectionView`藉由設定資料填入其`ItemsSource`屬性來實作任何集合`IEnumerable`。 可以在 XAML 中加入項目，初始化`ItemsSource`從字串陣列的屬性：

```xaml
<CollectionView>
  <CollectionView.ItemsSource>
    <x:Array Type="{x:Type x:String}">
      <x:String>Baboon</x:String>
      <x:String>Capuchin Monkey</x:String>
      <x:String>Blue Monkey</x:String>
      <x:String>Squirrel Monkey</x:String>
      <x:String>Golden Lion Tamarin</x:String>
      <x:String>Howler Monkey</x:String>
      <x:String>Japanese Macaque</x:String>
    </x:Array>
  </CollectionView.ItemsSource>
</CollectionView>
```

> [!NOTE]
> 請注意，`x:Array` 項目需要 `Type` 屬性，以指出陣列中的項目類型。

對等的 C# 程式碼是：

```csharp
CollectionView collectionView = new CollectionView();
collectionView.ItemsSource = new string[]
{
    "Baboon",
    "Capuchin Monkey",
    "Blue Monkey",
    "Squirrel Monkey",
    "Golden Lion Tamarin",
    "Howler Monkey",
    "Japanese Macaque"
};
```

> [!IMPORTANT]
> 如果`CollectionView`會需要重新整理，如新增、 移除或變更基礎集合中的項目，基礎集合應該`IEnumerable`集合，其中會傳送屬性變更通知，例如`ObservableCollection`。

根據預設，`CollectionView`在垂直清單中，會顯示項目，如下列螢幕擷取畫面所示：

[![螢幕擷取畫面的 CollectionView，包含文字的項目，在 iOS 和 Android 上](populate-data-images/text.png "CollectionView 中的文字項目")](populate-data-images/text-large.png#lightbox "CollectionView 中的文字項目")

如需如何變更`CollectionView`版面配置，請參閱 <<c2> [ 指定配置](layout.md)。 如需如何定義在每個項目的外觀`CollectionView`，請參閱 <<c2> [ 定義項目外觀](#define-item-appearance)。

### <a name="data-binding"></a>資料繫結

`CollectionView` 可以使用資料來填入繫結中使用資料繫結及其`ItemsSource`屬性設`IEnumerable`集合。 在 XAML，這達成與`Binding`標記延伸模組：

```xaml
<CollectionView ItemsSource="{Binding Monkeys}" />
```

對等的 C# 程式碼是：

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

在此範例中，`ItemsSource`屬性的資料繫結至`Monkeys`連接的檢視模型的屬性。

> [!NOTE]
> 可以啟用已編譯的繫結，以改善在 Xamarin.Forms 應用程式中的資料繫結效能。 如需詳細資訊，請參閱 <<c0> [ 編譯的繫結](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)。

如需資料繫結的詳細資訊，請參閱 [Xamarin.Forms 資料繫結](~/xamarin-forms/app-fundamentals/data-binding/index.md)。

## <a name="define-item-appearance"></a>定義項目外觀

在每個項目的外觀`CollectionView`可以藉由設定定義`CollectionView.ItemTemplate`屬性設[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate):

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
    ...
</CollectionView>
```

對等的 C# 程式碼是：

```csharp
CollectionView collectionView = new CollectionView();
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");

collectionView.ItemTemplate = new DataTemplate(() =>
{
    Grid grid = new Grid { Padding = 10 };
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.RowDefinitions.Add(new RowDefinition { Height = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });
    grid.ColumnDefinitions.Add(new ColumnDefinition { Width = GridLength.Auto });

    Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 60, WidthRequest = 60 };
    image.SetBinding(Image.SourceProperty, "ImageUrl");

    Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
    nameLabel.SetBinding(Label.TextProperty, "Name");

    Label locationLabel = new Label { FontAttributes = FontAttributes.Italic, VerticalOptions = LayoutOptions.End };
    locationLabel.SetBinding(Label.TextProperty, "Location");

    Grid.SetRowSpan(image, 2);

    grid.Children.Add(image);
    grid.Children.Add(nameLabel, 1, 0);
    grid.Children.Add(locationLabel, 1, 1);

    return grid;
});
```

在指定的項目[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)定義清單中的每個項目的外觀。 在範例中，內的版面配置`DataTemplate`受[ `Grid` ](xref:Xamarin.Forms.Grid)。 `Grid`包含[ `Image` ](xref:Xamarin.Forms.Image)物件，以及兩個[ `Label` ](xref:Xamarin.Forms.Label)物件時，所有繫結的屬性至`Monkey`類別：

```csharp
public class Monkey
{
    public string Name { get; set; }
    public string Location { get; set; }
    public string Details { get; set; }
    public string ImageUrl { get; set; }
}
```

下列螢幕擷取畫面的清單中，每個項目將顯示的範本結果：

[![螢幕擷取畫面的 CollectionView，其中每個項目是範本，在 iOS 和 Android 上](populate-data-images/datatemplate.png "CollectionView 中的樣板化項目")](populate-data-images/datatemplate-large.png#lightbox "CollectionView 中的樣板化項目")

如需資料範本的詳細資訊，請參閱 [Xamarin.Forms 資料範本](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)。

## <a name="related-links"></a>相關連結

- [CollectionView （範例）](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms 資料繫結](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Xamarin.Forms 資料範本](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md)