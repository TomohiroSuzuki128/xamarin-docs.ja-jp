---
ms.openlocfilehash: d46968f9c53314abe561e7f4871cfbf6e07b7002
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "61047858"
---
## <a name="firewall-configuration"></a>ファイアウォールの構成

テストを Xamarin Test Cloud に送信するためには、テストを送信するコンピューターが、Test Cloud サーバーと通信できる必要があります。 ファイアウォールを、ポート 80 と 443 の **testcloud.xamarin.com** のサーバーの往復のネットワークトラフィックを許可するように構成する必要があります。 このエンドポイントは DNS によって管理され、IP アドレスは変更されることがあります。 

場合によっては、テスト (または、テストを実行しているデバイス) がファイアウォールで保護された web サーバーと通信する必要があります。 その場合は、ファイアウォールを、次の IP アドレスからのトラフィックを許可するように構成する必要があります。

* **195.249.159.238**
* **195.249.159.239**
