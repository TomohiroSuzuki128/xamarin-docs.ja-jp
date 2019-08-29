---
title: Xamarin.iOS のウォレット機能
description: アプリケーションに機能を追加するには、多くの場合、追加のプロビジョニングの設定が必要です。 このガイドでは、ウォレット機能に必要なセットアップについて説明します。
ms.prod: xamarin
ms.assetid: BD9475E6-F586-488C-93D4-8A2A1629B99B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 8cb5d1cb29f5668194d15102c0b2fb4118388e94
ms.sourcegitcommit: 3d21bb1a6d9b78b65aa49917b545c39d44aa3e3c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70065467"
---
# <a name="wallet-capabilities-in-xamarinios"></a>Xamarin.iOS のウォレット機能

_アプリケーションに機能を追加するには、多くの場合、追加のプロビジョニングの設定が必要です。このガイドでは、ウォレット機能に必要なセットアップについて説明します。_

ウォレットは、ユーザーがチケット、搭乗券、クーポンをデバイスから表示できるバーコードや他のコンテンツを保存し、表示するアプリです。 この情報は、_パス_に格納されます。 たとえば、搭乗券または 1 枚のチケットは 1 つのパスです。 

開発者はさまざまな方法でウォレットを操作できます。

* パスを作成するために、アプリケーションを構築する必要はありません。 パスファイルは、いくつかの JSON ファイルとオプションのメタデータ ファイルを含む zip 形式のアーカイブです。 このファイルを準備するには、[パス タイプ ID](~/ios/platform/passkit.md) と[パス証明書](~/ios/platform/passkit.md)が必要です。 この情報は、JSON ファイルで宣言されます。 パスファイルをプロビジョニングする方法については、「[Introduction to PassKit](~/ios/platform/passkit.md)」 (PassKit の概要) ガイドを参照してください。

* パスを配布するため、コンパニオン アプリが作成されます。 コンパニオン アプリには、パスの作成、編集、および更新機能と、ウォレット アプリに追加する機能があります。 この種類のアプリの例として、シネマ アプリがあります。ユーザーがアプリでチケットを購入したときに、そのチケットをアプリからウォレットに直接追加できます。 コンパニオン アプリを使用するには、ウォレット機能を使用してプロビジョニング プロファイルにアプリ ID を含める必要があります。アプリ ID は、以下の手順を参照して設定することができます。 また、アプリには必要な権利も含める必要があります。

* コンジット アプリは、パスを直接操作しないアプリです。 コンジット アプリは、パスに対して最小限の操作を行い、パスを受け取り、ウォレットに追加するオプションをユーザーに与える以外の操作は行いません。 コンジット アプリは特殊なプロビジョニングや権利を必要としませんが、PassKit Framework のいくつかのメソッドを使用します。

## <a name="developer-center"></a>Developer Center

ウォレットに使用する新しいプロビジョニング プロファイルを作成するには、次の手順を実行します。

1. Apple Developer Portal の [[Certificates, Identifiers, and Profiles]](https://developer.apple.com/account/ios/certificate/)\(証明書、ID、プロファイル\) セクションに移動します。
2. **[Identifiers]** \(ID\) の **[App IDs]** \(アプリ ID\) を参照します。 
    
    ![アプリ ID の選択](wallet-capabilities-images/image17.png)

3. ページの右上にある **+** アイコンをクリックします。
4. **[Name]** とバンドル識別子を指定して新しいアプリ ID を登録します (このバンドル識別子は、プロジェクトのバンドル ID と一致する必要があります)。
   
    ![アプリ ID の詳細を追加する](wallet-capabilities-images/image18.png)

5. サービスの一覧から **[Wallet]** App Service を選択します。
    
    ![サービスの選択画面](wallet-capabilities-images/image19.png)

6. **[Continue]** 、 **[Register]** の順に押してアプリ ID を作成します。

必要に応じて、既存のアプリ ID を編集してウォレット機能を追加できます。

これでこのアプリ ID を使用して、新しいプロビジョニング プロファイルの生成または再生成ができます。手順については、[機能の使用](~/ios/deploy-test/provisioning/capabilities/index.md)に関するガイドを参照してください。

![新しく作成したアプリ ID を使用してプロビジョニング プロファイルを作成する](wallet-capabilities-images/image20.png)


ウォレットの使用の詳細については、次のガイドを参照してください。

* [PassKit の概要](~/ios/platform/passkit.md)
 
## <a name="next-steps"></a>次の手順
 
以下のリストでは、実行する必要がある可能性のある追加の手順について説明します。

* アプリでフレームワークの名前空間を使用します。
* アプリに必要な権利を追加します。 必要な権利とその追加方法については、[権利の使用](~/ios/deploy-test/provisioning/entitlements.md)に関するガイドを参照してください。
* アプリの **[iOS バンドル署名]** で、 **[カスタムの権利]** が **Entitlements.plist** に設定されていることを確認します。 これは、デバッグと iOS シミュレーターのビルドに対する既定の設定では_ありません_。

App Services で問題が発生した場合は、メイン ガイドの[トラブルシューティング](~/ios/deploy-test/provisioning/capabilities/index.md)のセクションを参照してください。
