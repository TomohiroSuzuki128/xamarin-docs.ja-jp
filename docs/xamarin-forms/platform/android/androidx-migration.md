---
title: AndroidX の移行Xamarin.Forms
description: この記事では、AndroidX が存在する理由と、アプリで AndroidX に移行する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 98884003-E65A-4EB4-842D-66CFE27344A4
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 01/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c2df309a8a12a05a4b492bb66977aa2411142850
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138269"
---
# <a name="androidx-migration-in-xamarinforms"></a>AndroidX の移行Xamarin.Forms

AndroidX は、Android サポートライブラリを置き換えます。 この記事では、AndroidX が存在する理由、影響を受ける方法、 Xamarin.Forms および androidx ライブラリを使用するようにアプリケーションを移行する方法について説明します。

## <a name="history-of-androidx"></a>AndroidX の履歴

Android サポートライブラリは、古いバージョンの Android に新しい機能を提供するために作成されました。 これは、開発者が Android オペレーティングシステムのすべてのバージョンに存在しない機能を使用し、古いバージョンに対して正常なフォールバックを行うことができる互換性レイヤーです。 サポートライブラリには、便利なヘルパークラス、デバッグツールとユーティリティツール、および他のサポートライブラリクラスに依存する高度なクラスも含まれています。

サポートライブラリはもともと1つのバイナリですが、ライブラリのスイートに拡張され、進化しています。これは、最新のアプリの開発において非常に重要です。 サポートライブラリの一般的に使用される機能を次に示します。

- `Fragment`サポートクラス。
- `RecyclerView`長いリストを管理するために使用される。
- 65536を超えるメソッドを使用したアプリのマルチ dex サポート。
- `ActivityCompat` クラスです。

AndroidX はサポートライブラリに代わるものであり、これは管理されなくなりました。すべての新しいライブラリの開発が AndroidX ライブラリで行われます。 AndroidX は、セマンティックバージョン管理を使用し、パッケージ名を明確にし、一般的なアプリケーションアーキテクチャパターンのサポートを強化する、再設計されたライブラリです。 AndroidX バージョン1.0.0 は、ライブラリバージョン28.0.0 のサポートに相当するバイナリです。 サポートライブラリから AndroidX へのクラスマッピングの完全な一覧については、「 [Support library class mappings](https://developer.android.com/jetpack/androidx/migrate/class-mappings) on developer.android.com」を参照してください。

Google は、AndroidX と Jetifier という移行プロセスを作成しました。 Jetifier は、ビルドプロセス中に jar バイトコードを検査し、アプリコードと依存関係の両方で、サポートされているライブラリ参照を AndroidX と同等のものに再マップします。

アプリでは Xamarin.Forms 、Android Java アプリと同様に、jar の依存関係を AndroidX に移行する必要があります。 ただし、適切な基になる jar ファイルを指すように Xamarin バインドも移行する必要があります。 Xamarin.Formsバージョン4.5 での自動 AndroidX 移行のサポートが追加されました。

AndroidX の詳細については、developer.android.com の「 [Androidx の概要](https://developer.android.com/jetpack/androidx)」を参照してください。

## <a name="automatic-migration-in-xamarinforms"></a>での自動移行Xamarin.Forms

AndroidX に自動的に移行するには、プロジェクトで次のことを Xamarin.Forms 行う必要があります。

- Android API バージョン29以上を対象とします。
- Xamarin.Formsバージョン4.5 以上を使用します。

プロジェクトでこれらの設定を確認したら、Visual Studio 2019 で Android アプリをビルドします。 ビルドプロセス中に中間言語 (IL) が検査され、ライブラリの依存関係をサポートし、バインドが AndroidX 依存関係にスワップされます。 アプリケーションに、ビルドに必要なすべての AndroidX 依存関係がある場合、ビルドプロセスに違いはありません。

> [!NOTE]
> サポートライブラリへの参照をプロジェクトに保持する必要があります。 これらは、移行プロセスが結果の IL を検査して依存関係を変換する前に、アプリケーションをコンパイルするために使用されます。

プロジェクトに含まれていない AndroidX 依存関係が検出されると、ビルドエラーが報告され、どの AndroidX パッケージが不足しているかが示されます。 ビルドエラーの例を次に示します。

```
Could not find 37 AndroidX assemblies, make sure to install the following NuGet packages:
- Xamarin.AndroidX.Lifecycle.LiveData
- Xamarin.AndroidX.Browser
- Xamarin.Google.Android.Material
- Xamarin.AndroidX.Legacy.Supportv4
You can also copy and paste the following snippit into your .csproj file:
 <PackageReference Include="Xamarin.AndroidX.Lifecycle.LiveData" Version="2.1.0-rc1" />
 <PackageReference Include="Xamarin.AndroidX.Browser" Version="1.0.0-rc1" />
 <PackageReference Include="Xamarin.Google.Android.Material" Version="1.0.0-rc1" />
 <PackageReference Include="Xamarin.AndroidX.Legacy.Support.V4" Version="1.0.0-rc1" />
```

不足している NuGet パッケージは、Visual Studio の NuGet パッケージマネージャーを使用してインストールすることも、Android の .csproj ファイルを編集して `PackageReference` エラーに記載されている XML アイテムを含めることによってインストールすることもできます。

見つからないパッケージが解決されると、プロジェクトを再構築すると、不足しているパッケージが読み込まれます。プロジェクトは、ライブラリの依存関係をサポートするのではなく、AndroidX 依存関係を使用してコンパイルされます

> [!NOTE]
> プロジェクトとプロジェクトの依存関係が Android サポートライブラリを参照していない場合、移行プロセスでは何も行われず、実行されません。

## <a name="related-links"></a>関連リンク

- Developer.android.com の[Android サポートライブラリの概要](https://developer.android.com/topic/libraries/support-library/index)
- Developer.android.com の[Androidx の概要](https://developer.android.com/jetpack/androidx)
