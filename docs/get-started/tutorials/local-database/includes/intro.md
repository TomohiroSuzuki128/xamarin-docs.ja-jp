---
ms.openlocfilehash: 8f379e8639846d9a6424c4aaadf83ff89d7a8684
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2019
ms.locfileid: "71107283"
---
このチュートリアルを試行する前に、以下を正常に完了しておく必要があります。

- [最初の Xamarin.Forms アプリのビルド](~/get-started/first-app/index.md)のクイック スタート。
- [StackLayout](~/get-started/tutorials/stacklayout/index.yml) のチュートリアル。
- [Button](~/get-started/tutorials/button/index.yml) のチュートリアル。
- [Entry](~/get-started/tutorials/entry/index.yml) のチュートリアル。
- [ListView](~/get-started/tutorials/listview/index.yml) のチュートリアル。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - NuGet パッケージ マネージャーを使用して、SQLite.NET を Xamarin.Forms プロジェクトに追加する。
> - データ アクセス クラスを作成する。
> - データ アクセス クラスを使用する。

ローカルの SQLite.NET データベースにデータを格納する方法を示す簡単なアプリケーションを作成するには、Visual Studio 2019 または Visual Studio for Mac を使用します。 次のスクリーンショットは、最終的なアプリケーションです。

[![iOS および Android での、ローカル SQLite.NET データベースのデータ永続化のスクリーンショット](../images/consume-data-access-classes-reduced.png "ローカル データベースのデータ永続化")](../images/consume-data-access-classes-large.png#lightbox "ローカル データベースのデータ永続化")
