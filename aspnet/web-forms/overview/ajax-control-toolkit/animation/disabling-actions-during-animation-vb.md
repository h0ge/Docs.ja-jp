---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: アニメーション (VB) 中のアクションを無効化 |Microsoft ドキュメント
author: wenz
description: アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 アクションもサポートしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 9e2a0517800e90788bb67c1d75482a3d9340674b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868647"
---
<a name="disabling-actions-during-animation-vb"></a>アニメーション (VB) 中のアクションを無効にします。
====================
によって[Christian Wenz](https://github.com/wenz)

[コードをダウンロードする](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 マウス クリックしてなどの操作もサポートします。 ただし、マウスのクリックでは、アニメーションを開始、ときに、アニメーションの中にマウスのクリックを無効にすることをお勧めします。


## <a name="overview"></a>概要

アニメーション コントロール、ASP.NET AJAX コントロール Toolkit ではなくコントロールだけアニメーションをコントロールに追加するために全体のフレームワークです。 マウス クリックしてなどの操作もサポートします。 ただし、マウスのクリックでは、アニメーションを開始、ときに、アニメーションの中にマウスのクリックを無効にすることをお勧めします。

## <a name="steps"></a>手順

まず、含める、 `ScriptManager` ; ページで次に、ASP.NET AJAX ライブラリが読み込まれるコントロール ツールキットを使用できるようにします。

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

アニメーションは、次のように HTML ボタンに適用されます。

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

ポストバック; を作成するボタンたくないので、Web コントロールではなく、HTML コントロールを使用することに注意してください。クライアント側のアニメーションを起動、いてはいけないだけです。

次に、追加、`AnimationExtender`のページを提供する、 `ID`、`TargetControlID`属性と、任意`runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

内で、`<Animations>`ノード、`<OnClick>`は、適切な要素をマウスのクリックを処理します。 ただし、アニメーションの中に、ボタンをクリックする可能性があります。 `<EnableAction>`その要素が注意することができます。 設定`Enabled="false"`アニメーションの一部として、ボタンを無効にします。 (ボタンおよび実際のアニメーションを無効にする、) 複数の個別のアニメーションを使用しているため、`<Parallel>`要素が 1 つにまとめて 1 つのアニメーションを接続する必要があります。 ここでは、完全なマークアップを`AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

リストの末尾に次の XML 要素を使用して、アニメーションの終了後 ボタンを再度有効にすることもできます。

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

ただし特定のシナリオでこのが役に立ちませんボタンからフェードアウトになり、アニメーションの最後に表示されていません。


[![アニメーションを実行するとすぐに、ボタンが無効になっています](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

アニメーションを実行するとすぐに、ボタンは無効になります ([フルサイズのイメージを表示するをクリックして](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [前へ](animating-in-response-to-user-interaction-vb.md)
> [次へ](triggering-an-animation-in-another-control-vb.md)
