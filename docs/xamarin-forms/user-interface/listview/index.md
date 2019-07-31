---
title: Xamarin.Forms ListView
description: このガイドでは、美しい、対話型のリストでデータを表示するために使用する Xamarin.Forms ListView が導入されています。
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: 4aae2a206f07ea6caa7fc0d7530fb9fec05ce5f0
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68648422"
---
# <a name="xamarinforms-listview"></a>Xamarin.Forms ListView

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)

[`ListView`](xref:Xamarin.Forms.ListView)は、データのリストを表示するためのビューです。特に、スクロールが必要な長いリストです。

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)は、さまざまなレイアウト仕様を使用してデータの一覧を表示するためのビューです。 これは [ `ListView`](xref:Xamarin.Forms.ListView) の代わりとして、より柔軟でより高パフォーマンスを提供することを目的にしています。 詳細については、「 [CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)」を参照してください。

## <a name="use-cases"></a>ユース ケース

ListView は、ニーズに応じて適切なコントロールを確認します。 ListView は、データのスクロール可能なリストを表示しているどのような状況で使用できます。 Listview は、コンテキスト アクションとデータ バインディングをサポートします。

ListView と混同しない[テーブル](~/xamarin-forms/user-interface/tableview.md)します。 テーブル コントロールは、オプションまたはデータの非バインド リストがあるたびより優れたオプションです。 たとえば、オプションのほとんどの場合に定義済みセットを持つ、iOS 設定アプリは、ListView よりテーブルを使用する適しています。

同種データも、ListView は、最適なが適しています&ndash;同じ型のすべてのデータは必ず、します。 これは、セルの 1 つだけの種類を一覧内の行ごとに使用されるためです。 ビューを混在させる必要がある場合より適切なオプションをされるため、TableViews は複数のセルの種類をサポートできます。

## <a name="components"></a>コンポーネント
ListView が、さまざまなコンポーネントの各プラットフォームのネイティブ機能を実行するために使用できます。 これらの各コンポーネントは、以下について説明します。

- **[ヘッダーとフッター](customizing-list-appearance.md#Headers_and_Footers)**  &ndash;リストのデータから先頭と、リストの末尾に表示するには、テキストまたはビューを分離します。 ヘッダーとフッター バインドできますしないデータ ソースに個別に ListView のデータ ソースから。
- **[グループ](customizing-list-appearance.md#Grouping)** &ndash;簡単に移動、ListView 内のデータをグループ化することができます。 通常、グループは、バインドされたデータです。

![](images/grouping-depth.png "グループ化されたデータと ListView")

- **[セル](customizing-cell-appearance.md)** &ndash;セルで、ListView でのデータが表示されます。 各セルは、データの行に対応します。 選択する組み込みのセルがまたは独自のカスタムのセルを定義することができます。 組み込みとカスタムの両方のセルには、XAML またはコードで使用される定義を指定できます。
  - **[組み込み](customizing-cell-appearance.md#Built_in_Cells)** &ndash;各プラットフォームのネイティブ コントロールに対応するため、パフォーマンスの優れた設定できます。 セル、特に TextCell と ImageCell、組み込まれています。
       - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash;詳細テキストを必要に応じて、テキストの文字列が表示されます。 詳細なテキストはアクセントの色とフォント サイズを小さくの 2 行目としてレンダリングされます。
       - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash;テキストとイメージを表示します。 左側のイメージの TextCell として表示されます。
  - **[カスタム セル](customizing-cell-appearance.md#customcells)** &ndash;複雑なデータを表示する必要がある場合は、カスタムのセルは最適です。 たとえば、曲、アルバム、アーティストなどの一覧を提供するカスタム ビューを使用できます。

![](images/image-cell-default.png "ImageCells と ListView")

ListView 内のセルのカスタマイズに関する詳細については、[ListView セルの外観のカスタマイズ](customizing-cell-appearance.md)を参照してください。

## <a name="functionality"></a>機能
ListView は、さまざまな相互作用のスタイル (など) をサポートしています。

- **[プルして更新](interactivity.md#Pull_to_Refresh)** &ndash; ListView は、各プラットフォームでプルして更新をサポートしています。
- **[コンテキスト アクション](interactivity.md#Context_Actions)** &ndash; ListView は、リスト内の個々 のアイテムに対するアクションの実行をサポートしています。 たとえば、ios では、スワイプのアクションを実装したり時間の長い Android でのアクションをタップできます。
- **[選択範囲](interactivity.md#selectiontaps)** &ndash;選択、および行がタップされたときに、アクションを実行する deselections をリッスンすることができます。

![](images/context-default.png "コンテキスト アクションを含む ListView")

ListView の対話機能の詳細については、[ListView の対話機能 (&)、アクション](interactivity.md)を参照してください。

## <a name="related-links"></a>関連リンク

- [ListView での操作 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)
- [2 つの方法 (サンプル) をバインド](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
- [セルで構築された (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [カスタムのセル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [グループ化 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [Custom Renderer View (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative/)
- [ListView の対話機能 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
