---
title: Windows 上の TabbedPage アイコン
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、TabbedPage ツールバーにページアイコンを表示できるようにする、Windows プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 7C5031A5-74EE-4469-994E-BEA7BA9D33CB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: d755f2242b1af94ce5729b1004eb867217876ac6
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656855"
---
# <a name="tabbedpage-icons-on-windows"></a>Windows 上の TabbedPage アイコン

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

これユニバーサル Windows プラットフォームプラットフォーム固有の機能により、 [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)ツールバーにページアイコンが表示されるようになります。また、必要に応じてアイコンのサイズを指定することもできます。 XAML で設定して使用される、 [ `TabbedPage.HeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty)添付プロパティを`true`、および必要に応じて設定して、 [ `TabbedPage.HeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty)添付プロパティを[ `Size`](xref:Xamarin.Forms.Size)値。

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" IconImageSource="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" IconImageSource="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" IconImageSource="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
    {
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", IconImageSource = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", IconImageSource = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", IconImageSource = "contacts.png" });
  }
}
```

`TabbedPage.On<Windows>`メソッドは、このプラットフォームに固有はユニバーサル Windows プラットフォームでのみ実行されるを指定します。 [ `TabbedPage.SetHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},System.Boolean))メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)名前空間はヘッダー アイコンをオンまたはオフにするために使用します。 [ `TabbedPage.SetHeaderIconsSize` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsSize(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},Xamarin.Forms.Size))メソッドは、必要に応じてでヘッダーのアイコンのサイズを指定します、 [ `Size` ](xref:Xamarin.Forms.Size)値。

さらに、`TabbedPage`クラス、`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`名前空間があります、 [ `EnableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*)ヘッダーのアイコンをできるようにするメソッド、 [ `DisableHeaderIcons` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*)ヘッダーのアイコンを無効にするメソッドと[ `IsHeaderIconsEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*)を返すメソッドを`boolean`ヘッダーのアイコンが有効になっているかどうかを示す値です。

結果は、そのページのアイコンを表示することができます、 [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) 、オプションで必要なサイズに設定されているアイコンのサイズを含むツールバー。

![TabbedPage プラットフォーム固有のアイコンが有効になっている](tabbedpage-icons-images/tabbedpage-icons.png "TabbedPage プラットフォーム固有のアイコンが有効になっています。")

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
