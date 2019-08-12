---
title: コンパイルエラー:拡散した再配置ではオフセット X をエンコードできません
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 84158C42-FE79-415A-A44A-D48C9E5E6760
ms.technology: xamarin-ios
author: chamons
ms.author: chhamo
ms.date: 08/07/2019
ms.openlocfilehash: df974649c8c9f1d1fb4c13d6d801eb0f6a3d1e77
ms.sourcegitcommit: 2e5a6b8bcd1a073b54604f51538fd108e1c2a8e5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/09/2019
ms.locfileid: "68876231"
---
# <a name="compile-error-can-not-encode-offset-x-in-resulting-scattered-relocation"></a>コンパイルエラー:拡散した再配置ではオフセット X をエンコードできません

```
1>C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Xamarin\iOS\Xamarin.iOS.Common.targets(804,3): error GDC116A36: can not encode offset ‘0x1155504’ in resulting scattered relocation.
1> .long mono_aot_Xamarin_iOS_got - . + 12 (TaskId:141)
1> ^ (TaskId:141)
1>C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Xamarin\iOS\Xamarin.iOS.Common.targets(804,3): error GDC116A36: can not encode offset ‘0x11555B8’ in resulting scattered relocation.
1> .long mono_aot_Xamarin_iOS_got - . + 16 (TaskId:141)
```

この問題は、ARMv7 などの32ビットアーキテクチャのビルド時に、最終的なバイナリがネイティブツールチェーンに対して大きすぎる場合に発生します。

これを解決するには:

- [プロジェクトオプション] の [iOS ビルド] ウィンドウで、そのアーキテクチャのビルドをドロップします。
- リンクを有効にするか、コードを手動で削除して最終的な実行可能ファイルのサイズを減らします
