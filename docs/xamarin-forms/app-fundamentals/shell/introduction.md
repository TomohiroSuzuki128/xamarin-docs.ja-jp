---
title: Xamarin.Forms シェルの概要
description: Xamarin.Forms シェルでは、一般的なナビゲーション ユーザー エクスペリエンス、URI ベースのナビゲーション体系、統合された検索ハンドラーなど、ほとんどのアプリケーションが必要とする基本機能を提供しています。
ms.prod: xamarin
ms.assetid: 4604DCB5-83DA-458A-8B02-6508A740BE0E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2019
ms.openlocfilehash: dd1dc9b679a46dc082de1fe9b3c5f10b6757c0d8
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739282"
---
# <a name="xamarinforms-shell-introduction"></a>Xamarin.Forms シェルの概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms シェルでは、次に示すようなほとんどのモバイル アプリケーションが必要としている基本機能を提供することで、モバイル アプリケーション開発の複雑さを軽減します。

- 1 つの場所にアプリケーションの視覚階層を示す機能。
- 一般的なナビゲーション ユーザー インターフェイス。
- アプリケーション内の任意のページへ移動できる URI ベースのナビゲーション体系。
- 統合された検索ハンドラー

さらに、シェル アプリケーションには、レンダリング速度が速くなり、メモリ使用量が減少するという利点があります。

> [!IMPORTANT]
> Xamarin.Forms シェルは、iOS と Android でのみ使用できます。 既存の iOS と Android アプリケーションではシェルを採用することができ、ナビゲーション、パフォーマンス、および拡張性の強化をすぐに活用できます。

## <a name="shell-navigation-experience"></a>シェルのナビゲーション エクスペリエンス

シェルでは、ポップアップとタブに基づいて、厳格なナビゲーション エクスペリエンスを提供しています。 シェル アプリケーションでのナビゲーションの最上位レベルは、アプリケーションのナビゲーション要件に応じて、ポップアップまたは下部のタブ バーのいずれかです。 次の例は、ナビゲーションの最上位レベルがポップアップであるアプリケーションを示しています。

[![iOS および Android 上での、シェル ポップアップのスクリーンショット](introduction-images/flyout.png "シェル ポップアップ")](introduction-images/flyout-large.png#lightbox "シェル ポップアップ")

ポップアップ項目を選択すると、項目が選択され表示された状態で、下部のタブが表示されます。

[![iOS および Android 上での、シェルの下部タブのスクリーンショット](introduction-images/monkeys.png "シェルの下部タブ")](introduction-images/monkeys-large.png#lightbox "シェルの下部タブ")

> [!NOTE]
> ポップアップが開いていないときは、下部のタブ バーはアプリケーション内の上位レベルのナビゲーションと見なすことができます。

各タブには、[`ContentPage`](xref:Xamarin.Forms.ContentPage) が表示されます。 ただし、下部のタブに複数のページが含まれる場合は、上部のタブ バーからページをナビゲートできます。

[![iOS および Android 上での、シェルの上部タブのスクリーンショット](introduction-images/cats.png "シェルの上部タブ")](introduction-images/cats-large.png#lightbox "シェルの上部タブ")

各タブ内で、追加の [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトに移動できます。

[![iOS および Android 上での、シェルのページ ナビゲーションのスクリーンショット](introduction-images/cat-details.png "シェル アプリ ナビゲーション")](introduction-images/cat-details-large.png#lightbox "シェル アプリ ナビゲーション")

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
