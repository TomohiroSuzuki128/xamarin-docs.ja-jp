---
title: IOS での NavigationPage バーの透明度
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、NavigationPage のナビゲーションバーの透明度を変更する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 1150941B-56DB-4781-BE36-A4C4F9F2C500
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d30e1f86e2ef22250b9e25e2352501b6f2c05ade
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939140"
---
# <a name="navigationpage-bar-translucency-on-ios"></a>IOS での NavigationPage バーの透明度

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、のナビゲーションバーの透明度を変更するために使用され、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) XAML では、添付プロパティを値に設定することによって使用され [`NavigationPage.IsNavigationBarTranslucent`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty) `boolean` ます。

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

メソッドは、 `NavigationPage.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 [ `NavigationPage.EnableTranslucentNavigationBar` ] (Xref: Xamarin.FormsPlatformConfiguration. Xamarin.Forms EnableTranslucentNavigationBar () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。NavigationPage})) メソッドを名前空間で使用して、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) ナビゲーションバーを半透明にします。 さらに、 [`NavigationPage`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage) 名前空間のクラスに `Xamarin.Forms.PlatformConfiguration.iOSSpecific` も [ `DisableTranslucentNavigationBar` ] (xref: があり Xamarin.Forms ます。PlatformConfiguration. Xamarin.Forms DisableTranslucentNavigationBar () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。NavigationPage})。ナビゲーションバーを既定の状態に復元し、[ `SetIsNavigationBarTranslucent` ] (xref: Xamarin.Forms .PlatformConfiguration. Xamarin.Forms SetIsNavigationBarTranslucent () を指定します。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。NavigationPage}, Boolean)) メソッドを呼び出してナビゲーションバーの透明度を切り替えるために使用できるメソッド `IsNavigationBarTranslucent` (xref: Xamarin.FormsPlatformConfiguration. Iosbar半透明 ( Xamarin.Forms です。IPlatformElementConfiguration { Xamarin.Forms .PlatformConfiguration。 iOS、 Xamarin.Forms 。NavigationPage})) メソッド:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

その結果、ナビゲーションバーの透明度を変更できるようになります。

![半透明のナビゲーションバープラットフォーム固有](navigation-bar-translucent-images/translucent-navigation-bar.png)

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
