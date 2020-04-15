---
title: Mac App Store に Xamarin.Mac アプリを公開する
description: このドキュメントでは、Visual Studio for Mac で Xamarin.Mac アプリを展開する方法について説明します。 Mac 開発者アカウントを設定する方法、コード署名用の証明書を作成する方法、その証明書を利用し、直接または Mac App Store 経由で配布できる Map アプリをビルドする方法について説明します。
ms.prod: xamarin
ms.assetid: D26C5E54-EAD2-5487-264D-4263AEA1EBF2
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 1a14bcc15299360a17b4c9e7a363b6d6e6f2e69a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725026"
---
# <a name="publishing-xamarinmac-apps-to-the-mac-app-store"></a>Mac App Store に Xamarin.Mac アプリを公開する

## <a name="overview"></a>概要

Xamarin.Mac アプリは、2 つの異なる方法で配布することができます。

- **Developer ID** – Developer ID を使用して署名されたアプリケーションは、App Store の外部で配布することができますが、GateKeeper で認識され、インストールが許可されます。
- **Mac App Store** – Mac App Store に送信するには、アプリにインストーラー パッケージがあり、アプリとインストーラーの両方が署名されている必要があります。

このドキュメントでは、Visual Studio for Mac と Xcode を使用して、Apple Developer アカウントをセットアップし、各展開の種類用の Xamarin.Mac プロジェクトを構成する方法について説明します。

## <a name="mac-developer-program"></a>Mac Developer Program

[Mac Developer Program](https://developer.apple.com/devcenter/mac/) に参加するときには、下のスクリーンショットに示すように、開発者は、個人または会社としての参加を選択することができます。

[![Azure Developer ポータル](images/image1.png "Azure Developer ポータル")](images/image1-large.png#lightbox)

状況に応じた正しい登録の種類を選択します。

> [!NOTE]
> ここで選択した内容は、開発者アカウントを構成するときに、一部の画面の表示方法に影響します。 このドキュメントの説明とスクリーンショットは、**個人**の開発者アカウントの観点から示しています。 **会社**では、いくつかのオプションは**チーム管理者**ユーザーのみが使用できます。

### <a name="certificates-and-identifiers"></a>[証明書と ID](~/mac/deploy-test/publishing-to-the-app-store/certificates-identifiers.md)

このガイドでは、Xamarin.Mac アプリを発行するのに必要な証明書と ID を作成する手順について説明します。

### <a name="create-provisioning-profile"></a>[プロビジョニング プロファイルを作成する](~/mac/deploy-test/publishing-to-the-app-store/profiles.md)

このガイドでは、Xamarin.Mac アプリを発行するのに必要なプロビジョニング プロファイルを作成する手順について説明します。

### <a name="mac-app-configuration"></a>[Mac アプリの構成](~/mac/deploy-test/publishing-to-the-app-store/app-configuration.md)

このガイドでは、パブリケーション用の Xamarin.Mac アプリの構成の手順を説明します。

### <a name="sign-with-developer-id"></a>[Developer ID を使用して署名する](~/mac/deploy-test/publishing-to-the-app-store/signing.md)

このガイドでは、パブリケーションのために Developer ID を使用して Xamarin.Mac アプリを署名する方法について説明します。

### <a name="bundle-for-mac-app-store"></a>[Mac App Store 用のバンドル](~/mac/deploy-test/publishing-to-the-app-store/bundling.md)

このガイドでは、Mac App Store に Xamarin.Mac アプリを発行するためのバンドルの手順について説明します。

### <a name="upload-to-mac-app-store"></a>[Mac App Store へのアップロード](~/mac/deploy-test/publishing-to-the-app-store/uploading.md)

このガイドでは、Mac App Store に Xamarin.Mac アプリを発行するためのアップロード手順について説明します。

## <a name="related-links"></a>関連リンク

- [インストール](/visualstudio/mac/installation/)
- [Hello Mac のサンプル](~/mac/get-started/hello-mac.md)
- [Developer ID と GateKeeper](https://developer.apple.com/developer-id/)
