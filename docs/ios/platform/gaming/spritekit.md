---
title: Xamarin. iOS の SpriteKit
description: このドキュメントでは、SpriteKit、SceneKit と統合された、物理とアニメーションを組み込んだ、照明と網掛けのサポートなどを含む、Apple の2D グラフィックスフレームワークについて説明します。 SpriteKit は、2D ゲームを作成するために使用できます。
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 9e897fbd35dc48e6e51e6a7df33759ac120d1e57
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938724"
---
# <a name="spritekit-in-xamarinios"></a>Xamarin. iOS の SpriteKit

Apple の2D グラフィックスフレームワークである SpriteKit には、iOS 8 と OS X の新しい機能がいくつかあります。 これには、SceneKit、シェーダーサポート、照明、影、制約、法線マップの生成、および物理的な機能強化との統合が含まれます。 特に、新しい物理機能により、現実的な効果をゲームに追加することが非常に簡単になります。

## <a name="physics-bodies"></a>物理ボディ

SpriteKit には、2D の固定ボディ物理 API が含まれています。 各スプライトには、質量や摩擦などの物理的な特性を定義する物理ボディ () と、 `SKPhysicsBody` 物理環境の本体のジオメトリが関連付けられています。

## <a name="creating-a-physics-body-from-a-texture"></a>テクスチャからの物理ボディの作成
SpriteKit は、テクスチャからスプライトの物理ボディを派生することをサポートするようになりました。 これにより、より自然な衝突を簡単に実装できます。

たとえば、次の衝突では、バナナとサルが各イメージの表面でほぼどのように衝突しているかがわかります。

![バナナとサルの衝突は、各イメージの表面でほぼ同じです。](spritekit-images/image13.png)

SpriteKit を使用すると、1行のコードで可能な物理本体を作成できます。 `SKPhysicsBody.Create`テクスチャとサイズを使用してを単にスプライトとして呼び出します。PhysicsBody = SKPhysicsBody (スプライト)。テクスチャ、スプライト。サイズ);

## <a name="alpha-threshold"></a>アルファしきい値

テクスチャから派生したジオメトリにプロパティを直接設定するだけでなく `PhysicsBody` 、アプリケーションではとのアルファしきい値を設定して、ジオメトリの派生方法を制御できます。 

アルファしきい値は、ピクセルが結果の物理ボディに含まれるために必要な最小のアルファ値を定義します。 たとえば、次のコードでは、わずかに異なる物理ボディが生成されます。

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

次のように、アルファしきい値を調整することによって前の衝突が微調整され、バナナを使用すると、サルがフォールオーバーするようになります。

![バナナが衝突すると、サルがフォールオーバーします。](spritekit-images/image14.png)

## <a name="physics-fields"></a>物理フィールド

SpriteKit のもう1つの優れた機能は、新しい物理フィールドのサポートです。 これらを使用すると、渦のようなフィールド、放射状の重心フィールド、および spring フィールドなどをいくつでも追加できます。

物理フィールドは、SKFieldNode クラスを使用して作成されます。このクラスは、シーンに他のと同様に追加され `SKNode` ます。 には、さまざまな `SKFieldNode` 物理フィールドを作成するためのさまざまなファクトリメソッドが用意されています。 Spring field を作成するには、を呼び出すことによって、 `SKFieldNode.CreateSpringField()` 放射状重力フィールドを作成 `SKFieldNode.CreateRadialGravityField()` します。

`SKFieldNode`には、フィールドの強さ、フィールド領域、フィールドの強制減衰などのフィールド属性を制御するプロパティもあります。

## <a name="spring-field"></a>Spring Field

たとえば、次のコードでは、spring フィールドを作成してシーンに追加しています。

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

次に、 `PhysicsBody` ユーザーが画面に触れると、次のコードに示すように、物理フィールドがスプライトに影響を与えるように、スプライトを追加し、そのプロパティを設定できます。

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    var touch = touches.AnyObject as UITouch;
    var pt = touch.LocationInNode (this);
    var node = SKSpriteNode.FromImageNamed ("TinyBanana");
    node.PhysicsBody = SKPhysicsBody.Create (node.Texture, node.Size);
    node.PhysicsBody.AffectedByGravity = false;
    node.PhysicsBody.AllowsRotation = true;
    node.PhysicsBody.Mass = 0.03f;
    node.Position = pt;
    AddChild (node);
}
```

これにより、bananas は、次のように、フィールドノードの周りの spring のように oscillate されます。

![Bananas oscillate [field] ノードの周りにある spring のようになります。](spritekit-images/image15.png)

## <a name="radial-gravity-field"></a>放射状重力フィールド

別のフィールドを追加することも似ています。 たとえば、次のコードでは放射状重力フィールドが作成されます。

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

この結果、別の force フィールドが生成されます。このフィールドでは、bananas がフィールドについて放射状に引き出されます。

![Bananas は、フィールドの周囲で放射状に引き出されます。](spritekit-images/image16.png)
