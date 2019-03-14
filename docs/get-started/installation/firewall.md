---
title: Xamarin 防火牆組態指示
description: 本文件提供的主機清單，必須列在防火牆的允許清單中，讓 Xamarin 在公司環境中運作。
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
author: asb3993
ms.author: amburns
ms.date: 10/05/2018
ms.openlocfilehash: 68689ce7d92a038d0724e1441f68fddcb1d0bba8
ms.sourcegitcommit: d62732ce6f3f9d8dc929d72d4acac3e592cba073
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/01/2019
ms.locfileid: "57199654"
---
# <a name="xamarin-firewall-configuration-instructions"></a>Xamarin 防火牆組態指示

_您需要在防火牆中加入允許清單許清的主機清單，以便讓 Xamarin 平台能為您的公司工作。_

為讓 Xamarin 產品能順利安裝及運作，Xamarin 必須要能夠存取特定端點來下載必要的工具及軟體更新。 若您或您的公司有嚴格的防火牆設定，您可能會遭遇安裝、授權、元件等問題。 本文件概要說明一些必須在您的防火牆中加入允許清單的已知端點，以讓 Xamarin 能正常運作。 這份清單不包含在下載內容中任何協力廠商工具所需要的端點。 若您在使用這份清單之後仍然遭遇問題，請參閱 Apple 或 Android 安裝疑難排解指南。

## <a name="endpoints-to-whitelist"></a>應加入允許清單的端點

### <a name="xamarin-installer"></a>Xamarin 安裝程式

在使用最新版本的 Xamarin 安裝程式時，您必須新增下列已知位址，才能順利安裝軟體：

- xamarin.com (安裝程式資訊清單)
- dl.xamarin.com (套件下載位置)
- dl.google.com (下載 Android SDK)
- download.oracle.com (JDK)
- visualstudio.com (安裝套件下載位置)
- go.microsoft.com (安裝 URL 解析)
- aka.ms (安裝 URL 解析)

若您使用的是 Mac 並且遭遇到 Xamarin.Android 安裝問題，請確認 macOS 可以下載 Java。

### <a name="nuget-including-xamarinforms"></a>NuGet (包括 Xamarin.Forms)

您必須新增下列位址，才能存取 NuGet (Xamarin.Forms 已封裝為 NuGet)：

- www\.nuget.org (以存取 NuGet)
- az320820.vo.msecnd.net (NuGet 下載)
- dl-ssl.google.com (適用於 Android 和 Xamarin.Forms 的 Google 元件)

### <a name="software-updates"></a>軟體更新

您必須新增下列位址，以確保軟體更新能夠順利下載：

- software.xamarin.com (Updater 服務)
- download.visualstudio.microsoft.com
- dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Xamarin Mac Agent

若要使用 Xamarin Mac Agent 將 Visual Studio 連線到 Mac 組建主機，您需要開啟 SSH 連接埠。 根據預設，**此連接埠是 22**。

## <a name="summary"></a>總結

本指南涵蓋應加入允許清的端點，以允許 Xamarin 產品在您的電腦上正常安裝及更新。