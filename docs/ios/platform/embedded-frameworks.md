---
title: Xamarin の埋め込みフレームワーク。 iOS
description: このドキュメントでは、Xamarin iOS アプリケーションの埋め込みフレームワークでコードを共有する方法について説明します。 これは、mtouch ツールまたはネイティブ参照のいずれかを使用して行うことができます。
ms.prod: xamarin
ms.assetid: F8C61020-4106-46F1-AECB-B56C909F42CB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 690aaf81ee2600bd792a36f14b81df3d15e2d21b
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930274"
---
# <a name="embedded-frameworks-in-xamarinios"></a>Xamarin の埋め込みフレームワーク。 iOS

_このドキュメントでは、アプリケーション開発者がアプリにユーザーフレームワークを埋め込む方法について説明します。_

IOS 8.0 Apple では、Xcode のアプリ拡張機能とメインアプリ間でコードを共有するための埋め込みフレームワークを作成することができました。

Xamarin iOS 9.0 では、Xamarin iOS アプリでこれらの埋め込みフレームワーク (Xcode で作成されたもの) を使用するためのサポートが追加されています。 *任意の種類の Xamarin から埋め込みフレームワークを作成することはでき**ません**。 iOS プロジェクトは、既存のネイティブ (目標 C) フレームワークのみを使用します。*

Xamarin でフレームワークを使用するには、次の2つの方法があります。

- プロジェクトの**IOS ビルド**オプションの追加の mtouch 引数に以下を追加して、フレームワークを mtouch ツールに渡します。

  ```csharp
  --framework:/Path/To/My.Framework
  ```

  これは、プロジェクト構成ごとに設定する必要があります。

- コンテキストメニューからのネイティブ参照の追加

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

プロジェクトを右クリックし、[参照] をクリックしてネイティブ参照を追加します。

![Visual Studio for Mac で [ネイティブ参照の追加] を選択します。](embedded-frameworks-images/xam-native-refs.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

プロジェクトを右クリックし、[参照] をクリックしてネイティブ参照を追加します。

![Visual Studio で [ネイティブ参照の追加] を選択する](embedded-frameworks-images/vs-native-refs.png)

-----

  これは、すべての構成に対して機能します。

今後のバージョンの Visual Studio for Mac および Xamarin Tools for Visual Studio では、IDE 内からフレームワークを使用することができます (プロジェクトファイルを手動で編集する必要はありません)。

いくつかのサンプルプロジェクトは、 [github](https://github.com/rolfbjarne/embedded-frameworks)にあります。

## <a name="limitations"></a>制限事項

- 埋め込みフレームワークは、[統合](~/cross-platform/macios/unified/index.md)プロジェクトでのみサポートされています。
- 埋め込みフレームワークは、iOS 8.0 以降の配置ターゲットを持つプロジェクトでのみサポートされています。
- 拡張機能に埋め込みフレームワークが必要な場合は、コンテナーアプリにフレームワークへの参照も必要です。そうしないと、フレームワークはアプリバンドルに含まれません。

## <a name="the-mono-runtime"></a>Mono ランタイム

内部 Xamarin。 iOS は、この機能を利用して Mono ランタイムをフレームワークとしてリンクします。 Mono ランタイムを各拡張機能とコンテナーアプリに静的にリンクするのではありません。

これは、コンテナーアプリが統合されたアプリであり、拡張機能を含み、ターゲットデプロイが iOS 8.0 以降である場合に自動的に実行されます。

拡張機能がないアプリは、引き続き Mono ランタイムとリンクされます。これは、アプリが1つしか参照していない場合にフレームワークを使用すると、サイズが低下するためです。

この動作は、アプリ開発者によってオーバーライドできます。そのためには、プロジェクトの iOS ビルドオプションで追加の mtouch 引数として次のものを追加します。

- `--mono:static`: Mono ランタイムに静的にリンクします。
- `--mono:framework`: フレームワークとしての Mono ランタイムとのリンク。

Mono ランタイムとのフレームワークとしてのリンクの1つのシナリオは、拡張機能のないアプリでも、実行可能ファイルのサイズを小さくして、Apple が実行可能ファイルに適用するサイズ制限を克服することです。 参考までに、Mono ランタイムはアーキテクチャあたり約 1.7 MB を追加しています (Xamarin. iOS 8.12 の場合)。ただし、これらはリリースとアプリ間でも異なります。 Mono フレームワークは、アーキテクチャあたり約 2.3 MB を追加します。これは、拡張機能を持たない1つのアーキテクチャアプリの場合、アプリをフレームワークとして Mono ランタイムにリンクすると、実行可能ファイルのサイズが 1.7 MB に短縮されるのに対し、~ 2.3 mb の大規模アプリ全体が作成されることを意味します。
