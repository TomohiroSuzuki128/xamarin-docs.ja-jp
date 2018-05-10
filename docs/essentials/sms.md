---
title: Xamarin.Essentials SMS
description: Sms クラスには、指定したメッセージの受信者に送信する既定の SMS アプリケーションを開くためのアプリケーションができるようにします。
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 460aea01381934f1862946c6e17e314560e889f2
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials SMS

![プレリリース NuGet](~/media/shared/pre-release.png)

**Sms**クラスには、指定したメッセージの受信者に送信する既定の SMS アプリケーションを開くためのアプリケーションができるようにします。

## <a name="using-sms"></a>Sms を使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

SMS 機能が呼び出すことによって、`ComposeAsync`メソッド、`SmsMessage`メッセージの受信者と、どちらも省略可能なメッセージの本文を格納しています。

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, recipient);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [Sms ソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/Sms)
- [Sms API ドキュメント](xref:Xamarin.Essentials.Sms)