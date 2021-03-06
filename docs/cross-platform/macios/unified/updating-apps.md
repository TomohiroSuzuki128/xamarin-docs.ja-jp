---
title: 既存のアプリを Unified API に更新する
description: このドキュメントでは、Xamarin アプリケーションを Unified API に更新する方法について説明しているさまざまなガイドにリンクしています。 Xamarin iOS アプリ、Xamarin、Mac アプリについて説明します。 Xamarin Forms apps、クロスプラットフォームアプリのネイティブ型、およびバインドプロジェクト。
ms.prod: xamarin
ms.assetid: 8A654C95-5DCA-4BB5-A582-F96C2BECC81C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: d4904688302ac7f17c6326249ff08315d929e23c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015022"
---
# <a name="updating-existing-apps-to-the-unified-api"></a>既存のアプリを Unified API に更新する

> [!IMPORTANT]
> Unified API の前にある Xamarin Classic API は非推奨とされました。
>
> - Classic API (monotouch.dialog) をサポートする最新バージョンの Xamarin.iOS 9.10 がありました。
> - Classic API は引き続き Xamarin.Mac でサポートされますが、更新されなくなりました。 非推奨とされているため、開発者はアプリケーションを Unified API に移行する必要があります。

## <a name="how-to-update-your-apps"></a>アプリを更新する方法

アプリを更新するには、次の3つの手順を実行します。

1. 既存のコードのコンパイラ警告を修正します (特に、非推奨の API に関連するもの)。

2. Visual Studio for Mac に組み込まれている移行ツールを使用して、プロジェクト ファイルと名前空間を更新します。

3. 新しい[64 型](~/cross-platform/macios/nativetypes.md)および変更された[その他の API](~/cross-platform/macios/unified/overview.md#deprecated-typos)に関連する残りのコンパイラエラーを修正しました。 必要になる可能性がある手動更新の詳細については、[これらのヒント](~/cross-platform/macios/unified/updating-tips.md)を参照してください。

各製品には特定のガイドが用意されており、Unified API と64ビットのサポートにアプリを更新するのに役立ちます。

### <a name="xamarinios-appscross-platformmaciosunifiedupdating-ios-appsmd"></a>[Xamarin.iOS アプリ](~/cross-platform/macios/unified/updating-ios-apps.md)

既存の Xamarin.iOS アプリを Unified API に更新するには、Visual Studio for Mac に組み込まれている自動移行ツールを使用します。 次の[手順](~/cross-platform/macios/unified/updating-ios-apps.md)と[ヒント](~/cross-platform/macios/unified/updating-tips.md)で説明するように、いくつかの追加の修正が必要になる場合があります。

### <a name="xamarinmac-appscross-platformmaciosunifiedupdating-mac-appsmd"></a>[Xamarin.Mac アプリ](~/cross-platform/macios/unified/updating-mac-apps.md)

既存の Xamarin.Mac アプリは、Visual Studio for Mac に組み込まれている自動移行ツールを使用して、Unified API に更新できます。 次の[手順](~/cross-platform/macios/unified/updating-mac-apps.md)と[ヒント](~/cross-platform/macios/unified/updating-tips.md)で説明するように、いくつかの追加の修正が必要になる場合があります。

### <a name="xamarinforms-appscross-platformmaciosunifiedupdating-xamarin-forms-appsmd"></a>[Xamarin.Forms アプリ](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)

次の手順に従って、Unified API を使用するように既存の Xamarin.Forms ソリューションを iOS プロジェクトに更新します。 Unified API サポートは、Xamarin.Forms 1.3 以降でのみ使用できます。[この手順](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)では、Xamarin.Forms アプリをバージョン 1.3 に更新する方法についても説明します。 これらの[ヒント](~/cross-platform/macios/unified/updating-tips.md)は、カスタム レンダラーまたは依存関係サービスのネイティブ iOS コードを更新するのに役立ちます。

## <a name="working-with-native-types-in-cross-platform-appscross-platformmaciosnativetypesmd"></a>[クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/nativetypes.md)

この記事では、Android や Windows Phone OS などの iOS 以外のデバイスでコードを共有するクロスプラットフォームアプリケーションで、新しい iOS Unified API ネイティブ型 (nint, nuint, nfloat) を使用する方法について説明します。 ネイティブ型を使用するタイミングについての洞察を提供し、新しい型をクロスプラットフォームコードと共に使用する必要がある場合に、いくつかの考えられる解決策を提供します。

## <a name="update-bindings-to-the-unified-api"></a>バインドを Unified API に更新します。

Objective-C ライブラリへのバインドを作成したお客様は、バインド プロジェクトを更新して、基になる API の変更を反映する必要があります (一部の型は現在、64 ビットになります)。
次の手順に従って、[Unified API をサポートするように既存のバインド プロジェクトを更新](~/cross-platform/macios/unified/update-binding.md)します。

## <a name="related-links"></a>関連リンク

- [IOS アプリの更新](~/cross-platform/macios/unified/updating-ios-apps.md)
- [既存の Mac アプリを更新しています](~/cross-platform/macios/unified/updating-mac-apps.md)
- [Xamarin.Forms アプリの更新](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md)
- [バインドの更新](~/cross-platform/macios/unified/update-binding.md)
- [ヒントの更新](~/cross-platform/macios/unified/updating-tips.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
