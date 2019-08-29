---
title: Xamarin.Essentials Unit Converters
description: Xamarin.Essentials の UnitConverters クラスは単位変換機能をいくつか備えており、Xamarin.Essentials の使用時、開発者を支援します。
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
ms.openlocfilehash: be560a156647274932265597ae5b83f22255d061
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120136"
---
# <a name="xamarinessentials-unit-converters"></a>Xamarin.Essentials:Unit Converters

**UnitConverters** クラスは単位変換機能をいくつか備えており、Xamarin.Essentials の使用時、開発者を支援します。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-unit-converters"></a>Unit Converters の使用

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

単位変換機能はすべて、Xamarin.Essentials の静的 `UnitConverters` クラスの使用時に利用できます。 たとえば、華氏を摂氏に簡単に変換できます。

```csharp
var celcius = UnitConverters.FahrenheitToCelsius(32.0);
```

利用できる変換の一覧:

- FahrenheitToCelsius
- CelsiusToFahrenheit
- CelsiusToKelvin
- KelvinToCelsius
- MilesToMeters
- MilesToKilometers
- KilometersToMiles
- DegreesToRadians
- RadiansToDegrees
- DegreesPerSecondToRadiansPerSecond
- RadiansPerSecondToDegreesPerSecond
- DegreesPerSecondToHertz
- RadiansPerSecondToHertz
- HertzToDegreesPerSecond
- HertzToRadiansPerSecond
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- KilopascalsToPascals
- HectopascalsToPascals
- AtmospheresToPascals
- PascalsToAtmospheres
- CoordinatesToMiles
- CoordinatesToKilometers

## <a name="api"></a>API

- [Unit Converters のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [Unit Converters の API ドキュメント](xref:Xamarin.Essentials.UnitConverters)
