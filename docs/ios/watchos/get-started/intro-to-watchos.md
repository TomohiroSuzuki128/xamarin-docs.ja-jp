---
title: WatchOS の概要
description: このドキュメントでは、アプリケーションのライフ サイクル、ユーザー インターフェイスの種類、画面サイズ、制限、および詳細を説明する watchOS の概要を示します。
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/13/2016
ms.openlocfilehash: 59d02db9fa2787e93ad88e4b6f37e0fef50572a5
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70293112"
---
# <a name="introduction-to-watchos"></a>WatchOS の概要

> [!NOTE]
> チェック アウト、 [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)最新の機能の概要についてはします。

## <a name="about-watchos"></a>WatchOS について

WatchOS アプリのソリューションでは、3 つのプロジェクトがあります。

- **拡張機能を見る**– watch アプリのコードを含むプロジェクト。
- **アプリを見る**: ストーリー ボードのユーザー インターフェイスとリソースが含まれています。
- **iOS アプリの親**– このアプリは通常の iPhone アプリ。 IPhone アプリ ユーザーの監視に配信するためには、watch アプリと拡張機能がバンドルされています。

WatchOS 1 のアプリで、拡張機能のコードは、iPhone で実行 – Apple Watch は外付けディスプレイを効果的に。 2 および 3 watchOS アプリは、Apple Watch に完全に実行されます。 この違いは、次の図に示されます。

[![](intro-to-watchos-images/arch-sml.png "次の図は、watchOS 1 と watchOS 2 (およびそれ以降) の違いを示しています。")](intro-to-watchos-images/arch.png#lightbox)

WatchOS のバージョンを対象とするに関係なく Visual Studio for Mac の Solution Pad で完全なソリューションは次のようします。

[![](intro-to-watchos-images/projectstructure-sml.png "Solution Pad")](intro-to-watchos-images/projectstructure.png#lightbox)

*親アプリ*watchOS ソリューションは、標準的な iOS アプリ。 これは、表示されているソリューションでのみプロジェクト **、スマート フォン**します。 このアプリのユース ケースは、チュートリアル、管理画面、および中間層のフィルター処理、cacheing などが含まれます。ただし、インストールして、watch アプリ/拡張機能を実行せずに、ユーザーは**これまで**親アプリを開いたので親アプリを管理、または 1 回限りの初期化の実行する必要がある場合必要があります、ウォッチのプログラミングユーザーに伝えるためのアプリ/拡張機能です。

親アプリは、watch アプリと拡張機能を配信するがさまざまなサンド ボックスで実行されます。

WatchOS 1 で、共有アプリ グループを使用して、または静的関数を使用してデータを共有すること`WKInterfaceController.OpenParentApplication`が発生する、`UIApplicationDelegate.HandleWatchKitExtensionRequest`メソッドが、親のアプリの`AppDelegate`(を参照してください[親アプリ使用](~/ios/watchos/app-fundamentals/parent-app.md))。

WatchOS 2 以降がウォッチ接続フレームワークが、親アプリとの通信に使用されるを使用して、`WCSession`クラス。

## <a name="application-lifecycle"></a>アプリケーションのライフ サイクル

ウォッチ拡張機能のサブクラスで、`WKInterfaceController`各ストーリー ボードのシーンのクラスを作成します。

これら`WKInterfaceController`クラスに似ています、 `UIViewController` iOS プログラミングにオブジェクトが、同じレベルのビューへのアクセスはありません。
たとえば、コントロールを追加するか、UI を再構築することはできません動的にします。
ただし、非表示にしてコントロールを表示して、いくつかのコントロールのサイズ、透明性、および外観のオプションを変更できます。

ライフ サイクルを`WKInterfaceController`オブジェクトには、次の呼び出しが含まれます。

- 起動[状態:](xref:WatchKit.WKInterfaceController.Awake*)ほとんどの初期化は、このメソッドで実行する必要があります。
- を[アクティブ](xref:WatchKit.WKInterfaceController.WillActivate)にします。Watch アプリがユーザーに表示される直前に呼び出されます。 瞬間瞬間の最終初期化を実行、アニメーションなどを開始するには、このメソッドを使用します。
- この時点では、Watch アプリが表示され、拡張機能がユーザー入力し、アプリケーション ロジックを Watch アプリの表示を更新する応答を開始します。
- [DidDeactivate](xref:WatchKit.WKInterfaceController.DidDeactivate) Watch 後のアプリがユーザーによって破棄されて、このメソッドが呼び出されます。 ユーザー インターフェイス コントロールは、次回まで変更できませんこのメソッドから制御が戻た後`WillActivate`が呼び出されます。 IPhone への接続が切断された場合、このメソッドを呼び出すこともされます。
- 拡張機能が非アクティブ化された後に、プログラムにアクセスできません。 保留中の非同期関数**されません**呼び出せません。 ウォッチ キットの拡張機能は、バック グラウンド処理モードを使用しない可能性があります。 プログラムがユーザーによって再アクティブ化、オペレーティング システムによって、アプリが退職していない場合は、最初に呼び出されるメソッドになります`WillActivate`します。

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "アプリケーション ライフ サイクルの概要")

## <a name="types-of-user-interface"></a>ユーザー インターフェイスの種類

これは、3 種類の相互作用が、ユーザーは、watch アプリを持つことができます。
カスタムのサブ クラスを使用してプログラムすべて`WKInterfaceController`ライフ サイクルの既に説明したシーケンスが普遍的に適用されるため、(通知のクラスがサブ プログラム`WKUserNotificationController`、自体のサブクラスは、 `WKInterfaceController`)。

### <a name="normal-interaction"></a>通常の操作

Watch アプリ/拡張機能の相互作用の大半のサブ クラスられます`WKInterfaceController`watch アプリの内のシーンに対応するために作成**Interface.storyboard**します。 これの詳細については、[インストール](~/ios/watchos/get-started/installation.md)と[Getting Started](~/ios/watchos/get-started/index.md)記事。
次の図の一部を示しています、[ウォッチ キット カタログ](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルのストーリー ボード。 ここで示した各シーンには、対応するカスタム`WKInterfaceController`(`LabelDetailController`、 `ButtonDetailController`、`SwitchDetailController`など) で拡張機能プロジェクト。

![](intro-to-watchos-images/scenes.png "標準の相互作用の例")

### <a name="notifications"></a>通知

[通知](~/ios/watchos/platform/notifications.md)Apple Watch の主要なユース ケースが。 ローカルとリモートの両方の通知がサポートされています。 通知との対話は、短いおよび時間の長い外観と呼ばれる 2 つの段階で発生します。

短い外観について簡単に表示され、watch アプリのアイコン、名前、およびタイトルを表示 (指定された`WKInterfaceController.SetTitle`)。

システム提供を組み合わせて長い検索**枠**領域とストーリー ボード ベースのカスタム コンテンツに閉じるボタン。

`WKUserNotificationInterfaceController` 拡張`WKInterfaceController`メソッドを使って`DidReceiveLocalNotification`と`DidReceiveRemoteNotification`します。
Notification イベントに対応するこれらのメソッドをオーバーライドします。

通知の UI デザインの詳細についてを参照してください、 [Apple Watch のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "サンプルの通知")

## <a name="screen-sizes"></a>画面サイズ

Apple Watch には、次の2つの顔サイズがあります。38mm と 42 mm、画面の表示比率が5:4、Retina が表示されます。 使用可能なサイズは次のとおりです。

- 38mm:136 x 170 論理ピクセル (272 x 340 物理ピクセル)
- 42 mm:156 x 195 論理ピクセル (312 x 390 物理ピクセル)。

使用`WKInterfaceDevice.ScreenBounds`をどのディスプレイ、Watch アプリが実行されているかを判断します。

一般より制限された 38 mm ディスプレイを使用した、テキストとレイアウトのデザインを開発し、スケール アップに簡単です。
大規模な環境で開始する場合は、厄介重複やテキストの切り捨てにつながる可能性スケール ダウンします。

詳細をご覧ください[操作画面サイズ](~/ios/watchos/app-fundamentals/screen-sizes.md)します。


## <a name="limitations-of-watchos"></a>WatchOS の制限事項

これには、watchOS アプリを開発するときに認識する watchOS のいくつかの制限があります。

- ストレージの制限されている Apple Watch デバイス - (例: 大きなファイルをダウンロードする前に、使用可能な領域を認識します。 オーディオまたはビデオ ファイル)。

- 多くの watchOS[コントロール](~/ios/watchos/user-interface/index.md)UIKit に類似のものがあるが、さまざまなクラス (`WKInterfaceButton`なく`UIButton`、`WKInterfaceSwitch`の`UISwitch`など) と比較して、UIKit メソッドの限定的な対応します。 さらに、いくつかのコントロールのある watchOS など`WKInterfaceDate`(日付と時刻を表示する) その UIKit 必要はありません。

  - 通知のみを監視するか (ルーティングに対するユーザーのコントロールの種類が発表されていない Apple によって)、iPhone のみをルーティングすることはできません。

その他のいくつかの既知の制限]、[よく寄せられる質問。

- Apple では、サード パーティ製のカスタムの腕時計型インターフェイスは許可されません。

- 接続されている電話で iTunes を制御するウォッチができるようにする Api はプライベートです。


## <a name="further-reading"></a>関連項目

Apple のドキュメントをご覧ください。

- [ウォッチ キット用の開発](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

- [キットのプログラミング ガイドをご覧ください。](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

- [Apple Watch のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>関連リンク

- [watchOS 3 カタログ (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [watchOS 1 カタログ (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [セットアップとインストール](~/ios/watchos/get-started/installation.md)
- [Watch アプリの最初のビデオ](https://blog.xamarin.com/your-first-watch-kit-app/)
- [ウォッチ キット ガイド用の Apple を開発します。](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Apple の WatchKit ヒント](https://developer.apple.com/watchkit/tips/)
- [watchOS 3 の概要](~/ios/watchos/platform/introduction-to-watchos3/index.md)
