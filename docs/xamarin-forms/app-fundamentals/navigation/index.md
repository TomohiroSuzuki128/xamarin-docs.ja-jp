---
title: Xamarin.Forms のナビゲーション
description: このガイドでは、Xamarin.Forms アプリでナビゲーションを実行する方法について説明します。 Xamarin.Forms には、使用するページの種類に応じたさまざまなページ ナビゲーションのエクスペリエンスが用意されています。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8c907cd8a4a1d14b936dee309610bffc67ef363f
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137840"
---
# <a name="xamarinforms-navigation"></a>Xamarin.Forms のナビゲーション

_Xamarin.Forms には、使用するページの種類に応じたさまざまなページ ナビゲーションのエクスペリエンスが用意されています。_

![](images/page-types.png "Xamarin.Forms Page Types")

または、Xamarin.Forms シェル アプリケーションでは、設定されたナビゲーション階層を適用しない URI ベースのナビゲーション エクスペリエンスが使われます。 詳細については、「[Xamarin.Forms シェルのナビゲーション](~/xamarin-forms/app-fundamentals/shell/navigation.md)」を参照してください。

## <a name="hierarchical-navigation"></a>[階層ナビゲーション](hierarchical.md)

[`NavigationPage`](xref:Xamarin.Forms.NavigationPage) クラスは、ユーザーが前後を希望どおりにページを移動することができる階層ナビゲーション エクスペリエンスを提供します。 このクラスは、[`Page`](xref:Xamarin.Forms.Page) オブジェクトの後入れ先出し (LIFO) スタックとしてナビゲーションを提供します。

## <a name="tabbedpage"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms の [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) は、タブのリストと大きい詳細エリアで構成されており、各タブでは、コンテンツが詳細エリアに読み込まれます。

## <a name="carouselpage"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms の [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) は、ギャラリーのように、ユーザーが端から端までスワイプしてコンテンツの各ページをナビゲートできるページです。

## <a name="masterdetailpage"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms の [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) は、2 つの関連する情報ページ、つまり項目を表示するマスター ページと、マスター ページ上の項目に関する詳細を表示する詳細ページを管理するページです。

## <a name="modal-pages"></a>[モーダル ページ](modal.md)

Xamarin.Forms ではモーダル ページもサポートされています。 モーダル ページは、そのタスクが完了するかキャンセルされるまで、他の操作ができない自己完結型のタスクを完了させるようユーザーに促します。
