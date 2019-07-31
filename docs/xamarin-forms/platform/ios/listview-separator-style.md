---
title: IOS での ListView 区切り記号のスタイル
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、ListView のセル間の区切り記号が ListView の完全な幅を使用するかどうかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: A4CB45CE-9FB7-47ED-8C3D-93E39BF282E4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 921ef905eed17b6bfe5cb2ea401f7da92ff744af
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648116"
---
# <a name="listview-separator-style-on-ios"></a>IOS での ListView 区切り記号のスタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有の制御は[`ListView`](xref:Xamarin.Forms.ListView) 、のセル間の区切り記号がの完全な幅`ListView`を使用するかどうかを制御します。 これは、 XAML で[`ListView.SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty)添付プロパティを[`SeparatorStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)列挙型の値に設定して使用します。

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

`ListView.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)セル間の区切り記号かどうか、名前空間を使用して、 [ `ListView` ](xref:Xamarin.Forms.ListView)完全な使用幅、`ListView`で、 [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)列挙型の 2 つの値を提供します。

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) – 既定の iOS の区切り記号の動作を示します。 これは Xamarin.Forms の既定の動作です。
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) – の 1 つの端から区切り記号が描画されることを示します、`ListView`にします。

結果はするが、指定された[ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)値に適用されます、 [ `ListView`](xref:Xamarin.Forms.ListView)セル間の区切り記号の幅を制御します。

![](listview-separator-style-images/listview-separatorstyle.png "後のプラットフォームに固有の ListView")

> [!NOTE]
> 区切り記号のスタイルに設定されていると`FullWidth`に変更することはできません`Default`実行時にします。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
