---
title: Xamarin.Forms でのテキスト
description: Xamarin.Forms は、テキストを操作するための 3 つのプライマリ ビューを備え、この記事は、それらを使用して入力し、Xamarin.Forms アプリケーションでテキストを表示する方法を説明します。
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2018
ms.openlocfilehash: 7b7cd9445ec9f28211037608ab04ffc45bec705c
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70227971"
---
# <a name="text-in-xamarinforms"></a>Xamarin.Forms でのテキスト

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Xamarin.Forms を使用して、テキストを入力または表示します。_

Xamarin.Forms では、テキストを操作するための 3 つのプライマリ ビューがあります。

- **[ラベル](#Label)** &mdash; 1 つまたは複数行のテキストを表示します。 同じ行に複数の書式設定オプションを使用してテキストを表示できます。
- **[エントリ](#Entry)** &mdash; 1 行のみであるテキストを入力します。 エントリでは、パスワードのモードがあります。
- **[エディター](#Editor)**  &mdash;複数行のあるテキストを入力します。

組み込みまたはカスタムを使用してテキストの外観を変更できます[スタイル](#Styles)と一部のコントロールにカスタム サポート[フォント](#Fonts)します。

<a name="Label" />

## <a name="labellabelmd"></a>[Label](label.md)

`Label`テキストを表示するビューを使用します。 複数行のテキストまたは 1 行のテキストを表示できます。 `Label` インラインで使用される複数の書式設定オプションを使用してテキストを表示できます。 ラベル表示では、ラップしたり、1 つの行に収まらないときにテキストを切り捨てることができます。

![ラベルの例](images/label.png)

参照してください、[ラベル](label.md)」の記事より詳細な情報。

ラベルで使用されるフォントをカスタマイズする方法の詳細については、[フォント](fonts.md)を参照してください。

<a name="Entry" />

## <a name="entryentrymd"></a>[エントリ](entry.md)

`Entry` 単一行テキスト入力を受け入れるように使用されます。 `Entry` プランに制御できる色とフォント。 `Entry` パスワードのモードがあり、テキストが入力されるまで、プレース ホルダー テキストを表示することができます。

![エントリの例](images/entry.png)

参照してください、[エントリ](entry.md)詳細についてはします。

異なりに注意してください`Label`、`Entry`カスタム フォントの設定を含めることはできません。

<a name="Editor" />

## <a name="editoreditormd"></a>[[エディター]](editor.md)

`Editor` 複数行テキスト入力を受け入れるように使用されます。 `Editor` プランに制御できる色とフォント。

![エディターの例](images/editor.png)

参照してください、[エディター](editor.md)詳細についてはします。

<a name="Fonts" />

## <a name="fontsfontsmd"></a>[フォント](fonts.md)

多くのコントロールは、各プラットフォームで組み込みフォントを使用するか、アプリに含まれるカスタムフォントを使用して、さまざまなフォント設定をサポートしています。 参照してください、[フォント](fonts.md)」の記事より詳細な情報。

<a name="Styles" />

## <a name="stylesstylesmd"></a>[スタイル](styles.md)

参照してください[スタイルを使用する](~/xamarin-forms/user-interface/styles/index.md)、フォントを設定する方法について[色](~/xamarin-forms/user-interface/colors.md)、および複数のコントロール全体に適用されるその他の表示プロパティ。

## <a name="related-links"></a>関連リンク

- [テキスト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
