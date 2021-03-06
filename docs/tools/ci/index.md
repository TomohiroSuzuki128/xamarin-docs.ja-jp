---
title: Xamarin との継続的な統合
description: このドキュメントでは、Xamarin との継続的な統合について説明しているガイドにリンクしています。 リンクされたコンテンツでは、継続的インテグレーションの概要を説明し、App Center Build、TeamCity、Jenkins について説明します。
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: davidortinau
ms.author: daortin
ms.date: 10/23/2018
ms.openlocfilehash: 9c87a65481ca58b2861c40a420459d629852f6b6
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997424"
---
# <a name="continuous-integration-with-xamarin"></a>Xamarin との継続的な統合

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## <a name="introduction-to-continuous-integration"></a>[継続的インテグレーションの概要](~/tools/ci/intro-to-ci.md)

このセクションでは、継続的インテグレーションとその関係に関連するさまざまなコンポーネントについて説明します。 ここでは、継続的な統合環境の概要を示します。詳細については、以下の特定のセクションで説明します。

## <a name="devops-with-xamarin"></a>[Xamarin を使用した DevOps](~/tools/ci/devops.md)

このセクションでは、Xamarin プロジェクトで適切に動作することを期待できる Azure と Visual Studio の DevOps 機能について説明します。

## <a name="working-with-continuous-integration-environments"></a>継続的インテグレーション環境の使用

### <a name="build-xamarin-apps-with-azure-pipelines"></a>[Azure Pipelines を使用して Xamarin アプリをビルドする](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/)

Android および iOS 用の Xamarin アプリを自動的にビルドするには、Azure Pipelines を使用します。

### <a name="build-xamarin-apps-using-app-center"></a>[App Center を使用して Xamarin アプリをビルドする](https://docs.microsoft.com/appcenter/build/xamarin/)

GitHub、Azure DevOps、または Bitbucket から直接 App Center を使用して、Xamarin と Xamarin の Android ソリューションをビルドします。

### <a name="build-xamarin-apps-with-teamcity"></a>[TeamCity で Xamarin アプリをビルドする](~/tools/ci/teamcity.md)

このガイドでは、TeamCity を使用してモバイルアプリをコンパイルし、App Center テストに送信するために必要な手順について説明します。

### <a name="build-xamarin-apps-with-jenkins"></a>[Jenkins を使用して Xamarin アプリをビルドする](~/tools/ci/jenkins-walkthrough.md)

このガイドでは、Jenkins を継続的インテグレーションサーバーとして設定し、Xamarin で作成されたモバイルアプリのコンパイルを自動化する方法について説明します。 ここでは、OS X に Jenkins をインストールし、構成し、変更がバージョン管理システムにコミットされたときに Xamarin アプリをコンパイルするようにジョブを設定する方法について説明します。
