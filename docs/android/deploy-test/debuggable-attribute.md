---
title: デバッグ可能な属性
ms.prod: xamarin
ms.assetid: 1ABF90F1-6A70-45AE-9271-D90DC42807D0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
ms.openlocfilehash: 55de652363cb761e730d2d92f66a14a941899ca2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028109"
---
# <a name="debuggable-attribute"></a>デバッグ可能な属性

デバッグできるようにするために、Android では Java Debug Wire Protocol (JDWP) がサポートされます。 これは、JVM と通信する ADB などのツールを許可するテクノロジです。 JDWP は開発時には重要ですが、アプリケーションを公開する前に無効にする必要があります。

Android アプリケーションで JDWP を `android:debuggable` 属性の値にすることができます。 Xamarin.Android では、この属性を設定する次の方法が提供されます。

1. `AndroidManifext.xml` ファイルを作成し、そこに `android:debuggable` 属性を設定する。
2. `[assembly: Application(Debuggable=false)]` のように、`.CS` ファイルに `ApplicationAttribute` を含める。

`AndroidManifest.xml` と `ApplicationAttribute` の両方が存在する場合は、`AndroidManifest.xml` の内容が、`ApplicationAttribute` で指定されたものより優先されます。

`AndroidManifest.xml` と `ApplicationAttribute` のいずれも存在しない場合、`android:debuggable` 属性の既定値は、デバッグ シンボルが生成されているかどうかによって異なります。 デバッグ シンボルが存在する場合は、Xamarin.Android で `android:debuggable` 属性が `true` に設定されます。

`android:debuggable` 属性の値は、必ずしもビルド構成によって異なるわけではないことに注意してください。 リリース ビルドでは、`android:debuggable` 属性を true に設定することができます。

## <a name="related-links"></a>関連リンク

- [Android マーケットにおけるデバッグ可能なアプリ](https://labs.f-secure.com/archive/debuggable-apps-in-android-market/)
