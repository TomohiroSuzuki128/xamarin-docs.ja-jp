---
title: Xamarin でのアラートの表示
description: このドキュメントでは、iOS 8 で導入された UIAlertController Api を使用して、Xamarin. iOS にアラートを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 1264b28b2ee56ec5de610350a199668c67d5c33c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022099"
---
# <a name="displaying-alerts-in-xamarinios"></a>Xamarin でのアラートの表示

IOS 8 以降では、UIAlertController は、Uialertcontroller と Uialertcontroller が両方とも非推奨とされるようになりました。

UIView のサブクラスである、置き換えられたクラスとは異なり、UIAlertController は UIViewController のサブクラスです。

`UIAlertControllerStyle` を使用して、表示するアラートの種類を指定します。 これらのアラートの種類は次のとおりです。

- **Uialertコントローラースタイルの Actionsheet**
  - IOS より前の8これは、UIActionSheet でした。
- **Uialertコントローラーのスタイルのアラート**
  - IOS より前の8これは、UIAlertView になりました 

警告コントローラーを作成するには、次の3つの手順を実行する必要があります。

- 次のものを使用してアラートを作成および構成します。
  - タイトル
  - message
  - preferredStyle

- Optionalテキストフィールドを追加する
- 必要なアクションを追加する
- ビューコントローラーを表示する

最も単純なアラートには、次のスクリーンショットに示すように、1つのボタンが含まれています。

 ![1つのボタンを持つアラート](alerts-images/alert1.png)

単純なアラートを表示するコードは次のとおりです。

```csharp
okayButton.TouchUpInside += (sender, e) => {

    //Create Alert
    var okAlertController = UIAlertController.Create ("Title", "The message", UIAlertControllerStyle.Alert);

    //Add Action
    okAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));

    // Present Alert
    PresentViewController (okAlertController, true, null);
};
```

複数のオプションを使用してアラートを表示する方法は似ていますが、2つのアクションが追加されます。 たとえば、次のスクリーンショットは、2つのボタンを持つアラートを示しています。

 ![2つのボタンを含むアラート](alerts-images/alert2.png)

```csharp
okayCancelButton.TouchUpInside += ((sender, e) => {

    //Create Alert
    var okCancelAlertController = UIAlertController.Create("Alert Title", "Choose from two buttons", UIAlertControllerStyle.Alert);

    //Add Actions
    okCancelAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, alert => Console.WriteLine ("Okay was clicked")));
    okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine ("Cancel was clicked")));

    //Present Alert
    PresentViewController(okCancelAlertController, true, null);
});
```

アラートには、次のスクリーンショットのような操作シートを表示することもできます。

 ![操作シートのアラート](alerts-images/alert3.png)

`AddAction` メソッドを使用して、アラートにボタンが追加されます。

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel, (action) => Console.WriteLine ("Cancel button pressed.")));

    // Required for iPad - You must specify a source for the Action Sheet since it is
    // displayed as a popover
    UIPopoverPresentationController presentationPopover = actionSheetAlert.PopoverPresentationController;
    if (presentationPopover!=null) {
        presentationPopover.SourceView = this.View;
        presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Up;
    }

    // Display the alert
    this.PresentViewController(actionSheetAlert,true,null);
});
```

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [警告コントローラー](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
