---
title: 相依性插入
description: 本章說明 eShopOnContainers 行動應用程式如何使用相依性插入, 將具象的類型與依賴這些類型的程式碼分離。
ms.prod: xamarin
ms.assetid: a150f2d1-06f8-4aed-ab4e-7a847d69f103
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 6cbcd6612323acc8619004d56fff82461e005e9e
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529133"
---
# <a name="dependency-injection"></a>相依性插入

一般而言, 當具現化物件時, 會叫用類別的函式, 而且物件所需的任何值都會當做引數傳遞至函式。 這是相依性插入的範例, 特別是所謂的「程式性*插入*」。 物件所需的相依性會插入至此函式中。

藉由將相依性指定為介面類別型, 相依性插入可將具象的類型與依賴這些類型的程式碼分離。 它通常會使用容器來保存介面與抽象類別型之間的註冊和對應清單, 以及用來執行或擴充這些類型的實體類型。

另外還有其他類型的相依性插入, 例如*屬性 setter 插入*和*方法呼叫插入*, 但較不常看到。 因此, 本章僅著重于使用相依性插入容器來執行函式插入。

<a name="introduction_to_dependency_injection" />

## <a name="introduction-to-dependency-injection"></a>相依性插入簡介

相依性插入是一種特製化版本的控制反轉 (IoC) 模式, 其中的顧慮是指取得所需相依性的過程。 使用相依性插入時, 另一個類別會負責在執行時間將相依性插入物件中。 下列程式碼範例示範如何`ProfileViewModel`在使用相依性插入時結構化類別:

```csharp
public class ProfileViewModel : ViewModelBase  
{  
    private IOrderService _orderService;  

    public ProfileViewModel(IOrderService orderService)  
    {  
        _orderService = orderService;  
    }  
    ...  
}
```

此`ProfileViewModel`函式會`IOrderService`接收實例做為引數, 並由另一個類別插入。 `ProfileViewModel`類別中的唯一相依性是在介面型別上。 因此, `ProfileViewModel`類別並不知道負責具現`IOrderService`化物件的類別。 負責具現化`IOrderService`物件並將其插入`ProfileViewModel`至類別的類別, 稱為相依性*插入容器*。

相依性插入容器會提供一個可具現化類別實例的功能, 並根據容器的設定來管理其存留期, 藉此減少物件之間的耦合。 建立物件時, 容器會在其中插入物件所需的任何相依性。 如果尚未建立這些相依性, 容器會先建立並解析其相依性。

> [!NOTE]
> 您也可以使用 factory 手動實作為相依性插入。 不過, 使用容器會提供額外的功能, 例如生命週期管理, 並透過元件掃描進行註冊。

使用相依性插入容器有幾個優點:

- 容器不需要類別來尋找其相依性並管理其存留期。
- 容器允許對應已執行的相依性, 而不會影響類別。
- 容器可讓您模擬相依性, 藉此協助測試。
- 容器可讓您輕鬆地將新的類別新增至應用程式, 以提高維護性。

在使用 MVVM 的 Xamarin Forms 應用程式內容中, 相依性插入容器通常會用來註冊和解析視圖模型, 以及註冊服務並將其插入視圖模型。

有許多相依性插入容器可用, 使用 Autofac 來管理應用程式中的視圖模型和服務類別的具現化 eShopOnContainers 行動應用程式。 Autofac 有助於建立鬆散結合的應用程式, 並提供在相依性插入容器中常用的所有功能, 包括註冊型別對應和物件實例、解析物件、管理物件存留期, 以及插入相依物件放入其解析之物件的構造函式中。 如需 Autofac 的詳細資訊, 請參閱 readthedocs.io 上的[Autofac](http://autofac.readthedocs.io/en/latest/index.html) 。

在 Autofac 中, `IContainer`介面會提供相依性插入容器。 [圖 3-1] 顯示使用此容器時的相依性, `IOrderService`它會具現化物件`ProfileViewModel` , 並將其插入至類別。

![](dependency-injection-images/dependencyinjection.png "使用相依性插入時的相依性範例")

**圖 3-1:** 使用相依性插入時的相依性

在執行時間, 容器必須知道它應該具現`IOrderService`化之介面的執行, 才能具現`ProfileViewModel`化物件。 這包括:

- 容器, 決定如何具現化可實作為`IOrderService`介面的物件。 這就是所謂的*註冊*。
- 具現化可實作為`IOrderService`介面之物件的容器, `ProfileViewModel`以及物件。 這就是所謂的*解決方案*。

最後, 應用程式將會使用`ProfileViewModel`物件完成, 而且它將會變成可供垃圾收集之用。 此時, 如果其他類別不共用相同的實例, `IOrderService`垃圾收集行程就應該處置實例。

> [!TIP]
> 撰寫與容器無關的程式碼。 請一律嘗試撰寫與容器無關的程式碼, 以將應用程式與所使用的特定相依性容器分離。

## <a name="registration"></a>註冊

在將相依性插入物件之前, 必須先向容器註冊相依性的類型。 註冊類型通常牽涉到將介面和實作為介面的具象型別傳遞給容器。

有兩種方式可透過程式碼在容器中註冊類型和物件:

- 向容器註冊類型或對應。 必要時, 容器將會建立指定類型的實例。
- 在容器中, 將現有的物件註冊為 singleton。 必要時, 容器會傳回現有物件的參考。

> [!TIP]
> 相依性插入容器不一定是合適的。 相依性插入導入了對小型應用程式可能不適用或很有用的額外複雜性和需求。 如果類別沒有任何相依性, 或不是其他類型的相依性, 則將它放在容器中可能沒有意義。 此外, 如果類別具有一組屬於類型的整數相依性, 而且永遠不會變更, 則將它放在容器中可能沒有意義。

需要相依性插入的類型註冊應該在應用程式的單一方法中執行, 而且應該在應用程式的生命週期早期叫用此方法, 以確保應用程式知道其類別之間的相依性。 在 eShopOnContainers 行動應用程式中, 這是由`ViewModelLocator`類別所執行, 它`IContainer`會建立物件, 而是應用程式中的唯一類別, 它會保存該物件的參考。 下列程式碼範例顯示 eShopOnContainers 行動應用程式`IContainer`如何`ViewModelLocator`在類別中宣告物件:

```csharp
private static IContainer _container;
```

類型和實例會在`RegisterDependencies` `ViewModelLocator`類別的方法中註冊。 這是藉由先建立`ContainerBuilder`實例來達成, 如下列程式碼範例所示:

```csharp
var builder = new ContainerBuilder();
```

然後, 會向`ContainerBuilder`物件註冊型別和實例, 而下列程式碼範例會示範最常見的型別註冊形式:

```csharp
builder.RegisterType<RequestProvider>().As<IRequestProvider>();
```

這裡`RegisterType`所示的方法會將介面型別對應至實體型別。 它會指示容器在具現`RequestProvider`化物件時, 將物件具現化, 而此`IRequestProvider`物件需要透過函式的插入。

實體類型也可以直接註冊, 而不需要介面類別型的對應, 如下列程式碼範例所示:

```csharp
builder.RegisterType<ProfileViewModel>();
```

`ProfileViewModel`當類型解析時, 容器將會插入其所需的相依性。

Autofac 也允許實例註冊, 其中容器會負責維護類型之單一實例的參考。 例如, 下列程式碼範例示範當`ProfileViewModel`實例`IOrderService`需要實例時, eShopOnContainers mobile 應用程式如何註冊要使用的具體類型:

```csharp
builder.RegisterType<OrderService>().As<IOrderService>().SingleInstance();
```

這裡`RegisterType`所示的方法會將介面型別對應至實體型別。 `SingleInstance`方法會設定註冊, 讓每個相依物件接收相同的共用實例。 因此, 容器中只`OrderService`會有一個實例, 這是由需要`IOrderService`透過函式的插入的物件所共用。

實例註冊也可以使用`RegisterInstance`方法來執行, 如下列程式碼範例所示:

```csharp
builder.RegisterInstance(new OrderMockService()).As<IOrderService>();
```

此處`RegisterInstance`顯示的方法會建立新`OrderMockService`的實例, 並向容器註冊它。 因此, 容器中只`OrderMockService`會有一個實例, 這是由需要`IOrderService`透過函式的插入的物件所共用。

在註冊類型和實例之後, `IContainer`必須建立物件, 如下列程式碼範例所示:

```csharp
_container = builder.Build();
```

在實例`ContainerBuilder`上叫用方法時,會建立新的相依性插入容器,其中包含已`Build`建立的註冊。

> [!TIP]
> 將`IContainer`視為不可變。 雖然 Autofac 提供`Update`方法來更新現有容器中的註冊, 但應該盡可能避免呼叫此方法。 在建立容器之後修改它會有風險, 特別是在已使用容器的情況下。 如需詳細資訊, 請參閱在 readthedocs.io 上[將容器視為不可變](http://docs.autofac.org/en/latest/best-practices/#consider-a-container-as-immutable)。

<a name="resolution" />

## <a name="resolution"></a>解析度

註冊型別之後, 可以將它解析或插入為相依性。 當類型已解析且容器需要建立新的實例時, 它會將任何相依性插入實例。

一般而言, 當類型解析時, 會發生下列三種情況之一:

1. 如果類型尚未註冊, 容器會擲回例外狀況。
1. 如果類型已註冊為 singleton, 則容器會傳回單一實例。 如果這是第一次針對呼叫類型, 容器會在必要時建立它, 並維護它的參考。
1. 如果類型尚未註冊為 singleton, 容器會傳回新的實例, 而且不會維護它的參考。

下列程式碼範例會示範如何`RequestProvider`解析先前向 Autofac 註冊的類型:

```csharp
var requestProvider = _container.Resolve<IRequestProvider>();
```

在此範例中, 會要求 Autofac 解析`IRequestProvider`類型的具象類型, 以及任何相依性。 一般而言, 當`Resolve`需要特定類型的實例時, 會呼叫方法。 如需控制已解析物件之存留期的詳細資訊, 請參閱[管理已解析物件的存留期](#managing_the_lifetime_of_resolved_objects)。

下列程式碼範例顯示 eShopOnContainers 行動應用程式如何具現化視圖模型類型及其相依性:

```csharp
var viewModel = _container.Resolve(viewModelType);
```

在此範例中, 系統會要求 Autofac 為要求的視圖模型解析視圖模型類型, 而且容器也會解析任何相依性。 解析`ProfileViewModel`類型時, 要解析的相依性`IOrderService`是物件。 因此, Autofac 會先`OrderService`建立物件, 然後將它傳遞給`ProfileViewModel`類別的函式。 如需 eShopOnContainers 行動應用程式如何建立視圖模型並將其與視圖產生關聯的詳細資訊, 請參閱[使用視圖模型定位器自動建立視圖模型](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator)。

> [!NOTE]
> 使用容器來登錄和解析類型具有效能成本，因為容器會使用反映來建立每種類型，特別是在針對應用程式中的每個頁面巡覽重建相依性時。 如果有許多或深度相依性，則建立的成本可能會大幅增加。

<a name="managing_the_lifetime_of_resolved_objects" />

## <a name="managing-the-lifetime-of-resolved-objects"></a>管理已解析物件的存留期

註冊型別之後, Autofac 的預設行為是在每次解析型別時, 或是在相依性機制將實例插入其他類別時, 建立已註冊型別的新實例。 在此案例中, 容器不會保存解析物件的參考。 不過, 在註冊實例時, Autofac 的預設行為是將物件的存留期當做 singleton 來管理。 因此, 當容器在範圍內時, 實例會維持在範圍內, 而且會在容器超出範圍並已進行垃圾收集時處置, 或是在程式碼明確處置容器時進行處置。

Autofac 實例範圍可以用來指定 Autofac 從已註冊類型建立之物件的單一行為。 Autofac 實例範圍會管理由容器具現化的物件存留期。 `RegisterType`方法的預設實例範圍`InstancePerDependency`是範圍。 不過, `SingleInstance`範圍可以搭配`RegisterType`方法使用, 以便在呼叫`Resolve`方法時, 容器會建立或傳回類型的單一實例。 下列程式碼範例顯示如何指示 Autofac 建立`NavigationService`類別的單一實例:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

第一次`INavigationService`解析介面時, 容器會建立新`NavigationService`的物件, 並維護它的參考。 在任何後續的`INavigationService`介面解析中, 容器都會傳回先前建立之`NavigationService`物件的參考。

> [!NOTE]
> SingleInstance 範圍會在處置容器時處置已建立的物件。

Autofac 包含其他實例範圍。 如需詳細資訊, 請參閱 readthedocs.io 上的[實例範圍](http://autofac.readthedocs.io/en/latest/lifetime/instance-scope.html)。

## <a name="summary"></a>總結

相依性插入可將具象的類型與依賴這些類型的程式碼分離。 它通常會使用容器來保存介面與抽象類別型之間的註冊和對應清單, 以及用來執行或擴充這些類型的實體類型。

Autofac 有助於建立鬆散結合的應用程式, 並提供在相依性插入容器中常用的所有功能, 包括註冊型別對應和物件實例、解析物件、管理物件存留期, 以及插入相依物件放入其解析之物件的構造函式中。


## <a name="related-links"></a>相關連結

- [下載電子書 (2 Mb 的 PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (範例)](https://github.com/dotnet-architecture/eShopOnContainers)
