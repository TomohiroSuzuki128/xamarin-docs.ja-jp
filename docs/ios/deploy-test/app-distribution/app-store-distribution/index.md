---
title: App Store 配布
description: このドキュメントでは、App Store で Xamarin.iOS アプリケーションを配布する方法について説明します。 配布証明書および配布プロビジョニング プロファイルの作成方法と、iTunes Connect を構成してアプリを提出する方法を説明します。
ms.prod: xamarin
ms.assetid: B07E2C1F-A6DF-43CB-BFB0-0252A5558467
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/23/2017
ms.openlocfilehash: c9882944938b2d5136d36304c4a400208a68663a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937333"
---
# <a name="app-store-distribution"></a>App Store 配布

Xamarin.iOS アプリの開発が完了したら、ソフトウェア開発ライフサイクルの次の手順は、iTunes App Store を使用してアプリをユーザーに配布することです。 これは、アプリケーションを配布する最も一般的な方法です。 Apple の App Store でアプリケーションを発行することにより、世界中のコンシューマーが使用できるようになります。

> [!IMPORTANT]
> Apple は、2019 年 3 月以降に App Store に提出されるすべてのアプリおよび更新プログラムが iOS 12.1 SDK (Xcode 10.1 以降に含まれている) でビルドされる必要があることを[通知しました](https://developer.apple.com/ios/submit/)。
> アプリでは、iPhone XS および 12.9 インチ iPad Pro の画面サイズもサポートされる必要もあります。

> [!IMPORTANT]
> Xamarin.Forms の使用時に `UIWebView` の非推奨の警告 (ITMS-90809) についてリソースを探している場合は、[Xamarin.Forms WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809) のドキュメントを参照してください。

アプリケーションを配布するには、アプリケーションの開発の場合と同じように、適切な*プロビジョニング プロファイル*を使用してアプリケーションをプロビジョニングする必要があります。 プロビジョニング プロファイルは、コード署名情報だけでなく、アプリケーションの ID と使用する配布メカニズムも含むファイルです。 App Store 以外の配布では、アプリを展開できるデバイスに関する情報も含まれています。

> [!IMPORTANT]
> iTunes Connect を使う場合、またそのためアプリを App Store に発行する場合は、お客様は個人または組織の Apple Developer Program に参加する**必要があります**。 Apple Developer **Enterprise** Program のメンバーの場合、このページの手順に従うことはできません。

<a name="provisioning"></a>

## <a name="provisioning-an-app-for-app-store-distribution"></a>App Store で配布するためのアプリのプロビジョニング

Xamarin.iOS アプリケーションをリリースするためにどのように計画しているかに関係なく、固有の*配布プロビジョニング プロファイル*をビルドする必要があります。 このプロファイルでは、iOS デバイスにインストールできるように、リリースするアプリケーションにデジタル署名することができます。 開発プロビジョニング プロファイルと同様に、配布プロファイルには以下が含まれます。

- アプリ ID
- 配布証明書

開発プロビジョニング プロファイルで使用したものと同じ**アプリ ID** と**デバイス**を選択できますが、まだ作成していないため、App Store にアプリを提出する際に組織を識別するための配布証明書を作成する必要があります。 配布証明書の作成手順については、以下のセクションで説明します。

> [!NOTE]
> 配布証明書とプロビジョニング プロファイルを作成できるのは、チーム エージェントと管理者のみです。

<a name="creatingcertificate"></a>

## <a name="creating-a-distribution-certificate"></a>配布証明書の作成

1. Apple Developer Member Center の *[Certificates, Identifiers & Profiles]\(証明書、ID、およびプロファイル\)* セクションに移動します。
2. *[Certificates]\(証明書\)* の下で **[Production]\(運用\)** を選択します。
3. **+** ボタンをクリックして、新しい証明書を作成します。
4. *[Production]\(運用\)* の見出しの下で **[App Store and Ad Hoc]\(App Store およびアドホック\)** を選択します。

    [![[App Store and Ad Hoc]\(App Store およびアドホック\) を選択する](images/createcertmanually01.png)](images/createcertmanually01.png#lightbox)
5. **[Continue]\(続行\)** をクリックし、指示に従って Keychain Access を使用して証明書署名要求を作成します。

    [![キーチェーン アクセスを使用して証明書署名要求を作成します](images/createcertmanually02.png)](images/createcertmanually02.png#lightbox)
6. 指示どおりに CSR を作成したら、 **[Continue]\(続行\)** をクリックし、CSR を Member Center にアップロードします。

    [![CSR を Member Center にアップロードします](images/createcertmanually03.png)](images/createcertmanually03.png#lightbox)

7. **[Generate]\(生成\)** をクリックして証明書を作成します。
8. 最後に、完成した証明書を**ダウンロード**し、ファイルをダブルクリックしてインストールします。
9. この時点で、証明書はコンピューターにインストールされますが、場合によっては、[プロファイルを更新](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)して、Xcode で表示されるようにする必要があります。

または、Xcode の [Preferences]\(環境設定\) ダイアログを使用して証明書を要求することができます。 この操作を行うには、次の手順に従います。

1. チームを選択し、 **[証明書の管理...** ] をクリックします。[![チーム、[View Details]\(詳細の表示\) の順に選択する](images/selectteam.png)](images/selectteam.png#lightbox)

2. 次に、 **[iOS Distribution Certificate]\(iOS 配布証明書\)** の横の **[作成]** ボタンをクリックします。[![iOS 配布証明書を作成する](images/selectcert.png)](images/selectcert.png#lightbox)

3. チーム権限に応じて、次のように署名 ID が生成されます。チーム エージェントまたは管理者が承認するまで待機する必要がある場合もあります。[![署名 ID が生成され、ダイアログが表示されます](images/generated.png)](images/generated.png#lightbox)

<a name="creatingprofile"></a>

## <a name="creating-a-distribution-profile"></a>配布プロファイルの作成

<a name="creatingappid"></a>

### <a name="creating-an-app-id"></a>アプリ ID の作成

作成する他のプロビジョニング プロファイルと同じように、ユーザーのデバイスに配布するアプリを識別するためにアプリ ID が必要になります。 ID をまだ作成していない場合は、次の手順に従って作成します。

1. [Apple Developer Center](https://developer.apple.com/account/overview.action) で *[Certificate, Identifiers and Profiles]\(証明書、ID、およびプロファイル\)* セクションを参照します。 **[Identifiers]** \(ID\) の下で **[App IDs]** \(App ID\) を選択します。
2. **+** ボタンをクリックして、ポータルで識別するための**名前**を指定します。
3. アプリのプレフィックスは、チーム ID として既に設定されており、変更できません。 明示的またはワイルドカード アプリ ID を選択し、次のように逆引き DNS 形式でバンドル ID を入力します。
    - **明示的**: com.[DomainName].[AppName]
    - **ワイルドカード**: com.[DomainName].*
4. アプリで必要な任意の [App Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning-for-application-services) を選択します。
5. **[Continue]\(続行\)** ボタンをクリックし、画面の指示に従って新しいアプリ ID を作成します。

### <a name="creating-a-provisioning-profile"></a>プロビジョニング プロファイルの作成

配布プロファイルを作成するのに必要なコンポーネントがそろったら、次の手順に従って配布プロファイルを作成します。

1. Apple Provisioning ポータルに戻り、 **[Provisioning]\(プロビジョニング\)**  >  **[Distribution]\(配布\)** の順に選択します。

    [![[Provisioning]、[Distribution] の順に選択する](images/distribute01.png)](images/distribute01.png#lightbox)

2. **+** ボタンをクリックし、**App Store** として作成する配布プロファイルの種類を選択します。

    [![App Store 配布プロファイルを作成する](images/distribute02.png)](images/distribute02.png#lightbox)

3. **[Continue]\(続行\)** ボタンをクリックし、配布プロファイルを作成するアプリ ID をドロップダウン リストから選択します。

    [![ドロップダウン リストからアプリ ID を選択します](images/distribute03.png)](images/distribute03.png#lightbox)

4. **[Continue]\(続行\)** ボタンをクリックし、アプリケーションに署名するために必要な証明書を選択します。

    [![アプリケーションに署名するために必要な証明書を選択する](images/distribute04.png)](images/distribute04.png#lightbox)

5. **[Continue]\(続行\)** ボタンをクリックし、Xamarin.iOS アプリケーションを実行できるようにする iOS を選択します。

    [![アプリを実行できるようにする iOS デバイスを選択する](images/distribute05.png)](images/distribute05.png#lightbox)

6. **[Continue]\(続行\)** ボタンをクリックし、新しい配布プロファイルの**名前**を入力します。

    [![新しい配布プロファイルの名前を入力します](images/distribute06.png)](images/distribute06.png#lightbox)

7. **[Generate]\(生成\)** ボタンをクリックし、新しいプロファイルを作成してプロセスを終了します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

 Visual Studio for Mac で新しい配布プロファイルを使用可能にするには、Visual Studio for Mac を終了し、(「[Requesting Signing Identities](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)」 (署名 ID の要求) セクションの手順に従って) Xcode で使用可能な署名 ID とプロビジョニング プロファイルのリストを更新する必要があります。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

 Visual Studio で新しい配布プロファイルを使用可能にするには、Visual Studio を終了して、([署名 ID の要求](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)に関するセクションの手順に従って、ビルド ホストの Mac で) Xcode で使用可能な署名 ID とプロビジョニング プロファイルのリストを更新する必要があります。

-----

<a name="selectprofile"></a>

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Xamarin.iOS プロジェクトでの配布プロファイルの選択

iTunes App Store の販売向けの Xamarin.iOS アプリケーションの最終ビルドを行う準備ができたら、作成済みの配布プロファイルを選択します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

 Visual Studio for Mac で、次の操作を行います。

1. **ソリューション エクスプローラー**でプロジェクト名をダブルクリックして、編集用に開きます。
2. **[iOS バンドル署名]** を選択し、 **[構成]** ドロップダウン リストから **[Release | iPhone]\(リリース | iPhone\)** を選択します。

    ![[構成] ドロップダウン リストから [Release | iPhone]\(リリース | iPhone\) を選択する](images/releasexs01.png)
3. ほとんどの場合、 **[署名 ID]** と **[プロビジョニング プロファイル]** は既定値の **[自動]** のままにしておいてかまいません。Visual Studio for Mac は、Info.plist のバンドル識別子に従って適切なプロファイルを選択します。

    ![既定値の [自動] に設定された署名 ID とプロビジョニング プロファイル](images/releasexs02.png)
4. 必要に応じて、ドロップダウン リストから署名 ID と配布プロファイル (前の手順で作成したもの) を選択します。

    ![署名 ID と配布プロファイルを選択する](images/releasexs03.png)
5. **[OK]** ボタンをクリックして、変更を保存します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

 Visual Studio で、次の操作を行います。

1. **ソリューション エクスプローラー**でプロジェクト名を右クリックし、 **[プロパティ]** を選択して編集用に開きます。
2. **[iOS バンドル署名]** を選択し、 **[構成]** ドロップダウン リストから **[Release | iPhone]\(リリース | iPhone\)** を選択します。

    ![[構成] ドロップダウン リストから [Release | iPhone]\(リリース | iPhone\) を選択する](images/releasevs01.png)
3. ほとんどの場合、 **[署名 ID]** と **[プロビジョニング プロファイル]** は既定値の **[自動]** のままにしておいてかまいません。Visual Studio は、Info.plist のバンドル識別子に従って適切なプロファイルを選択します。

    ![既定値の [自動] に設定された署名 ID とプロビジョニング プロファイル](images/releasevs02.png)
4. 必要に応じて、ドロップダウン リストから署名 ID と配布プロファイル (前の手順で作成したもの) を選択します。

    ![署名 ID と配布プロファイルを選択します](images/releasevs03.png)
5. プロジェクトのプロパティへの変更を保存します。

-----

<a name="itunesconnect"></a>

## <a name="configuring-your-application-in-itunes-connect"></a>iTunes Connect でのアプリケーションの構成

アプリケーションが正常にプロビジョニングされたら、次の手順では、特に、App Store で iOS アプリケーションを管理するための一連の Web ベース ツールである、[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) でアプリを構成します。

Xamarin.iOS アプリケーションをレビューのために Apple に提出し、最終的に App Store で販売または無償アプリとしてリリースする前に、アプリケーションを正しく設定して、iTunes Connect で構成する必要があります。

詳細については、「[Configuring an App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)」 (iTunes Connect でのアプリの構成) ドキュメントを参照してください。

<a name="submitting"></a>

## <a name="submitting-an-app-to-itunes-connect"></a>ITunes Connect へのアプリの提出

配布プロビジョニング プロファイルを使用してアプリケーションが署名され、iTunes Connect でアプリが作成されると、アプリケーション バイナリがレビューのために Apple にアップロードされます。 Apple で正常にレビューされると、App Store で使用できるようになります。

App Store へのアプリケーションの発行の詳細については、「[App Store に発行する](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)」を参照してください。

<a name="windows"></a>

## <a name="automatically-copy-app-bundles-back-to-windows"></a>.app バンドルを自動的に Windows にコピーする

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="summary"></a>まとめ

ここでは、App Store で Xamarin.iOS アプリケーションを配布するための準備を行う際に重要なコンポーネントについて説明します。

## <a name="related-links"></a>関連リンク

- [iTunes Connect でのアプリの構成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [社内配布](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [アドホック配布](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist ファイル](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA のサポート](~/ios/deploy-test/app-distribution/ipa-support.md)
