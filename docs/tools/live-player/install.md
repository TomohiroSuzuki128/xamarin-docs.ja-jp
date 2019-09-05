---
title: Xamarin Live Player Visual Studio の構成
description: このドキュメントでは、Xamarin Live Player を使用して、実行中のアプリケーションをライブ編集する方法について説明します。
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
author: conceptdev
ms.author: crdun
ms.date: 06/13/2019
ms.openlocfilehash: 94f1d36bf97aab7eabb57e6f2712c9850b390ab1
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290484"
---
# <a name="xamarin-live-player-visual-studio-configuration"></a>Xamarin Live Player Visual Studio の構成

![プレビュー機能](~/media/shared/preview.png)

> [!WARNING]
> Xamarin Live Player プレビューが終了しました。 アプリは使用できなくなりました。 以下の手順は、Visual Studio 2017 でプレビューの使用を継続しているお客様向けに提供されています。

> [!TIP]
> Visual Studio 2019 または Visual Studio for Mac の[XAML プレビューアー](~/xamarin-forms/xaml/xaml-previewer/index.md)を使用して、編集時に画面のデザインを表示できます。

# <a name="visual-studio-2017tabwindows"></a>[Visual Studio 2017](#tab/windows)

## <a name="using-xamarin-live-player"></a>Xamarin Live Player の使用

デバイスに Xamarin Live Player アプリが既にある必要があります。 ダウンロードすることはできません。

1. 開いている**Visual Studio 2017**します。
2. 移動して**ツール > オプション.** を選択し、 **Xamarin > その他の**タブ。
3. ティック**Xamarin Live Player を有効にする**:

    ![チェック オプション ウィンドウで、Xamarin Live Player を有効にするボックス](install-images/vs2017-options.png)

4. 作成するか、Xamarin プロジェクトを開きます (または[サンプル](~/tools/live-player/samples.md))。
5. 選択**Live Player**デバイスの一覧で。

    ![デバイスの一覧には、Xamarin Live Player のオプションが含まれています。](install-images/devices-empty-windows.png)

    - 既にデバイスをペアリングがある場合は、オプションとして使用ができます。
    - それ以外の場合求められますを必要な場合にデバイスをペアリングします。

6. キーを押して、**実行**ボタン、または、次のいずれかのオプションから選択、**実行**または右クリック メニュー。

    - **デバッグなしで開始**– アプリとデバイスの変更が発生する参照を編集することができます (アプリが、変更を加えるし、ファイルの保存と再起動される)。
    - **デバッグの開始**– ブレークポイントを設定し、変数を検査することができますが、コードを編集することはできません。

    または、選択**ツール > Xamarin Live Player > 現在のビューの実行を Live**アプリとデバイスの変更が発生する参照を編集することができます。 (アプリケーションのメイン画面) ではなく、現在のビューが表示されます。

7. デバイスは既にペアリングされています、Xamarin Live Player アプリがデバイスで実行されている場合は、コードはすぐ実行されます。

    デバイスをペアリングしていない場合、デバイスをペアリングする手順について QR コードに表示されます。

    ![ペアのデバイス ウィンドウ](install-images/manage-empty-windows.png)

    ペアリングには、デバイスに接続できない場合は、エラーが表示される可能性があります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="using-xamarin-live-player"></a>Xamarin Live Player の使用

デバイスに Xamarin Live Player アプリが既にある必要があります。 ダウンロードすることはできません。

1. 開いている**Visual Studio for Mac**します。
2. 移動して**Visual Studio > の基本設定.** を選択し、**プロジェクト > Xamarin Live Player (プレビュー)** タブ。
3. ティック**Xamarin Live Player を有効にする**:

    [![Xamarin Live Player を有効にするボックス](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

4. 作成するか、Xamarin プロジェクトを開きます (または[サンプル](~/tools/live-player/samples.md))。
5. 選択**Live Player**デバイスの一覧にします。

    ![デバイスの一覧には、Xamarin Live Player のオプションが含まれています。](install-images/devices.png)

    - 既にデバイスをペアリングがある場合は、オプションとして使用ができます。
    - それ以外の場合求められますを必要な場合にデバイスをペアリングします。
    - 選択**Xamarin Live Player デバイスしています.** の Xamarin Live Player を使用するデバイスを管理します。

6. キーを押して、**実行**ボタン、または、次のいずれかのオプションから選択、**実行**または右クリック メニュー。

    ![メニュー オプションを実行します。](install-images/run-menu.png)

    - **デバッグなしで開始**– アプリとデバイスの変更が発生する参照を編集することができます (アプリが、変更を加えるし、ファイルの保存と再起動される)。
    - **デバッグの開始**– ブレークポイントを設定し、変数を検査することができますが、コードを編集することはできません。
    - **ライブ実行の現在のビュー** – アプリとデバイスの変更が発生する参照を編集することができます。 (アプリケーションのメイン画面) ではなく、現在のビューが表示されます。

7. デバイスは既にペアリングされています、Xamarin Live Player アプリがデバイスで実行されている場合は、コードはすぐ実行されます。

    デバイスをペアリングしていない場合、デバイスをペアリングする手順について QR コードに表示されます。

    ![ペアのデバイス ウィンドウ](install-images/manage-empty.png)

    ペアリングには、デバイスに接続できない場合は、エラーが表示されます。

    ![エラー メッセージをデバイスに接続できません。](install-images/error-cannot-connect.png)

-----

問題が発生するか、または接続できない場合は、[制限事項とトラブルシューティング](~/tools/live-player/troubleshooting.md)を参照してください。

## <a name="related-links"></a>関連リンク

- [トラブルシューティング](~/tools/live-player/troubleshooting.md)
