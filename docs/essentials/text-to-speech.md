---
title: Xamarin.Essentials:音声合成
description: Xamarin.Essentials の TextToSpeech クラスを使用すると、アプリケーションで組み込みの音声合成エンジンを利用して、デバイスからテキストを読み上げたり、エンジンがサポートしている利用可能な言語を照会したりすることができます。
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: abe591d67ea749de4ae9a2f8dadf4df07712691a
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83149720"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials:音声合成

**TextToSpeech** クラスを使用すると、アプリケーションで組み込みの音声合成エンジンを利用して、デバイスからテキストを読み上げたり、エンジンがサポートしている利用可能な言語を照会したりすることができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-text-to-speech"></a>音声合成の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

音声合成は、テキストと省略可能なパラメーターと共に `SpeakAsync` メソッドを呼び出すことで動作し、発話が終了すると戻ります。

```csharp
public async Task SpeakNowDefaultSettings()
{
    await TextToSpeech.SpeakAsync("Hello World");

    // This method will block until utterance finishes.
}

public void SpeakNowDefaultSettings2()
{
    TextToSpeech.SpeakAsync("Hello World").ContinueWith((t) =>
    {
        // Logic that will run after utterance finishes.

    }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

このメソッドは、開始した後で発話を停止するために、オプションの `CancellationToken` を受け取ります。

```csharp
CancellationTokenSource cts;
public async Task SpeakNowDefaultSettings()
{
    cts = new CancellationTokenSource();
    await TextToSpeech.SpeakAsync("Hello World", cancelToken: cts.Token);

    // This method will block until utterance finishes.
}

// Cancel speech if a cancellation token exists & hasn't been already requested.
public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? true)
        return;

    cts.Cancel();
}
```

音声合成は、同じスレッドからの音声の要求を自動的にキューに登録します。

```csharp
bool isBusy = false;
public void SpeakMultiple()
{
    isBusy = true;
    Task.Run(async () =>
    {
        await TextToSpeech.SpeakAsync("Hello World 1");
        await TextToSpeech.SpeakAsync("Hello World 2");
        await TextToSpeech.SpeakAsync("Hello World 3");
        isBusy = false;
    });

    // or you can query multiple without a Task:
    Task.WhenAll(
        TextToSpeech.SpeakAsync("Hello World 1"),
        TextToSpeech.SpeakAsync("Hello World 2"),
        TextToSpeech.SpeakAsync("Hello World 3"))
        .ContinueWith((t) => { isBusy = false; }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

### <a name="speech-settings"></a>音声の設定

ボリューム、ピッチ、ロケールの設定を可能にする `SpeechOptions` を使用して、オーディオがどのように話されるのかをより細かく制御します。

```csharp
public async Task SpeakNow()
{
    var settings = new SpeechOptions()
        {
            Volume = .75f,
            Pitch = 1.0f
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

これらのパラメーターに使用できる値は次のとおりです。

| パラメーター | 最小要件 | 最大 |
| --- | :---: | :---: |
| ピッチ | 0 | 2.0 |
| ボリューム | 0 | 1 |

### <a name="speech-locales"></a>音声のロケール

テキストをさまざまな言語およびアクセントで読み上げるために、各プラットフォームではさまざまなロケールがサポートされています。 プラットフォームにはロケールを指定するためのさまざまなコードや方法があります。そのため、Xamarin.Essentials には、クロスプラットフォームの `Locale` クラスや、`GetLocalesAsync` でそれらを照会する方法が用意されています。

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeechOptions()
        {
            Volume = .75f,
            Pitch = 1.0f,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>制限事項

- 複数のスレッドから呼び出された場合、発話のキューは保証されません。
- バックグラウンドでのオーディオ再生は、公式にはサポートされていません。

## <a name="api"></a>API

- [TextToSpeech のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [TextToSpeech API ドキュメント](xref:Xamarin.Essentials.TextToSpeech)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Text-to-Speech-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
