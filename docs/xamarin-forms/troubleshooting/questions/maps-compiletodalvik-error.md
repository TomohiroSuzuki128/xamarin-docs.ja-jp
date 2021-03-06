---
title: なぜですか Xamarin.Forms 。マップ Android プロジェクトが COMPILETODALVIK の予期しないトップレベルエラーで失敗する
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C0251EB1-F509-47AD-98D6-846AF46425E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e29535e71cb77b05da41c043c6fd932ae4f5ce95
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84135851"
---
# <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik-unexpected-top-level-error"></a>なぜですか Xamarin.Forms 。マップ Android プロジェクトが COMPILETODALVIK の予期しないトップレベルエラーで失敗する

このエラーは、Visual Studio for Mac のエラーパッドまたは Visual Studio のビルド出力ウィンドウに表示されることがあります。Android プロジェクトでは、を使用 Xamarin.Forms します。マップ.

これは、Xamarin Android プロジェクトの Java ヒープサイズを増やすことによって最も一般的に解決されます。 ヒープサイズを増やすには、次の手順に従います。

## <a name="visual-studio"></a>Visual Studio

1. Android プロジェクト & 右クリックして、[プロジェクトオプション] を開きます。
2. **Android オプションにアクセス-> 詳細設定**
3. [Java ヒープのサイズ] テキストボックスに「1G」と入力します。
4. プロジェクトをリビルドします。

![Visual Studio プロジェクトオプションのスクリーンショット](maps-compiletodalvik-error-images/vsjavaheap.png "Visual Studio の Android ビルドオプション")

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. Android プロジェクト & 右クリックして、[プロジェクトオプション] を開きます。
2. **ビルド-> Android ビルド-> 詳細設定**にアクセス
3. [Java ヒープのサイズ] テキストボックスに「1G」と入力します。
4. プロジェクトをリビルドします。  

![Visual Studio for Mac プロジェクトオプションのスクリーンショット](maps-compiletodalvik-error-images/xsjavaheap.png "Visual Studio for Mac の Android ビルドオプション")
