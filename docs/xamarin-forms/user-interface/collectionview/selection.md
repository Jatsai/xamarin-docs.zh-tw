---
title: 設定 Xamarin.Forms CollectionView 選取模式
description: 根據預設，會停用 CollectionView 選取項目。 不過，您可以啟用單一和多重選取項目。
ms.prod: xamarin
ms.assetid: 423D91C7-1E58-4735-9E80-58F11CDFD953
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/18/2019
ms.openlocfilehash: 441afb9348a85de61d35574bb9121c7de713a897
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329841"
---
# <a name="set-collectionview-selection-mode"></a>設定 CollectionView 選取模式

![預覽](~/media/shared/preview.png)

[![下載範例](~/media/shared/download.png)下載範例](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

> [!IMPORTANT]
> `CollectionView`目前為預覽狀態，且缺少其中一些規劃功能。 此外，實作完成時，可能會變更的 API。

`CollectionView` 定義可控制的項目選取下列屬性：

- `SelectionMode`型別的`SelectionMode`，選取模式。
- `SelectedItem`型別的`object`，在清單中選取的項目。 此屬性具有`null`值選取任何項目時。
- `SelectionChangedCommand`型別的`ICommand`，來執行選取的項目變更時。
- `SelectionChangedCommandParameter`型別的`object`，這是傳遞至參數`SelectionChangedCommand`。

所有這些屬性會受到[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)物件，這表示，屬性可以是資料繫結的目標。

根據預設，`CollectionView`選取項目已停用。 不過，變更此行為，藉由設定`SelectionMode`屬性值的其中一個`SelectionMode`列舉成員：

- `None` – 表示不選取項目。 這是預設值。
- `Single` – 表示的單一項目可選取，以選取要反白顯示的項目。
- `Multiple` – 表示，多個項目可選取，在反白顯示選取的項目。

`CollectionView` 定義`SelectionChanged`事件引發時`SelectedItem`屬性變更，可能是因為使用者選取項目，從清單中，或應用程式設定的屬性。 `SelectionChangedEventArgs`隨附的物件`SelectionChanged`事件有兩個屬性，這兩個型別`IReadOnlyList<object>`:

- `PreviousSelection` -已選取，將選取範圍變更之前的項目清單。
- `CurrentSelection` -已選取，在選取範圍變更後的項目清單。

## <a name="single-selection"></a>選取單一項目

當`SelectionMode`屬性設定為`Single`中的單一項目`CollectionView`可選取。 選取的項目時，`SelectedItem`屬性會設定為所選的項目值。 當這個屬性變更時，`SelectionChangedCommand`執行 (其值為`SelectionChangedCommandParameter`傳遞至`ICommand`)，而`SelectionChanged`引發事件。

下列 XAML 範例所示`CollectionView`可以回應單一項目選取項目：

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectionChanged="OnCollectionViewSelectionChanged">
    ...
</CollectionView>
```

對等的 C# 程式碼是：

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SelectionChanged += OnCollectionViewSelectionChanged;
```

在此範例中，`OnCollectionViewSelectionChanged`事件處理常式執行時`SelectionChanged`事件引發時，與事件處理常式擷取先前選取的項目，以及目前選取的項目：

```csharp
void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string previous = (previousSelectedItems.FirstOrDefault() as Monkey)?.Name;
    string current = (currentSelectedItems.FirstOrDefault() as Monkey)?.Name;
    ...
}
```

> [!IMPORTANT]
> `SelectionChanged`因為變更而發生的變更可以引發事件`SelectionMode`屬性。

下列螢幕擷取畫面顯示單一項目中的選取項目`CollectionView`:

[![使用 iOS 和 Android 上的單一選取項目，以 CollectionView 垂直清單的螢幕擷取畫面](selection-images/single-selection.png "CollectionView 垂直清單選取單一項目")](selection-images/single-selection-large.png#lightbox "CollectionView 單一的垂直清單選取項目")

## <a name="pre-selection"></a>預先選取的項目

當`SelectionMode`屬性設定為`Single`中的單一項目`CollectionView`可以藉由設定預先選取`SelectedItem`項目的屬性。 下列 XAML 範例所示`CollectionView`預先選取單一項目：

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                SelectionMode="Single"
                SelectedItem="{Binding SelectedMonkey, Mode=TwoWay}">
    ...
</CollectionView>
```

對等的 C# 程式碼是：

```csharp
CollectionView collectionView = new CollectionView
{
    SelectionMode = SelectionMode.Single
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
collectionView.SetBinding(SelectableItemsView.SelectedItemProperty, "SelectedMonkey", BindingMode.TwoWay);
```

`SelectedItem`屬性的資料繫結至`SelectedMonkey`連線的檢視模型，也就是型別的屬性`Monkey`。 A`TwoWay`所以，如果使用者變更選取的項目值，會使用繫結`SelectedMonkey`屬性會設定於所選`Monkey`物件。 `SelectedMonkey`屬性定義於`MonkeysViewModel`類別，並設定的第四個項目為`Monkeys`集合：

```csharp
public class MonkeysViewModel : INotifyPropertyChanged
{
    ...
    public ObservableCollection<Monkey> Monkeys { get; private set; }

    Monkey selectedMonkey;
    public Monkey SelectedMonkey
    {
        get
        {
            return selectedMonkey;
        }
        set
        {
            if (selectedMonkey != value)
            {
                selectedMonkey = value;
            }
        }
    }

    public MonkeysViewModel()
    {
        ...
        selectedMonkey = Monkeys.Skip(3).FirstOrDefault();
    }
    ...
}
```

因此，當`CollectionView`出現時，系統會預先選取清單中的第四個項目：

[![使用單一預先選取的詳細資訊，請在 iOS 和 Android 的 CollectionView 垂直清單的螢幕擷取畫面](selection-images/single-pre-selection.png "CollectionView 與單一的預先選取的垂直清單")](selection-images/single-pre-selection-large.png#lightbox "CollectionView 垂直清單使用單一的預先選取項目")

## <a name="change-selected-item-color"></a>變更選取的項目色彩

`CollectionView` 已`Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState)可用來起始 visual 中的選取項目變更`CollectionView`。 一個常見使用案例，此`VisualState`變更選取的項目，下列 XAML 範例所示的背景色彩：

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="Grid">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CommonStates">
                        <VisualState x:Name="Normal" />
                        <VisualState x:Name="Selected">
                            <VisualState.Setters>
                                <Setter Property="BackgroundColor"
                                        Value="LightSkyBlue" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}"
                        SelectionMode="Single">
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        ...
                    </Grid>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </StackLayout>
</ContentPage>
```

> [!IMPORTANT]
> [ `Style` ](xref:Xamarin.Forms.Style) ，其中包含`Selected``VisualState`必須[ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)是根項目類型的屬性值[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)，此值會設定為`ItemTemplate`屬性值。

在此範例中， [ `Style.TargetType` ](xref:Xamarin.Forms.Style.TargetType)屬性值設定為`Grid`因為的根項目`ItemTemplate`是[ `Grid` ](xref:Xamarin.Forms.Grid)。 `Selected` [ `VisualState` ](xref:Xamarin.Forms.VisualState)中的項目，表示`CollectionView`選取，則[ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)的項目將會設定為`LightSkyBlue`:

[![使用自訂的單一選取色彩，在 iOS 和 Android 上的 CollectionView 垂直清單的螢幕擷取畫面](selection-images/single-selection-color.png "CollectionView 垂直清單，以自訂的單一選取色彩")] (selection-images/single-selection-color-large.png#lightbox "CollectionView 垂直清單，以自訂的單一選取的色彩")

如需有關視覺狀態的詳細資訊，請參閱[Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)。

## <a name="disable-selection"></a>停用選取範圍

`CollectionView` 預設會停用選取範圍。 不過，如果`CollectionView`已選取項目啟用，可以藉由設定停用`SelectionMode`屬性設`None`:

```xaml
<CollectionView ...
                SelectionMode="None" />
```

對等的 C# 程式碼是：

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    SelectionMode = SelectionMode.None
};
```

當`SelectionMode`屬性設定為`None`中的項目`CollectionView`無法選取`SelectedItem`屬性會保留`null`，和`SelectionChanged`將不會引發事件。

> [!NOTE]
> 當已選取項目和`SelectionMode`屬性已從`Single`要`None`，則`SelectedItem`屬性會設定為`null`和`SelectionChanged`會引發事件，使用空`CurrentSelection`屬性。

## <a name="related-links"></a>相關連結

- [CollectionView （範例）](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)