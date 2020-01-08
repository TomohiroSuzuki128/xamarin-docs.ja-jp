---
title: Microsoft Speech API を使用して、音声認識
description: Microsoft Speech API には、音声言語を処理するアルゴリズムを提供するクラウド ベースの API です。 この記事では、Microsoft の音声認識の REST API を使用して、Xamarin.Forms アプリケーションでのテキストをオーディオに変換する方法について説明します。
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: eca079972f4e46c0cf60c4749658ff9a7fe1791b
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489805"
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>Microsoft Speech API を使用して、音声認識

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Microsoft Speech API は、音声言語を処理するためのアルゴリズムを提供するクラウドベースの API です。この記事では、Microsoft 音声認識 REST API を使用して、Xamarin. フォームアプリケーションで音声をテキストに変換する方法について説明します。_

## <a name="overview"></a>の概要

Microsoft Speech API では、2 つのコンポーネントがあります。

- 話された単語をテキストに変換するための音声認識 API。 音声認識は、REST API、クライアント ライブラリ、またはサービス ライブラリを使用して実行できます。
- テキストを音声に変換するためのテキストを音声 API。 テキストの音声変換は、REST API 経由で実行されます。

この記事では、REST API 経由での音声認識を実行する方法について説明します。 クライアントとサービス ライブラリでは、部分的な結果を返すことをサポート、REST API はのみ、部分的な結果なしの 1 つの認識結果を返します。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

Microsoft Speech API を使用して API キーを取得する必要があります。 これは、Azure から取得できます[ポータル](https://portal.azure.com/)します。 詳細については、次を参照してください。 [、Azure portal で Cognitive Services アカウントの作成](/azure/cognitive-services/cognitive-services-apis-create-account)です。

Microsoft Speech API の詳細については、次を参照してください。 [Microsoft Speech API のドキュメント](/azure/cognitive-services/speech/home/)します。

## <a name="authentication"></a>認証

Microsoft Speech の REST API に加えられたすべての要求で cognitive services のトークン サービスから取得できる JSON Web トークン (JWT) アクセス トークンを必要と`https://api.cognitive.microsoft.com/sts/v1.0/issueToken`します。 トークンのサービスに POST 要求を行うことによって、トークンを取得できますを指定する、 `Ocp-Apim-Subscription-Key` API キーとしてその値を含むヘッダー。

次のコード例は、トークン、トークン サービスからのアクセスを要求する方法を示します。

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

Base64 テキストには、返されたアクセス トークンが 10 分間の有効期限です。 そのため、サンプル アプリケーションは、9 分ごと、アクセス トークンを更新します。

各 Microsoft Speech REST API でアクセス トークンを指定する必要がありますとして呼び出す、`Authorization`ヘッダー文字列で始まる`Bearer`次のコード例のように。

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Microsoft Speech の REST API に有効なアクセス トークンを渡すの失敗は、応答が 403 エラーが発生されます。

## <a name="performing-speech-recognition"></a>音声認識を実行します。

音声認識がへの POST 要求をすることで実現は、`recognition`で API を`https://speech.platform.bing.com/speech/recognition/`します。 1 つの要求は、オーディオの 10 秒以上を含めることはできませんし、要求の合計期間は 14 秒を超えることはできません。

Wav 形式の要求の POST の本文には、オーディオ コンテンツを配置する必要があります。

サンプル アプリケーションで、`RecognizeSpeechAsync`メソッドは、音声認識のプロセスを呼び出します。

```csharp
public async Task<SpeechResult> RecognizeSpeechAsync(string filename)
{
    ...

    // Read audio file to a stream
    var file = await PCLStorage.FileSystem.Current.LocalStorage.GetFileAsync(filename);
    var fileStream = await file.OpenAsync(PCLStorage.FileAccess.Read);

    // Send audio stream to Bing and deserialize the response
    string requestUri = GenerateRequestUri(Constants.SpeechRecognitionEndpoint);
    string accessToken = authenticationService.GetAccessToken();
    var response = await SendRequestAsync(fileStream, requestUri, accessToken, Constants.AudioContentType);
    var speechResult = JsonConvert.DeserializeObject<SpeechResult>(response);

    fileStream.Dispose();
    return speechResult;
}
```

PCM の wav データとしては、各プラットフォーム固有プロジェクトにオーディオを記録し、`RecognizeSpeechAsync`メソッドは、 `PCLStorage` NuGet パッケージをストリームとして音声ファイルを開きます。 URI が生成された、音声認識の要求およびアクセス トークンは、トークン サービスから取得されます。 音声認識の要求がポストされた、 `recognition` API で、結果を含む JSON 応答を返します。 JSON 応答は、表示するため、呼び出し元メソッドに返される結果を逆シリアル化です。

### <a name="configuring-speech-recognition"></a>音声認識を構成します。

音声認識のプロセスは、HTTP クエリ パラメーターを指定して構成できます。

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

によって実行される主な構成、`GenerateRequestUri`メソッドは、オーディオ コンテンツのロケールを設定します。 サポートされているロケールの一覧については、「[サポートされる言語](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/)」を参照してください。

### <a name="sending-the-request"></a>要求を送信します。

`SendRequestAsync`メソッドは、Microsoft Speech の REST API に POST 要求を出すし、応答を返します。

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);
    var response = await httpClient.PostAsync(url, content);
    return await response.Content.ReadAsStringAsync();
}
```

このメソッドは、POST 要求を作成します。

- オーディオ ストリームをラップする`StreamContent`インスタンスで、ストリームに基づく HTTP コンテンツを提供します。
- 設定、`Content-Type`への要求のヘッダー`audio/wav; codec="audio/pcm"; samplerate=16000`します。
- アクセス トークンを追加、`Authorization`文字列で始まるヘッダー`Bearer`します。

POST 要求に送信し、 `recognition` API。 応答が読み取られ、呼び出し元メソッドに返されます。

`recognition` API は、要求が有効である、要求が成功したことを示すし、の要求された情報は、応答で提供される応答には、HTTP 状態コード 200 (OK) を送信します。 想定されるエラー応答の一覧は、次を参照してください。[トラブルシューティング](/azure/cognitive-services/speech/troubleshooting)します。

### <a name="processing-the-response"></a>応答の処理

認識されたテキストに含まれていることと、JSON 形式で API の応答が返される、`name`タグ。 次の JSON データは、一般的な正常な応答メッセージを示しています。

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

サンプル アプリケーションで JSON 応答が逆シリアル化、`SpeechResult`の次のスクリーン ショットに示すように、表示のため、呼び出し元メソッドに返される結果のインスタンス。

![](speech-recognition-images/speech-recognition.png "Speech Recognition")

## <a name="summary"></a>要約

この記事では、Microsoft の音声の REST API を使用して、Xamarin.Forms アプリケーションでのテキストをオーディオに変換する方法について説明します。 音声認識を実行するだけでなく Microsoft Speech API はテキストを音声にも変換できます。

## <a name="related-links"></a>関連リンク

- [Microsoft Speech API のドキュメント](/azure/cognitive-services/speech/home/)します。
- [RESTful Web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
