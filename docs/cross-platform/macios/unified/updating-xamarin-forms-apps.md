---
title: 既存の Xamarin. Forms アプリの更新
description: このドキュメントでは、Classic API から Unified API に Xamarin のフォームアプリを更新するために従う必要がある手順について説明します。
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 620b941d7e6ac50d716916cdde18fe9aed0b0709
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86929598"
---
# <a name="updating-existing-xamarinforms-apps"></a>既存の Xamarin. Forms アプリの更新

_Unified API を使用するように既存の Xamarin. Forms アプリを更新し、バージョン1.3.1 に更新するには、次の手順に従います。_

> [!IMPORTANT]
> 1.3.1 は Unified API をサポートする最初のリリースであるため、ソリューション全体を更新して、iOS アプリを統合に移行するのと同時に最新バージョンを使用する必要があります。 これは、統合されたサポートのために iOS プロジェクトを更新するだけでなく、ソリューション内の_すべて_のプロジェクトのコードを編集する必要があることを意味します。

更新は、次の2つの手順で実行されます。

1. 移行ツールで Visual Studio for Mac のビルドを使用して、iOS アプリを Unified API に移行します。

    - 移行ツールを使用して、プロジェクトを自動的に更新します。

    - [Ios アプリを更新](~/cross-platform/macios/unified/updating-ios-apps.md)するための指示に従って Ios ネイティブ api を更新します (特にカスタムレンダラーまたは依存関係サービスコード)。

2. ソリューション全体を Xamarin. Forms バージョン1.3 に更新します。

    1. Xamarin 1.3.1 NuGet パッケージをインストールします。

    2. `App`共有コードのクラスを更新します。

    3. IOS プロジェクトのを更新し `AppDelegate` ます。

    4. Android プロジェクトのを更新し `MainActivity` ます。

    5. Windows Phone プロジェクトのを更新し `MainPage` ます。

## <a name="1-ios-app-unified-migration"></a>1. iOS アプリ (統合移行)

移行の一環として、Unified API をサポートするバージョン1.3 に Xamarin. Forms をアップグレードする必要があります。 正しいアセンブリ参照を作成するためには、まず、Unified API を使用するように iOS プロジェクトを更新する必要があります。

### <a name="migration-tool"></a>移行ツール

IOS プロジェクトをクリックし、選択された状態にして、[ **project > Migrate To Xamarin. iOS Unified API...** ] を選択し、表示される警告メッセージに同意します。

![[プロジェクト] を選択して、Xamarin. iOS Unified API に移行 > ます...表示される警告メッセージに同意します。](updating-xamarin-forms-apps-images/beta-tool1.png)

これは自動的に行われます。

- 統合された64ビット API をサポートするようにプロジェクトの種類を変更します。
- フレームワーク参照を**Xamarin. iOS**に変更します (古い**monotouch.dialog**参照を置き換えます)。
- プレフィックスを削除するには、コード内の名前空間参照を変更し `MonoTouch` ます。
- Unified API の正しいビルドターゲットを使用するように、 **.csproj**ファイルを更新します。

プロジェクトを**クリーンアップ**して**ビルド**し、他に修正するエラーがないことを確認します。 これ以上の操作は必要ありません。 これらの手順の詳細については、 [Unified API のドキュメント](~/cross-platform/macios/unified/updating-ios-apps.md)を参照してください。

### <a name="update-native-ios-apis-if-required"></a>ネイティブ iOS Api を更新する (必要な場合)

追加の iOS ネイティブコード (カスタムレンダラーや依存サービスなど) を追加している場合は、追加の手動コード修正を実行することが必要になる場合があります。 アプリを再コンパイルし、必要になる可能性のある変更の詳細については、「[既存の IOS アプリの更新](~/cross-platform/macios/unified/updating-ios-apps.md)」の手順を参照してください。 [これらのヒント](~/cross-platform/macios/unified/updating-tips.md)は、必要な変更の特定にも役立ちます。

## <a name="2-xamarinforms-131-update"></a>2. Xamarin. Forms 1.3.1 Update

IOS アプリが Unified API に更新されたら、ソリューションの残りの部分を Xamarin. Forms バージョン1.3.1 に更新する必要があります。 これには次のものが含まれます

- 各プロジェクトの Xamarin. フォーム NuGet パッケージを更新しています。
- 新しい Xamarin `Application` 、 `FormsApplicationDelegate` (iOS)、 `FormsApplicationActivity` (Android)、および `FormsApplicationPage` (Windows Phone) クラスを使用するようにコードを変更します。

以下の手順について説明します。

### <a name="21-update-nuget-in-all-projects"></a>2.1 すべてのプロジェクトの NuGet を更新する

ソリューション内のすべてのプロジェクト (PCL (存在する場合)、iOS、Android、Windows Phone) で NuGet パッケージマネージャーを使用して、Xamarin. Forms を1.3.1 に更新します。 バージョン1.3 に更新するには、Xamarin. Forms NuGet パッケージを**削除してから再度追加**することをお勧めします。

> [!NOTE]
> 現在、Xamarin. Forms バージョン1.3.1 は*プレリリース*版です。 つまり、最新のプレリリースバージョンを確認するには、NuGet の**プレリリース**オプションを選択する必要があります (Visual Studio for Mac または Visual Studio のドロップダウンリストから)。

> [!IMPORTANT]
> Visual Studio を使用している場合は、最新バージョンの NuGet パッケージマネージャーがインストールされていることを確認してください。 以前のバージョンの Visual Studio では、統合されたバージョンの Xamarin. Forms 1.3.1 が正しくインストールされません。 [**ツール] > [拡張機能と更新プログラム**] にアクセスし、[**インストール済み**] の一覧をクリックして、 **Visual Studio の NuGet パッケージマネージャー**がバージョン2.8.5 以降であることを確認します。 古いバージョンの場合は、[**更新プログラム**] の一覧をクリックして最新バージョンをダウンロードします。

NuGet パッケージを Xamarin. Forms 1.3.1 に更新したら、各プロジェクトで次の変更を行って、新しいクラスにアップグレードし `Xamarin.Forms.Application` ます。

### <a name="22-portable-class-library-or-shared-project"></a>2.2 ポータブルクラスライブラリ (または共有プロジェクト)

**App.cs**ファイルを次のように変更します。

- クラスは、 `App` から継承されるようになりました `Application` 。
- `MainPage`プロパティは、表示する最初のコンテンツページに設定されます。

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

`GetMainPage`メソッドを完全に削除し、代わりにサブクラスの `MainPage` *プロパティ*を設定しました `Application` 。

この新しい `Application` 基本クラス `OnStart` では、 `OnSleep` `OnResume` アプリケーションのライフサイクルを管理するのに役立つ、、およびオーバーライドもサポートされています。

クラスは、 `App` `LoadApplication` 次に示すように、各アプリケーションプロジェクトの新しいメソッドに渡されます。

### <a name="23-ios-app"></a>2.3 iOS アプリ

**AppDelegate.cs**ファイルを次のように変更します。

- クラスは、 `FormsApplicationDelegate` (以前のではなく) から継承さ `UIApplicationDelegate` れます。
- `LoadApplication`は、の新しいインスタンスを使用して呼び出され `App` ます。

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="23-android-app"></a>2.3 Android アプリ

**MainActivity.cs**ファイルを次のように変更します。

- クラスは、 `FormsApplicationActivity` (以前のではなく) から継承さ `FormsActivity` れます。
- `LoadApplication`は、の新しいインスタンスを使用して呼び出されます。`App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="24-windows-phone-app"></a>2.4 Windows Phone アプリ

**Mainpage.xaml** (XAML と分離コード) の両方を更新する必要があります。

**Mainpage.xaml**ファイルを次のように変更します。

- ルート XAML 要素は、である必要があり `winPhone:FormsApplicationPage` ます。
- `xmlns:phone`属性をに*変更*する必要があります。`xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

次に、更新された例を示します。これらの項目を編集するだけで済みます (残りの属性は同じままにしておく必要があります)。

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

**MainPage.xaml.cs**ファイルを次のように変更します。

- クラスは、 `FormsApplicationPage` (以前のではなく) から継承さ `PhoneApplicationPage` れます。
- `LoadApplication`は、Xamarin. Forms クラスの新しいインスタンスを使用して呼び出され `App` ます。 独自の `App` クラスが既に定義されている Windows Phone ため、この参照を完全修飾する必要がある場合があります。

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

### <a name="troubleshooting"></a>トラブルシューティング

場合によっては、Xamarin. Forms NuGet パッケージを更新した後に、次のようなエラーが表示されることがあります。 これは、NuGet アップデーターが、 **.csproj**ファイルから以前のバージョンへの参照を完全に削除しない場合に発生します。

>\_プロジェクト. .csproj: エラー: このプロジェクトは、このコンピューターに存在しない NuGet パッケージを参照しています。 NuGet パッケージの復元を有効にしてダウンロードします。  詳細については、「https://go.microsoft.com/fwlink/?LinkID=322105」を参照してください。 不足しているファイルはです。/../packages/Xamarin.Forms.1.2.3.6257/build/portable-win + net45 + wp80 + MonoAndroid10 + MonoTouch10/. (お客様の \_作品

これらのエラーを修正するには、テキストエディターで **.csproj**ファイルを開き、 `<Target` 次に示す要素など、以前のバージョンの Xamarin. Forms を参照する要素を探します。 この要素全体を **.csproj**ファイルから手動で削除し、変更を保存する必要があります。

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see https://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

これらの古い参照が削除されると、プロジェクトは正常にビルドされます。

## <a name="considerations"></a>考慮事項

このアプリが1つ以上のコンポーネントまたは NuGet パッケージに依存している場合は、既存の Xamarin. Forms プロジェクトを Classic API から新しい Unified API に変換する際には、次の点に注意する必要があります。

### <a name="components"></a>コンポーネント

アプリケーションに含まれているコンポーネントも Unified API に更新する必要があります。または、コンパイルしようとすると競合が発生します。 含まれているコンポーネントについて、現在のバージョンを Xamarin コンポーネントストアの新しいバージョンに置き換えて、Unified API をサポートし、クリーンビルドを実行します。 作成者によってまだ変換されていないコンポーネントは、コンポーネントストアに32ビットのみの警告を表示します。

### <a name="nuget-support"></a>NuGet のサポート対象

Unified API サポートを利用するために NuGet に変更が加えられましたが、NuGet の新しいリリースはありませんでした。そのため、NuGet を入手して新しい Api を認識する方法を評価しています。

この時間が経過するまでは、コンポーネントと同じように、プロジェクトに含まれているすべての NuGet パッケージを、統合された Api をサポートするバージョンに切り替え、後でクリーンビルドを実行する必要があります。

> [!IMPORTANT]
> "エラー 3" という形式のエラーが発生した場合は _、同じ Xamarin に "monotouch.dll" と "Xamarin.iOS.dll" の両方を含めることはできません。 iOS プロジェクト-' Xamarin.iOS.dll ' は明示的に参照されていますが、' monotouch.dll ' は ' xxx, Version = 0.0.000, Culture = 中立的, PublicKeyToken = null ' によって参照さ_れています。アプリケーションを統合 api に変換した後は、通常、Unified API に更新されていないコンポーネントまたは NuGet パッケージがプロジェクトにあることが原因 既存のコンポーネントまたは NuGet を削除し、統合された Api をサポートし、クリーンビルドを実行するバージョンに更新する必要があります。

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin iOS アプリの64ビットビルドを有効にする

Unified API に変換された Xamarin iOS モバイルアプリケーションでは、開発者は引き続き、アプリのオプションから64ビットコンピューター用のアプリケーションのビルドを有効にする必要があります。 64ビットビルドを有効にするための詳細な手順については、 [32/64 ビットプラットフォームに関する考慮事項](~/cross-platform/macios/32-and-64/index.md#enable-64)の**64 ビットビルドの有効化**に関するドキュメントを参照してください。

## <a name="summary"></a>まとめ

これで、1.3.1 アプリケーションがバージョンに更新され、iOS アプリが Unified API に移行されます (iOS プラットフォームで64ビットアーキテクチャがサポートされます)。

前述のように、Xamarin のフォームアプリにカスタムレンダラーや依存サービスなどのネイティブコードが含まれている場合は、 [Unified API で導入](~/cross-platform/macios/index.md)された新しい型を使用するための更新も必要になることがあります。

## <a name="related-links"></a>関連リンク

- [IOS アプリの更新](~/cross-platform/macios/unified/updating-apps.md)
- [IOS アプリの更新](~/cross-platform/macios/unified/updating-ios-apps.md)
- [クロスプラットフォーム アプリでのネイティブ型の使用](~/cross-platform/macios/native-types-cross-platform.md)
- [ヒントの更新](~/cross-platform/macios/unified/updating-tips.md)
- [クラシックと Unified API の違い](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
