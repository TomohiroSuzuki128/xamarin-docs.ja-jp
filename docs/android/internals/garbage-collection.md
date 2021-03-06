---
title: ガベージ コレクション
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: f8a64a68be042268860889357e46b42612795635
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84567958"
---
# <a name="garbage-collection"></a>ガベージ コレクション

Xamarin Android では、Mono の[単純な世代のガベージコレクター](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/)を使用します。 これは、2種類のコレクションを持つ、2つのジェネレーションと*大きなオブジェクト空間*を持つマークアンドスイープガベージコレクターです。 

- マイナーコレクション (Gen0 ヒープの収集) 
- 主要なコレクション (Gen1 と大きなオブジェクト空間ヒープを収集します)。 

> [!NOTE]
> GC を介した明示的なコレクションが存在しない場合[。Collect ()](xref:System.GC.Collect)コレクションは、ヒープ割り当て*に基づいて要求時に*行われます。 *これは参照カウントシステムではありません*。未処理の参照がない場合、またはスコープが終了した場合は、すぐにオブジェクト*が収集されません*。 GC は、新しい割り当てのためにマイナーヒープのメモリが不足したときに実行されます。 割り当てられていない場合は、実行されません。

マイナーコレクションは安価で頻繁に使用され、最近割り当てられたオブジェクトと配信されていないオブジェクトを収集するために使用されます。 マイナーコレクションは、割り当てられたオブジェクトの数 MB ごとに実行されます。 マイナーコレクションは、GC を呼び出すことによって手動で実行でき[ます。Collect (0)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

主要なコレクションはコストが高く、頻度が低く、すべての配信不能オブジェクトを回収するために使用されます。 メジャーコレクションは、ヒープのサイズを変更する前に、現在のヒープサイズに対してメモリが使い果たされると実行されます。 メジャーコレクションは、GC を呼び出すことによって手動で実行でき[ます。Collect ()](xref:System.GC.Collect)を呼び出すか、GC を呼び出し[ます。Collect (int)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_)を引数 GC と共に使用[します。MaxGeneration](xref:System.GC.MaxGeneration)。 

## <a name="cross-vm-object-collections"></a>クロス VM オブジェクトコレクション

オブジェクトの種類には、3つのカテゴリがあります。

- **マネージオブジェクト**: system.string[から](xref:Java.Lang.Object)継承*されていない*型 ( [system.string](xref:System.String)など)。 
    これらは通常、GC によって収集されます。 

- **Java オブジェクト**: ANDROID ランタイム VM 内に存在しても Mono vm には公開されていない java 型。 これらは退屈なものであり、それ以上については説明しません。 これらは、通常、Android ランタイム VM によって収集されます。 

- **ピアオブジェクト**: [IJavaObject](xref:Android.Runtime.IJavaObject)を実装する型、たとえば、[すべての](xref:Java.Lang.Object)java.lang.throwable サブクラスと[Java.Lang.Throwable](xref:Java.Lang.Throwable)サブクラスです。 これらの型のインスタンスには、*マネージピア*と*ネイティブピア*の2つの "halfs" があります。 マネージピアは C# クラスのインスタンスです。 ネイティブピアは Android ランタイム VM 内の Java クラスのインスタンスであり、C# の[IJavaObject](xref:Android.Runtime.IJavaObject.Handle)プロパティには、ネイティブピアへの JNI グローバル参照が含まれています。 

ネイティブピアには、次の2種類があります。

- **フレームワークピア**: "Normal" Java の種類では、Xamarin Android の何も認識しません。たとえば、  [android. コンテンツコンテキスト](xref:Android.Content.Context)。

- **ユーザーピア**: アプリケーション内に存在する各 Java lang.ini オブジェクトサブクラスのビルド時に生成される、 [Android 呼び出し可能ラッパー](~/android/platform/java-integration/working-with-jni.md) 。

Xamarin. Android プロセス内には2つの Vm が存在するため、次の2種類のガベージコレクションがあります。

- Android ランタイムコレクション 
- Mono コレクション 

Android ランタイムコレクションは正常に動作しますが、注意点があります。 JNI のグローバル参照は、GC ルートとして扱われます。 その結果、Android ランタイム VM オブジェクトを保持している JNI グローバル参照が存在する場合、そのオブジェクトがコレクションに適合していても、そのオブジェクトを収集する*ことはできません*。

Mono コレクションは、楽しいものです。 管理オブジェクトは正常に収集されます。 ピアオブジェクトを収集するには、次の手順を実行します。

1. Mono コレクションの対象となるすべてのピアオブジェクトの JNI グローバル参照は、JNI の弱いグローバル参照に置き換えられます。 

2. Android ランタイム VM GC が呼び出されます。 ネイティブピアインスタンスはすべて収集できます。 

3. (1) で作成された JNI 弱いグローバル参照がチェックされます。 弱い参照が収集されている場合は、ピアオブジェクトが収集されます。 弱い参照が収集されて*い*ない場合、弱い参照は JNI グローバル参照に置き換えられ、ピアオブジェクトは収集されません。 注: API 14 以降では、から返される値は GC 後に変更される可能性があることを意味します `IJavaObject.Handle` 。 

この結果、ピアオブジェクトのインスタンスは、いずれかのマネージコード (変数に格納されているなど) によって参照されている `static` か、Java コードによって参照されている限り、有効になります。 また、ネイティブピアは、ネイティブピアとマネージピアの両方が収集可能になるまで収集されないので、ネイティブピアの有効期間は、それ以外の場合よりも長く拡張されます。

## <a name="object-cycles"></a>オブジェクトサイクル

ピアオブジェクトは、Android ランタイムと Mono VM の両方に論理的に存在します。 たとえば、 [android](xref:Android.App.Activity)の管理対象ピアインスタンスには、対応する[android](https://developer.android.com/reference/android/app/Activity.html) .Net framework ピアの Java インスタンスが含まれます。 [Java. オブジェクト](xref:Java.Lang.Object)から継承されるすべてのオブジェクトは、両方の vm 内に表現を持つことが想定されます。 

両方の Vm で表現を持つすべてのオブジェクトの有効期間は、1つの VM 内にのみ存在するオブジェクト (など) と比較して拡張され [`System.Collections.Generic.List<int>`](xref:System.Collections.Generic.List%601) ます。 [GC を呼び出しています。収集](xref:System.GC.Collect)では、必ずしもこれらのオブジェクトが収集されることはありません。 ANDROID GC では、オブジェクトを収集する前に、いずれの VM からも参照されないようにする必要があります。 

オブジェクトの有効期間を短縮するには、 [Java.. object. Dispose ()](xref:Java.Lang.Object.Dispose)を呼び出す必要があります。 これにより、グローバル参照を解放することによって、2つの Vm 間でオブジェクトの接続を手動で "サーバー" します。これにより、オブジェクトをより迅速に収集できます。 

## <a name="automatic-collections"></a>自動コレクション

[リリース 4.1.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/mono_for_android_4/mono_for_android_4.1.0/index.md)以降、Xamarin. Android では、しきい値を超えた場合に、完全な GC が自動的に実行されます。 このしきい値は、プラットフォームの既知の最大 grefs の90% であり、エミュレーターの 1800 grefs (2000 max)、およびハードウェア上の 46800 grefs (最大 52000) です。 *注:* Xamarin Android では、 [android. ランタイム](xref:Android.Runtime.JNIEnv)によって作成された grefs のみがカウントされ、プロセスで作成された他の grefs は認識されません。 これはヒューリスティックに*すぎ*ません。 

自動コレクションが実行されると、次のようなメッセージがデバッグログに出力されます。

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

この出現は非決定的であり、悪い時 (グラフィックスレンダリングの途中など) で発生する可能性があります。 このメッセージが表示された場合は、他の場所で明示的にコレクションを実行するか、[ピアオブジェクトの有効期間を短縮](#Helping_the_GC)することをお勧めします。 

## <a name="gc-bridge-options"></a>GC ブリッジオプション

Android と android ランタイムを使用して、透過的なメモリ管理機能を提供します。 これは、 *GC ブリッジ*と呼ばれる Mono ガベージコレクターの拡張機能として実装されます。 

GC ブリッジは、Mono ガベージコレクション中に動作し、Android ランタイムヒープで "活性" を確認する必要があるピアオブジェクトを識別します。 GC ブリッジは、次の手順を順番に実行して、この決定を行います。

1. 到達できないピアオブジェクトの mono 参照グラフを、そのオブジェクトが表す Java オブジェクトに強制的に挿入します。 

2. Java GC を実行します。

3. どのオブジェクトが本当に停止しているかを確認します。 

この複雑なプロセスは、のサブクラスが任意のオブジェクトを自由に参照できるようにすることです。これにより、 `Java.Lang.Object` Java オブジェクトを C# にバインドできる制限がなくなります。 この複雑さにより、ブリッジプロセスは非常にコストが高くなり、アプリケーションで顕著な一時停止が発生する可能性があります。 アプリケーションで大幅な一時停止が発生している場合は、次の3つの GC ブリッジ実装のいずれかを調査する価値があります。 

- **Tarjan** - [Robert tarjan のアルゴリズムと後方参照の伝達](https://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm)に基づく GC ブリッジのまったく新しい設計。
    シミュレートされたワークロードで最高のパフォーマンスが得られますが、試験的なコードの共有も大きくなります。 

- **新**機能: 元のコードの大きな見直しで、2つの二次動作のインスタンスを修正し、コアアルゴリズムを維持します (強い接続コンポーネントを検出するための[Kosaraju のアルゴリズム](https://en.wikipedia.org/wiki/Kosaraju's_algorithm)に基づいています)。 

- **Old** -元の実装 (最も安定したものと見なされます)。 これは、 `GC_BRIDGE` 一時停止が許容される場合にアプリケーションが使用する必要があるブリッジです。 

どの GC ブリッジが最適に動作するかを判断する唯一の方法は、アプリケーションを試して出力を分析することです。 ベンチマークのためにデータを収集するには、次の2つの方法があります。 

- **ログ記録を有効**にする-各 GC ブリッジオプションについて、[構成](~/android/internals/garbage-collection.md)セクションで説明されているようにログ記録を有効にし、各設定のログ出力をキャプチャして比較します。 `GC`各オプションのメッセージ (特にメッセージ) を検査し `GC_BRIDGE` ます。 非対話型のアプリケーションでは、最大150ミリ秒の一時停止が許容されますが、非常に対話型のアプリケーション (ゲームなど) の場合、60ミリ秒を超えると問題が発生します。 

- **ブリッジアカウンティングを有効にする**-ブリッジアカウンティングには、ブリッジプロセスに含まれる各オブジェクトが指すオブジェクトの平均コストが表示されます。 この情報をサイズで並べ替えると、余分なオブジェクトが最も多く保持されているかどうかを示すヒントが表示されます。 

既定の設定は、 **Tarjan**です。 回帰が見つかった場合は、このオプションを**Old**に設定する必要があります。 また、 **Tarjan**によってパフォーマンスが向上しない場合は、より安定した**古い**オプションを使用することもできます。

アプリケーションで使用するオプションを指定するには、、 `GC_BRIDGE` `bridge-implementation=old` 、 `bridge-implementation=new` または `bridge-implementation=tarjan` 環境変数にを渡し `MONO_GC_PARAMS` ます。 これは、の**ビルドアクション**を使用して、プロジェクトに新しいファイルを追加することで実現され `AndroidEnvironment` ます。 次に例を示します。 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

詳細については、[構成](#configuration)に関するページを参照してください。

<a name="Helping_the_GC"></a>

## <a name="helping-the-gc"></a>GC のサポート

GC でメモリ使用量と収集時間を短縮するには、複数の方法があります。

### <a name="disposing-of-peer-instances"></a>ピアインスタンスの破棄

GC にはプロセスの不完全なビューがあり、メモリが不足していることが GC で認識されないため、メモリが不足している場合は実行されない可能性があります。 

たとえば、 [Java. Lang. オブジェクト](xref:Java.Lang.Object)型または派生型のインスタンスのサイズは少なくとも20バイトです (予告なしに変更される可能性があります)。 
[マネージ呼び出し可能ラッパー](~/android/internals/architecture.md)は、追加のインスタンスメンバーを追加しません。そのため、10 mb のメモリの blob を参照する[android の Graphics. Bitmap](xref:Android.Graphics.Bitmap)インスタンスがある場合、Xamarin は、gc が20バイトのオブジェクトを参照することを認識せず、 &ndash; 10 mb のメモリを保持している android ランタイムに割り当てられたオブジェクトにリンクされていること 

多くの場合、GC を支援する必要があります。 残念ながら、 *GC です。AddMemoryPressure ()* および*GC。RemoveMemoryPressure ()* はサポートされていないので、Java で割り当てられた大きなオブジェクトグラフを解放したことが*わかっ*ている場合は、手動で GC を呼び出す必要があり[ます。収集 ()](xref:System.GC.Collect)を実行して、java 側のメモリを解放するよう GC に要求します。または、 *Java の lang.ini*を明示的に破棄して、マネージ呼び出し可能ラッパーと java インスタンス間のマッピングを中断することもできます。 例については、「 [Bug 1084](https://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6)」を参照してください。 

> [!NOTE]
> サブクラスのインスタンスを破棄する場合は *、細心の注意が*必要 `Java.Lang.Object` です。

メモリが破損する可能性を最小限に抑えるには、を呼び出すときに、次のガイドラインを確認して `Dispose()` ください。

#### <a name="sharing-between-multiple-threads"></a>複数のスレッド間での共有

*Java またはマネージ*インスタンスが複数のスレッド間で共有されている可能性がある場合は、これ**まで**は d ではあり*ません `Dispose()` *。 例えば[`Typeface.Create()`](xref:Android.Graphics.Typeface.Create*) 
キャッシュされた*インスタンス*を返す場合があります。 複数のスレッドが同じ引数を提供する場合は、*同じ*インスタンスを取得します。 その結果、あるスレッドからインスタンスを使用すると、 `Dispose()` 他のスレッドが無効に `Typeface` なる可能性があります。これにより、 `ArgumentException` インスタンスが別の `JNIEnv.CallVoidMethod()` スレッドから破棄されたため、が (その他の) から発生することがあります。 

#### <a name="disposing-bound-java-types"></a>バインドされた Java 型の破棄

インスタンスがバインドされた Java 型の場合、インスタンスを*マネージコードから*再利用できず、java インスタンスをスレッド間で共有できない*限り*、インスタンスを破棄できます (前の説明を参照してください `Typeface.Create()` )。 (この決定を行うことは困難な場合があります)。次に Java インスタンスがマネージコードに入ると、*新しい*ラッパーが作成されます。 

これは、次のような場合に便利です。

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

[CreateFromPath ()](xref:Android.Graphics.Drawables.Drawable.CreateFromPath*)が返すピアは、ユーザーピアでは*なく*、フレームワークピアを参照するため、上記は安全です。 `Dispose()`ブロックの末尾での呼び出しに `using` より、マネージ[ド](xref:Android.Graphics.Drawables.Drawable)コードインスタンスとフレームワークの描画可能[Drawable](https://developer.android.com/reference/android/graphics/drawable/Drawable.html)インスタンスの間のリレーションシップが解除され、Android ランタイムで必要になったときに Java インスタンスをすぐに収集できるようになります。 ピアインスタンスがユーザーピアを指している場合、これは安全では*ありません*。ここでは、が*know* `Drawable` ユーザーピアを参照できず、その呼び出しが安全であることを知るために "外部" 情報を使用してい `Dispose()` ます。 

#### <a name="disposing-other-types"></a>その他の型の破棄 

インスタンスが Java 型のバインドではない型 (カスタムなど) を参照している場合 `Activity` 、 **DO NOT** `Dispose()` そのインスタンスでオーバーライドされたメソッドを呼び出す java コードがないことが*わかっ*ている場合を除き、を呼び出すことはできません。 そうしない[ `NotSupportedException` と、が発生します。](~/android/internals/architecture.md#Premature_Dispose_Calls) 

たとえば、カスタムクリックリスナーがある場合は、次のようになります。

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

このインスタンスは、今後 Java がメソッドを呼び出そうとするため、破棄*しないでください*。

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```

#### <a name="using-explicit-checks-to-avoid-exceptions"></a>例外を回避するための明示的なチェックの使用

JNI[のオーバーロードメソッドを実装](xref:Java.Lang.Object.Dispose*)している場合は、関係するオブジェクトに触れないようにしてください。 このようにすると、既にガベージコレクトされている基になる Java オブジェクトにコードから (致命的) アクセスを試みることができるように、*ダブル破棄*の状況が発生する可能性があります。 これを行うと、次のような例外が発生します。 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

この状況は、多くの場合、オブジェクトの最初の破棄によってメンバーが null になると発生します。その後、この null メンバーに対する後続のアクセス試行によって例外がスローされます。 具体的には、オブジェクトの `Handle` (マネージインスタンスを基になる java インスタンスにリンクする) は、最初の dispose で無効になりますが、マネージコードは、使用できなくなった場合でも、この基になる java インスタンスにアクセスしようとします (java インスタンスとマネージインスタンスのマッピングの詳細については、「[マネージ呼び出し可能ラッパー](~/android/internals/architecture.md#Managed_Callable_Wrappers) 

この例外を回避するには、 `Dispose` マネージインスタンスと基になる Java インスタンスの間のマッピングがまだ有効であることをメソッドで明示的に検証します。つまり、そのメンバーにアクセスする前に、オブジェクトのが null () かどうかを確認し `Handle` `IntPtr.Zero` ます。 たとえば、次の `Dispose` メソッドはオブジェクトにアクセスし `childViews` ます。 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);
        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

初期の dispose パスによってが無効になると、ループアクセスによってが `childViews` `Handle` `for` スローされ `ArgumentException` ます。 最初のアクセスの前に明示的な null チェックを追加することにより、 `Handle` `childViews` 次の `Dispose` メソッドは例外が発生しないようにします。 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);

        // Check for a null handle:
        if (this.childViews.Handle == IntPtr.Zero)
            return;

        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

### <a name="reduce-referenced-instances"></a>参照されるインスタンスを減らす

`Java.Lang.Object`GC 中に型またはサブクラスのインスタンスがスキャンされるたびに、インスタンスが参照する*オブジェクトグラフ*全体もスキャンされる必要があります。 オブジェクトグラフは、"ルートインスタンス" が参照するオブジェクトインスタンスのセット、*および*ルートインスタンスが参照するすべてのものを再帰的に参照します。 

次のクラスについて考えてみます。

```csharp
class BadActivity : Activity {

    private List<string> strings;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

が構築されると、 `BadActivity` オブジェクトグラフには `BadActivity` `strings` `string[]` `strings` 、インスタンスがスキャンされるたびに*すべて*のインスタンスをスキャンする必要がある10004インスタンス (1x、1x、10000x 文字列インスタンスによって保持されているインスタンス) が格納され `BadActivity` ます。 

これにより、収集時間が悪影響を受ける可能性があり、その結果、GC の一時停止時間が増加します。 

ユーザーピアインスタンスによってルートされるオブジェクトグラフのサイズを*小さく*することで、GC を支援できます。 上記の例では、これを実行するには、次のように、 `BadActivity.strings` Java. Lang. オブジェクトから継承しない別のクラスに移動します。 

```csharp
class HiddenReference<T> {

    static Dictionary<int, T> table = new Dictionary<int, T> ();
    static int idgen = 0;

    int id;

    public HiddenReference ()
    {
        lock (table) {
            id = idgen ++;
        }
    }

    ~HiddenReference ()
    {
        lock (table) {
            table.Remove (id);
        }
    }

    public T Value {
        get { lock (table) { return table [id]; } }
        set { lock (table) { table [id] = value; } }
    }
}

class BetterActivity : Activity {

    HiddenReference<List<string>> strings = new HiddenReference<List<string>>();

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

## <a name="minor-collections"></a>マイナーコレクション

マイナーコレクションは、GC を呼び出すことによって手動で実行でき[ます。(0) を収集](xref:System.GC.Collect)します。 マイナーコレクションは安価ですが (主要なコレクションと比較した場合)、大幅な固定コストが発生します。そのため、頻繁にトリガーしないようにし、数ミリ秒の一時停止時間を設定する必要があります。 

同じ処理が何度も行われる "デューティサイクル" がアプリケーションにある場合は、デューティサイクルが終了したら、手動でマイナーコレクションを実行することをお勧めします。 デューティサイクルの例を次に示します。 

- 1つのゲームフレームのレンダリングサイクル。
- 特定のアプリダイアログとの相互作用 (開く、塗りつぶし、終了) 
- アプリケーションデータを更新または同期するためのネットワーク要求のグループ。

## <a name="major-collections"></a>主要なコレクション

メジャーコレクションは、GC を呼び出すことによって手動で実行でき[ます。()](xref:System.GC.Collect)または `GC.Collect(GC.MaxGeneration)` を収集します。 

512 MB のヒープを収集するときに、Android スタイルのデバイスで1秒間に一時停止することがあります。 

メジャーコレクションは、次の場合にのみ手動で呼び出す必要があります。 

- 時間のかかるデューティサイクルの終わりに、長い一時停止によってユーザーに問題が表示されない場合。 

- オーバーライドされた[Android. Activity. OnLowMemory ()](xref:Android.App.Activity.OnLowMemory)メソッド内。 

## <a name="diagnostics"></a>診断

グローバル参照がいつ作成および破棄され[*たかを*](~/android/troubleshooting/index.md)追跡するには、 ["システムの](~/android/troubleshooting/index.md)プロパティ" プロパティを[*設定し*](~/android/troubleshooting/index.md)ます。 

## <a name="configuration"></a>構成

Xamarin. Android ガベージコレクターは、環境変数を設定することによって構成できます `MONO_GC_PARAMS` 。 環境変数は、 [Androidenvironment](~/android/deploy-test/environment.md)のビルドアクションを使用して設定できます。

`MONO_GC_PARAMS`環境変数は、次のパラメーターのコンマ区切りのリストです。 

- `nursery-size` = *size* : 新世代のサイズを設定します。 サイズはバイト単位で指定され、2の累乗でなければなりません。 サフィックス `k` とは `m` 、 `g` それぞれキロバイト、メガバイト、ギガバイトを指定するために使用できます。 新世代は最初の世代 (2) です。 新世代が大きいほど、通常はプログラムが高速化されますが、より多くのメモリが使用されます。 既定の新世代サイズは 512 kb です。 

- `soft-heap-limit` = *サイズ*: アプリで使用される、ターゲットの最大マネージメモリ消費量。 メモリ使用量が指定された値を下回る場合、GC は実行時間 (コレクションが減少) に最適化されます。 
    この制限を超えると、GC はメモリ使用量 (コレクションの数が増えます) に合わせて最適化されます。 

- `evacuation-threshold` = *threshold* : 退避しきい値をパーセント単位で設定します。 値は 0 ~ 100 の範囲の整数である必要があります。 既定値は66です。 コレクションのスイープフェーズによって、特定のヒープブロックの種類の占有率がこの割合よりも小さいことが判明した場合、次のメジャーコレクションでそのブロックの種類のコレクションをコピーします。これにより、占有率が100% に近い値に復元されます。 値0は、退避をオフにします。 

- `bridge-implementation` = *ブリッジの実装*: gc のパフォーマンスの問題に対処するために、gc ブリッジオプションが設定されます。 有効な値には、 *old* 、 *new* 、 *tarjan*の3つがあります。

- `bridge-require-precise-merge`: 1 月のブリッジには最適化が含まれています。これにより、まれにオブジェクトがガベージになった後に1つの GC が収集される可能性があります。 このオプションを含めると、その最適化が無効になり、Gc が予測可能になりますが、処理速度が低下する可能性があります。

たとえば、ヒープサイズの上限が 128 MB になるように GC を構成するには、の**ビルドアクション**を含む新しいファイルをプロジェクトに追加し `AndroidEnvironment` ます。 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
