---
title: Live アプリケーションの検査
description: このドキュメントでは、Xamarin Inspector を使用してアプリケーションを検査する方法について説明します。 また、Xamarin Inspector ツールの制限事項についても説明します。
ms.prod: xamarin
ms.assetid: 91B3206E-B2A5-4660-A6E5-B924B8FE69A7
author: davidortinau
ms.author: daortin
ms.date: 06/19/2018
ms.openlocfilehash: 4e4a231b6ae8dda3417cd32f95850a44d6c72260
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939387"
---
# <a name="inspecting-live-applications"></a>Live アプリケーションの検査

企業のお客様は、ライブアプリの検査を利用できます。

1. Visual Studio for Mac または Visual Studio で[サポートされているアプリプロジェクト](~/tools/inspector/install.md#supported-platforms)を開きます。
1. デバッグ モードでアプリケーションを実行します。
1. IDE ツールバーの [**検査**] ボタンをクリックします (Visual Studio では、[**ツール**] メニューまたは [**デバッグ**] メニューから [現在の**アプリの検査...** ] を選択することもできます)。

[![IDE ツールバーの [検査] ボタンをクリックします。](inspect-images/mac-heres-the-button.png)](inspect-images/mac-heres-the-button.png#lightbox)

新しい REPL プロンプトが表示され、新しい Xamarin Inspector クライアントウィンドウが開きます。

[![新しい REPL プロンプトを使用して、新しい Xamarin Inspector クライアントウィンドウが開きます。](inspect-images/inspector-0.7.0-map-inspect-small.png)](inspect-images/inspector-0.7.0-map-inspect.png#lightbox)

このウィンドウが表示されたら、C# のステートメントと式の実行と評価に使用できる対話型の C# プロンプトがあります。 これは、ターゲットプロセスのコンテキストでコードが評価されることを意味します。 この例では、表示されている iOS アプリケーションに対して実行されるコードを示しています。

アプリケーションの状態に対して行った変更は、実際にはターゲットプロセスで行われます。そのため、C# を使用してアプリケーションをライブで変更したり、アプリケーションの状態をライブで検査したりすることができます。

たとえば、iOS では、主なドライバーである UIApplication delegate クラスを見つけることができます。このクラスは、アプリケーションの状態を多数格納します。

```csharp
var del = (MyApp.AppDelegate) UIApplication.SharedApplication.Delegate
del.Database.GetAllCustomers ()
...
del.Database.AddCustomer (...)
```

(各送信は複数行エディターで行われることに注意してください。 `Shift + Enter`によって新しい行が作成され、 `Cmd + Enter` ( `Ctrl + Enter` Windows の場合は) 評価のためのコードが送信されます。 `Enter`安全な場合に自動的に送信されます。)

[検査] ボタンを使用すると、アプリケーションのビジュアル要素に簡単にアクセスできます。 このボタンをクリックすると、アプリケーションをクリックして UI 要素を選択できます。 変数は、 `selectedView` 画面上の実際の要素を指すように割り当てられます。 上のスクリーンショットでは、選択したにアクセスして編集する方法を確認でき `selectedView.BarTintColor` `UISearchBar` ます。

ライブビジュアルツリーも非常に便利です。 これは、ビュー階層の現在のスナップショットを表します。 REPL で設定する行を選択して、 `selectedView` ビューのプロパティ値を表示できます。 Mac では、階層ビューの3D 分解視覚化を操作できます。 Windows では、ビューのプロパティ値を視覚的に編集できます。

## <a name="known-limitations"></a>既知の制限事項

- ビューの選択は、メインディスプレイでのみサポートされています。
- プロパティグリッドの編集は Mac では使用できません。 Windows では、いくつかのデータ型に制限されています。 REPL を使用して、より強力な編集を行います。
- IDE でインスペクターのアドイン/拡張機能がインストールされ、有効になっている限り、デバッグモードで起動するたびに、アプリにコードが挿入されます。 アプリで奇妙な動作が発生した場合は、インスペクターのアドイン/拡張機能の無効化またはアンインストール、IDE の再起動、およびノートを試してください。 ご意見[をお聞かせください。](~/tools/inspector/install.md#reporting-bugs)
- UI 要素を検査しても変更されない場合は、[お知らせください。](~/tools/inspector/install.md#reporting-bugs)これはバグを示している可能性があるためです。
