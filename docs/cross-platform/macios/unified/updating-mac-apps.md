---
title: 既存の Mac アプリを更新しています
description: このドキュメントでは、Classic API から Unified API に Xamarin. Mac アプリを更新するために従う必要がある手順について説明します。
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 956d3285459d8ed93f2db84fa0bd2bb2880d3d7a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86936514"
---
# <a name="updating-existing-mac-apps"></a>既存の Mac アプリを更新しています

Unified API を使用するように既存のアプリを更新するには、アプリケーションコードで使用される名前空間と Api に加えて、プロジェクトファイル自体を変更する必要があります。

## <a name="the-road-to-64-bits"></a>64ビットへの道路

Xamarin. Mac アプリケーションから64ビットのデバイスアーキテクチャをサポートするには、統合された新しい Api が必要です。 2015年2月1日の時点で、Mac App Store へのすべての新しいアプリの送信で64ビットアーキテクチャがサポートされている必要があります。

Xamarin には、Visual Studio for Mac と Visual Studio の両方のツールが用意されており、Classic API から Unified API への移行プロセスを自動化できます。また、プロジェクトファイルを手動で変換することもできます。 自動ツールを使用することを強くお勧めしますが、この記事では両方の方法について説明します。

### <a name="before-you-start"></a>開始する前に...

既存のコードを Unified API に更新する前に、すべての*コンパイル警告*を除去することを強くお勧めします。 統合に移行すると、Classic API の多くの*警告*がエラーになります。 Classic API のコンパイラメッセージは、多くの場合、更新対象のヒントを提供するため、作業を開始する前に修正する方が簡単です。

## <a name="automated-updating"></a>自動更新

警告が修正されたら、Visual Studio for Mac または Visual Studio で既存の Mac プロジェクトを選択し、[**プロジェクト**] メニューから [ **Xamarin. Mac Unified API に移行**] を選択します。 次に例を示します。

![[プロジェクト] メニューから [Xamarin. Mac Unified API に移行する] を選択します。](updating-mac-apps-images/beta-tool1.png)

自動移行を実行する前に、この警告に同意する必要があります (当然、この adventure 着手の前にバックアップとソース管理があることを確認する必要があります)。

![自動移行を実行する前に、この警告に同意します](updating-mac-apps-images/migrate01.png)

Xamarin. Mac アプリケーションで Unified API を使用するときに選択できるターゲットフレームワークの種類として、次の2つがサポートされています。

- **Xamarin. Mac Mobile framework** -これは、完全な**デスクトップ**フレームワークのサブセットをサポートする xamarin および xamarin Android で使用されるものと同じチューニングされた .net framework です。 これは、優れたリンク動作のため、より小さな平均バイナリを提供するため、推奨されるフレームワークです。
- **Xamarin .net 4.5 framework** -このフレームワークは、**デスクトップ**フレームワークのサブセットでもあります。 ただし、**モバイル**フレームワークよりも完全な**デスクトップ**フレームワークはなく、ほとんどの NuGet パッケージまたはサードパーティ製のライブラリを使用して_作業_する必要があります。 これにより、開発者はサポートされているフレームワークを使用しながら、標準の**デスクトップ**アセンブリを使用できますが、このオプションを使用すると、より大きなアプリケーションバンドルが生成されます。 これは、サードパーティの .NET アセンブリが使用されていて、 **Xamarin. Mac Mobile framework**と互換性がない場合に推奨されるフレームワークです。 サポートされているアセンブリの一覧については、[アセンブリ](~/cross-platform/internals/available-assemblies.md)のドキュメントを参照してください。

ターゲットフレームワークと、Xamarin. Mac アプリケーションの特定のターゲットを選択した場合の影響の詳細については、[ターゲットフレームワーク](~/mac/platform/target-framework.md)のドキュメントを参照してください。 

このツールでは、次に示す「**手動で更新**する」セクションで説明されているすべての手順が基本的に自動化されており、既存の Xamarin. Mac プロジェクトを Unified API に変換するための推奨される方法です。

## <a name="steps-to-update-manually"></a>手動で更新するための手順

警告が修正されたら、次の手順に従って、新しい Unified API を使用するように Xamarin アプリを手動で更新します。

### <a name="1-update-project-type--build-target"></a>1. ビルドターゲット & プロジェクトの種類を更新します

**.Csproj**ファイルのプロジェクトフレーバーをからに変更 `42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23` し `A3F8F2AB-B479-4A4A-A458-A89E7DC349F1` ます。 テキストエディターで **.csproj**ファイルを編集し、 `<ProjectTypeGuids>` 次に示すように、要素の最初の項目を置き換えます。

![テキストエディターで .csproj ファイルを編集し、表示されているように ProjectTypeGuids 要素の最初の項目を置き換えます。](updating-mac-apps-images/csproj.png)

**Import** `Xamarin.Mac.targets` 次に示すように、を含む Import 要素をに変更 `Xamarin.Mac.CSharp.targets` します。

![図に示すように、Xamarin. Mac. ターゲットを含む Import 要素を変更します。](updating-mac-apps-images/csproj2.png)

要素の後に次のコード行を追加し `<AssemblyName>` ます。

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

例:

![AssemblyName> 要素の後に、次のコード行を追加します。 \<](updating-mac-apps-images/csproj3.png)

### <a name="2-update-project-references"></a>2. プロジェクト参照を更新する

Mac アプリケーションプロジェクトの [**参照設定**] ノードを展開します。 最初に、このスクリーンショットのような * **XamMac**の参照が表示されます (プロジェクトの種類を変更しただけなので)。

![最初に、このスクリーンショットのような XamMac の参照が表示されます。](updating-mac-apps-images/references.png)

**XamMac**エントリの横にある**歯車アイコン**をクリックし、[**削除**] を選択して、壊れている参照を削除します。

次に、**ソリューションエクスプローラー**の [**参照**] フォルダーを右クリックし、[**参照の編集**] を選択します。 参照の一覧の一番下までスクロールし、 **Xamarin. Mac**以外のチェックボックスをオンにします。

![参照の一覧の一番下までスクロールし、Xamarin. Mac 以外のチェックボックスをオンにします。](updating-mac-apps-images/references2.png)

**[OK]** をクリックして、プロジェクト参照の変更を保存します。

### <a name="3-remove-monomac-from-namespaces"></a>3. 名前空間からのモノ Mac の削除

ステートメント内の名前空間**から、** `using` または classname が完全に修飾されている場所 (たとえば、 `MonoMac.AppKit`がだけになり `AppKit` ます)。

### <a name="4-remap-types"></a>4. リマップ型

以前に使用されていたいくつかの型 (たとえば、のインスタンス) を置き換える[ネイティブ型](~/cross-platform/macios/nativetypes.md)が導入されました `System.Drawing.RectangleF` `CoreGraphics.CGRect` 。 型の完全な一覧については、「[ネイティブ型](~/cross-platform/macios/nativetypes.md)」ページを参照してください。

### <a name="5-fix-method-overrides"></a>5. メソッドのオーバーライドを修正する

一部 `AppKit` のメソッドでは、新しい[ネイティブ型](~/cross-platform/macios/nativetypes.md)(など) を使用するようにシグネチャが変更されてい `nint` ます。 カスタムサブクラスでこれらのメソッドをオーバーライドすると、署名が一致しなくなり、エラーが発生します。 ネイティブ型を使用して新しいシグネチャに一致するようにサブクラスを変更することで、これらのメソッドのオーバーライドを修正します。 

## <a name="considerations"></a>考慮事項

アプリが1つ以上のコンポーネントまたは NuGet パッケージに依存している場合は、既存の Xamarin. Mac プロジェクトを Classic API から新しい Unified API に変換する際には、次の点に注意する必要があります。 

### <a name="components"></a>コンポーネント

アプリケーションに含まれているコンポーネントも Unified API に更新する必要があります。または、コンパイルしようとすると競合が発生します。 含まれているコンポーネントについて、現在のバージョンを Xamarin コンポーネントストアの新しいバージョンに置き換えて、Unified API をサポートし、クリーンビルドを実行します。 作成者によってまだ変換されていないコンポーネントは、コンポーネントストアに32ビットのみの警告を表示します。

### <a name="nuget-support"></a>NuGet のサポート対象

Unified API サポートを利用するために NuGet に変更が加えられましたが、NuGet の新しいリリースはありませんでした。そのため、NuGet を入手して新しい Api を認識する方法を評価しています。 

この時間が経過するまでは、コンポーネントと同じように、プロジェクトに含まれているすべての NuGet パッケージを、統合された Api をサポートするバージョンに切り替え、後でクリーンビルドを実行する必要があります。

> [!IMPORTANT]
> "エラー 3" という形式のエラーが発生した場合は _、同じ Xamarin に "monomac.dll" と "Xamarin.Mac.dll" の両方を含めることはできません。 Mac プロジェクト-' Xamarin.Mac.dll ' は明示的に参照されていますが、' monomac.dll ' は ' xxx, Version = 0.0.000, Culture = 中立的, PublicKeyToken = null ' によって参照さ_れています。アプリケーションを統合 api に変換した後は、通常、Unified API に更新されていないコンポーネントまたは NuGet パッケージがプロジェクトにあることが原因 既存のコンポーネントまたは NuGet を削除し、統合された Api をサポートし、クリーンビルドを実行するバージョンに更新する必要があります。

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>Xamarin. Mac アプリの64ビットビルドを有効にする

Unified API に変換された Xamarin. Mac モバイルアプリケーションの場合でも、開発者はアプリのオプションから64ビットコンピューター用のアプリケーションのビルドを有効にする必要があります。 64ビットビルドを有効にするための詳細な手順については、 [32/64 ビットプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md)に関するドキュメントの**64 ビットビルドの有効化**に関するドキュメントを参照してください。

## <a name="finishing-up"></a>終了しています

自動または手動のいずれかの方法を使用して、Xamarin アプリケーションをクラシック Api から統合 Api に変換することを選択したかどうかにかかわらず、手動での介入が必要になるインスタンスがいくつかあります。 既知の問題と回避策については[、Unified API ドキュメントにコードを更新するためのヒント](~/cross-platform/macios/unified/updating-tips.md)を参照してください。

## <a name="related-links"></a>関連リンク

- [コードを Unified API に更新する場合のヒント](~/cross-platform/macios/unified/updating-tips.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
