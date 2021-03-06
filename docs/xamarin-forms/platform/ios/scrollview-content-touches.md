---
title: IOS での ScrollView コンテンツへの触れる
description: プラットフォーム固有の機能を使用すると、カスタムレンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ使用できる機能を使用できます。 この記事では、ScrollView がタッチジェスチャを処理するか、そのコンテンツに渡すかを制御する iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 99F823DB-B379-40F0-A343-A9783C341120
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1161600ad9b587c30ef28be1828fdb9e3b94f665
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938555"
---
# <a name="scrollview-content-touches-on-ios"></a>IOS での ScrollView コンテンツへの触れる

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

IOS のでタッチジェスチャが開始されると、暗黙的なタイマーがトリガーされ [`ScrollView`](xref:Xamarin.Forms.ScrollView) `ScrollView` ます。これは、タイマースパン内のユーザー操作に基づいて、ジェスチャを処理するか、そのコンテンツに渡すかどうかを決定します。 既定では、iOS はコンテンツにタッチしますが、これにより、 `ScrollView` コンテンツが `ScrollView` 必要になったときにジェスチャに優先されない状況が発生する可能性があります。 したがって、このプラットフォーム固有のは、がタッチジェスチャを処理するか、そのコンテンツに渡すかを制御し `ScrollView` ます。 添付プロパティを値に設定することにより、XAML で使用 `ScrollView.ShouldDelayContentTouches` され `boolean` ます。

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

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

メソッドは、 `ScrollView.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `ScrollView.SetShouldDelayContentTouches`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) が [`ScrollView`](xref:Xamarin.Forms.ScrollView) タッチジェスチャを処理するか、そのコンテンツに渡すかを制御するために使用されます。 また、メソッドを `SetShouldDelayContentTouches` 使用して、コンテンツ `ShouldDelayContentTouches` のタッチが遅れているかどうかを返すメソッドを呼び出すことによって、コンテンツの遅延を切り替えることができます。

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

結果として、は、 [`ScrollView`](xref:Xamarin.Forms.ScrollView) コンテンツの受信遅延を無効にすることができます。このシナリオでは、はの [`Slider`](xref:Xamarin.Forms.Slider) ページではなくジェスチャを受け取り [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) ます。

[![ScrollView Delay コンテンツはプラットフォーム固有のものに触れる](scrollview-content-touches-images/scrollview-delay-content-touches.png)](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay コンテンツはプラットフォーム固有のものに触れる")

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
