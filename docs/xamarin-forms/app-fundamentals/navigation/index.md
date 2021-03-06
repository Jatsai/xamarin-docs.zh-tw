---
title: Xamarin.Forms 導覽
description: 本指南說明如何在 Xamarin.Forms 應用程式執行導覽。 Xamarin.Forms 會根據使用的頁面類型，提供多種不同的網頁導覽體驗。
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 682e3bd0ac4cdd651203496dd28586db2cef3165
ms.sourcegitcommit: c2bba24233624c2ec0e9ee9827310ca022212a2c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2019
ms.locfileid: "66835269"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms 導覽

_Xamarin.Forms 會根據使用的頁面類型，提供多種不同的網頁導覽體驗。_

![](images/page-types.png "Xamarin.Forms 頁面類型")

或者，Xamarin.Forms Shell 應用程式使用以 URI 為基礎的瀏覽體驗，該體驗不會強制執行一組瀏覽階層集合。 如需詳細資訊，請參閱 [Xamarin.Forms Shell 導覽](~/xamarin-forms/app-fundamentals/shell/navigation.md)。

## <a name="hierarchical-navigationhierarchicalmd"></a>[階層式導覽](hierarchical.md)

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 類別提供的階層式導覽體驗讓使用者能夠視需要，向前及向後導覽頁面。 此類別會實作一堆後進先出 (LIFO) 的 [`Page`](xref:Xamarin.Forms.Page) 物件導覽。

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) 由索引標籤和較大的詳細資料區域清單所組成，每個索引標籤會將內容載入至詳細資料區域中。

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) 讓使用者像使用資源庫般，能從一邊撥動到另一邊的頁面，以便導覽內容頁面。

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) 是可管理兩個相關資訊頁面的頁面，包含會顯示項目的主版頁面，以及詳細資料頁面，顯示的是主版頁面上的項目相關詳細資料。

## <a name="modal-pagesmodalmd"></a>[強制回應頁面](modal.md)

Xamarin.Forms 也支援強制回應頁面。 強制回應頁面鼓勵使用者完成各自獨立且無法離開的工作，直到完成或取消工作為止。
