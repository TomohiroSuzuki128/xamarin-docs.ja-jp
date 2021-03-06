---
title: Javadoc でパラメーターに名前を付ける
description: この記事では、Java プロジェクトから生成された Javadoc を使用して Java バインド プロジェクト内のパラメーター名を回復する方法について説明します。
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/20/2017
ms.openlocfilehash: bcfc5778ed4e5486d188f4eefbd32811792b44e5
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852978"
---
# <a name="naming-parameters-with-javadoc"></a>Javadoc でパラメーターに名前を付ける

> [!IMPORTANT]
> 現在、Xamarin プラットフォームでのカスタム バインディングの使用を調査しています。 今後の開発作業の発展のために、この[**アンケート**](https://www.surveymonkey.com/r/KKBHNLT)にご回答ください。

_この記事では、Java プロジェクトから生成された Javadoc を使用して Java バインド プロジェクト内のパラメーター名を回復する方法について説明します。_

## <a name="overview"></a>概要

既存の Java ライブラリをバインドする場合、バインドされた API に関するメタデータの一部は失われます。 具体的には、メソッドのパラメーターの名前です。 パラメーター名は、`p0`、`p1` などとして表示されます。これは、Java の `.class` ファイルでは、Java ソース コードで使用されたパラメーター名が保持されないためです。 

Xamarin.Android Java バインド プロジェクトでは、元のライブラリから Javadoc HTML にアクセスできる場合は、パラメーター名を指定できます。 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Java バインド プロジェクトへの Javadoc HTML の統合

Javadoc HTML を Java バインド プロジェクトに統合するには、次の手順で構成される手動のプロセスを実行します。 

1. ライブラリ用の Javadoc をダウンロードします
2. `.csproj` ファイルを編集し、`<JavaDocPaths>` プロパティを追加します
3. プロジェクトをクリーンアップしてリビルドします

この処理が完了したら、Java バインド プロジェクトによってバインドされた API に元の Java パラメーター名が存在するはずです。 

> [!NOTE]
> JavaDoc の出力には、非常に大きなバリエーションがあります。 .JAR バインディング ツールチェーンは、考えられるすべての変形をサポートしているわけではないため、一部のパラメーターに適切な名前が付けられない場合があります。

## <a name="summary"></a>まとめ

この記事では、Java バインド プロジェクトで Javadoc を使用して、バインドされた API に対して意味のあるパラメーター名を指定する方法について説明しました。 
