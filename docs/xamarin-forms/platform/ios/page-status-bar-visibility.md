---
title: IOS でのページステータスバーの表示
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、ページのステータスバーの表示を設定する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: D8BB7C24-A27F-4758-8557-6A81F909ABD9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: a187efa9310fa150ddc884d8b42da5ccb9ecee11
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655842"
---
# <a name="page-status-bar-visibility-on-ios"></a>IOS でのページステータスバーの表示

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有のは、のステータスバー [`Page`](xref:Xamarin.Forms.Page)の表示を設定するために使用され`Page`ます。また、ステータスバーがを入力または脱退する方法を制御することもできます。 これは、 XAML で `Page.PrefersStatusBarHidden` 添付プロパティを `StatusBarHiddenMode` 列挙型の値に設定して使用します。そして任意で `Page.PreferredStatusBarUpdateAnimation` 添付プロパティを `UIStatusBarAnimation` 列挙型の値に設定します。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 `Page.SetPrefersStatusBarHidden` メソッドは、`Xamarin.Forms.PlatformConfiguration.iOSSpecific` 名前空間に存在し、`StatusBarHiddenMode` 列挙型の値（`Default`、`True`、`False`）の 1 つを指定することで [`Page`](xref:Xamarin.Forms.Page) 上のステータスバーの可視性を設定するために使用されます。 `StatusBarHiddenMode.True` と `StatusBarHiddenMode.False` のそれぞれの値は、デバイスの方向に関係なくステータスバーの可視性を設定し、`StatusBarHiddenMode.Default` の値は縦向きの小さな環境でステータス バーを非表示にします。

その結果、[`Page`](xref:Xamarin.Forms.Page)のステータスバーの可視性を設定することができます。

![](page-status-bar-visibility-images/hide-status-bar.png "ステータス バーの可視性のプラットフォーム仕様")

> [!NOTE]
> [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage)では、`StatusBarHiddenMode` 列挙型の値にが設定されると、すべての子ページのステータスバーも更新されます。 [`Page`](xref:Xamarin.Forms.Page)から派生する他のすべての型では、`StatusBarHiddenMode` 列挙型の値が設定されると、現在のページのステータスバーだけが更新されます。

`Page.SetPreferredStatusBarUpdateAnimation` メソッドは、`UIStatusBarAnimation` 列挙型の値（`None`、`Fade`、`Slide`）の1つを指定することによって、ステータス バーがどのようにして [`Page`](xref:Xamarin.Forms.Page)に出入りするかを設定するのに使用されます。 `Fade` か `Slide` を指定すると、ステータスバーが `Page` を出入りする際に0.25秒のアニメーションが実行されます。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
