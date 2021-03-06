---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
title: (VB) を展開する必要があるファイルを決定する |Microsoft ドキュメント
author: rick-anderson
description: 開発環境から実稼働環境に展開する必要があるファイル一部異なるかどうか、ASP.NET アプリケーションがビルドされた us.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: ea918f62-c9d6-4a7f-9bc6-e054d3764b2c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b9fcdbaaa0c2a6d7610339ecb6018a0fe6895f4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889918"
---
<a name="determining-what-files-need-to-be-deployed-vb"></a>(VB) を展開する必要があるファイルを決定します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_vb.pdf)

> 開発環境から実稼働環境に展開する必要があるファイルは、ASP.NET アプリケーションが Web サイトのモデルまたは Web アプリケーションのモデルを使用してビルドするかどうかに部分的に依存します。 これら 2 つのプロジェクトのモデルと、プロジェクト モデルが配置に与える影響の詳細を表示します。


## <a name="introduction"></a>はじめに

ASP.NET web アプリケーションを配置するには、開発環境から運用環境に ASP.NET に関連するファイルをコピーする必要があります。 ASP.NET に関連するファイルには、ASP.NET web ページのマークアップが含まれてし、コードおよびクライアント側とサーバー側ファイルをサポートします。 クライアント側のサポート ファイルは、それらのファイル、web ページによって参照され、たとえばブラウザー - イメージ、CSS ファイル、および JavaScript ファイルに直接送信です。 サーバー側のサポート ファイルには、サーバー側では、要求の処理に使用されるものが含まれます。 その他の SQL ファイルに構成ファイル、web サービス、クラス ファイル、型指定されたデータセット、および LINQ が含まれます。

一般に、すべてのクライアント側のサポート ファイルが、開発環境から実稼働環境にコピーするか、どのようなサーバー側のサポート ファイルがコピーを取得するかどうかに明示的に、サーバー側コードのコンパイルはアセンブリ (、に異なります`.dll`ファイル)、または自動生成されたこれらのアセンブリがある場合。 このチュートリアルでは、明示的にこのコンパイル手順が自動的に実行する場合とアセンブリに、コードをコンパイルするときに展開する必要があるファイルが強調表示されます。

## <a name="explicit-compilation-versus-automatic-compilation"></a>自動のコンパイルとの比較の明示的なコンパイル

ASP.NET web ページは、宣言型マークアップとソース コードに分かれています。 宣言型マークアップ部分には、HTML が含まれています。 Web コントロール、およびデータ バインド構文です。コードの部分には、Visual Basic または c# のコードで記述されたイベント ハンドラーが含まれます。 別のファイルには、マークアップおよびコードの部分は分離される通常:`WebPage.aspx`中に、宣言型マークアップを含む`WebPage.aspx.vb`コードを格納します。

名前付き ASP.NET ページを検討してください`Clock.aspx`テキスト プロパティが設定される現在の日付と時刻、ページが読み込まれるときに、ラベル コントロールを格納しています。 宣言型マークアップ部分 (で`Clock.aspx`) ラベル Web コントロールでのマークアップが含まれます`<asp:Label runat="server" id="TimeLabel" />`- コード部分の中に (で`Clock.aspx.vb`) 必要があります、`Page_Load`イベント ハンドラーを次のコード。

[!code-vb[Main](determining-what-files-need-to-be-deployed-vb/samples/sample1.vb)]

ASP.NET エンジン ページのコードの部分は、このページの要求を処理するために (、 *`WebPage`* `.aspx.vb`ファイル) は、最初にコンパイルされます。 このコンパイルは、明示的または自動に発生することができます。

かどうかは、コンパイル時の動作を明示的に、アプリケーション全体のソース コードは、1 つまたは複数のアセンブリにコンパイル (`.dll`ファイル) に、アプリケーションの配置`Bin`ディレクトリ。 コンパイルが発生する場合に自動的にし、結果として、自動生成されたアセンブリですが、既定では、配置、`Temporary ASP.NET Files`フォルダーにある`%WINDOWS%\Microsoft.NET\Framework\<version>`この場所は、使用して構成できますが、 [ &lt;コンパイル&gt;要素](https://msdn.microsoft.com/library/s10awwz0.aspx)で`Web.config`です。 明示的なコンパイルと、アセンブリに、ASP.NET アプリケーションのコードをコンパイルするいくつかの操作を行う必要があり、この手順は、展開する前に発生します。 コンパイル処理は、自動コンパイルとリソースにまずアクセスするときに、web サーバー上に発生します。

使用するどのようなコンパイル モデルにかかわらず、すべての ASP.NET ページのマークアップの部分 (、`WebPage.aspx`ファイル)、運用環境にコピーする必要があります。 明示的なコンパイルと内のアセンブリをコピーする必要があります、`Bin`フォルダー、ですが、ASP.NET ページのコードの部分をコピーする必要はありません (、`WebPage.aspx.vb`ファイル)。 自動のコンパイルとコードが存在し、ページにアクセスするときに自動的にコンパイルできるように、コードの一部のファイルをコピーする必要があります。 ASP.NET web ページのマークアップの部分が含まれています、`@Page`ページの関連するコードが既に明示的にコンパイルされたか、または自動的にコンパイルする必要があるかどうかを示す属性を持つディレクティブです。 その結果、実稼働環境ことができますの使用かコンパイル モデルにシームレスと明示的または自動コンパイルを使用することを示すために特別な構成設定を適用する必要はありません。

表 1 は、自動コンパイルとの比較の明示的なコンパイルを使用する場合に配置する別のファイルをまとめたものです。 コンパイルに関係なくモデル使用する必要があります内のアセンブリを展開して常に、`Bin`フォルダー、そのフォルダーが存在する場合。 `Bin`フォルダーには、アセンブリ web アプリケーションに固有の明示的なコンパイル モデルを使用する場合は、コンパイル済みのソース コードを含めるにはが含まれています。 `Bin`ディレクトリは、他のプロジェクトからアセンブリおよび使用する場合、オープン ソースまたはサード パーティ製のアセンブリにも含まれており、これらには、実稼働サーバー上にある必要があります。 そのため、一般的な原則として、コピー、`Bin`フォルダーを展開するときに実稼働までです。 (が起きないかどうかは、自動コンパイル モデルを使用して、すべての外部アセンブリを使用していない、`Bin`ディレクトリ - OK である!)

| **コンパイル モデル** | **マークアップの一部のファイルを展開しますか。** | **ソース コード ファイルを展開しますか。** | **アセンブリを展開`Bin`ディレクトリしますか?** |
| --- | --- | --- | --- |
| 明示的なコンパイル | [はい] | いいえ | [はい] |
| 自動のコンパイル | [はい] | [はい] | はい (存在する場合) |

**表 1: どのようなファイルを展開するために使用コンパイル モデルによって異なります。**

## <a name="taking-a-trip-down-memory-lane"></a>メモリのレーンを旅行の取得

どのようなコンパイル アプローチを使用して、一部異なります、Visual Studio での ASP.NET アプリケーションを管理する方法です。 以降。Visual Studio .NET 2002、Visual Studio .NET 2003、Visual Studio 2005、および Visual Studio 2008、Visual Studio での 4 つの異なるバージョンがあった 2000 年に NET の開始。 Visual Studio .NET 2002 および 2003 管理を使用して ASP.NET アプリケーション、 *Web アプリケーション プロジェクト*モデル。 Web アプリケーション プロジェクト モデルの主な機能は次のとおりです。

- 構成のプロジェクトが 1 つのプロジェクト ファイルで定義されているファイルです。 プロジェクト ファイルで定義されていないすべてのファイルでは、Visual Studio によって、web アプリケーションの一部は考慮されません。
- 明示的なコンパイルを使用します。 プロジェクトのビルド ファイルをコンパイル、コード プロジェクト内に配置されている単一のアセンブリに、`Bin`フォルダーです。

Microsoft Visual Studio 2005 をリリースされたときに、Web アプリケーション プロジェクト モデルのサポートを削除し、Web サイト プロジェクトのモデルに置き換えします。 自身が区別されます。 この Web サイト プロジェクトのモデル、 *Web アプリケーション プロジェクト*次のようにモデル。

- 1 つのプロジェクト ファイルで、プロジェクトのファイルを表示するのではなく、ファイル システムが使用されます。 つまり、web アプリケーションのフォルダー (またはサブフォルダー) 内のファイルは、プロジェクトの一部と見なされます。
- Visual Studio でプロジェクトのビルドは、内のアセンブリを作成できません、`Bin`ディレクトリ。 代わりに、Web サイト プロジェクトをビルドすると、コンパイル時エラーを報告します。
- 自動コンパイルをサポートします。 Web サイト プロジェクトをコードにプリコンパイル済みの (明示的なコンパイル) を指定できますが、通常マークアップとソース コードを運用環境にコピーすることによって展開されます。

Microsoft は、Visual Studio 2005 Service Pack 1 を離したときに、Web アプリケーション プロジェクトのモデルを復元します。 ただし、Visual Web Developer は、のみ、Web サイト プロジェクトのモデルをサポートするために継続します。 良いニュースは、この制限は、Visual Web Developer 2008 Service Pack 1 ドロップされました。 現在 Visual Studio (Visual Web Developer) における Web アプリケーション プロジェクトのモデルまたは Web サイト プロジェクトのモデルを使用して ASP.NET アプリケーションを作成できます。 両方のモデルでは、それらの長所と短所があります。 参照してください[Web アプリケーション プロジェクトの概要: Web サイト プロジェクトを比較して Web アプリケーション プロジェクト](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5)とどのようなプロジェクト モデルは、状況に応じた最適を決定する際に 2 つのモデルの比較についてはします。

## <a name="exploring-the-sample-web-application"></a>サンプル Web アプリケーションの探索

このチュートリアルでのダウンロードには、呼び出される書評 ASP.NET アプリケーションが含まれています。 Web サイトは他のユーザーを作成、趣味 web サイトを模倣を book を共有するオンライン コミュニティにレビューします。 この ASP.NET web アプリケーションが非常に単純なあり、次のリソースで構成されています。

- `Web.config`、アプリケーションの構成ファイル。
- マスター ページ (`Site.master`)。
- 7 つの異なる ASP.NET ページ:

    - ~/`Default.aspx` -サイトのホーム ページです。
    - ~/`About.aspx` -「はサイトは、」ページ。
    - ~/`Fiction/Default.aspx` -は確認済みフィクションの書籍を一覧表示するページ。

        - ~/`Fiction/Blaze.aspx` -Richard Bachman novel のレビュー *Blaze*です。
    - ~/`Tech/Default.aspx` -確認されているテクノロジ書籍を一覧表示するページ。

        - ~/`Tech/CYOW.aspx` -のレビュー*作成独自の web サイトの*します。
        - ~/`Tech/TYASP35.aspx` -のレビュー*学べる自分で ASP.NET 3.5 24 時間以内に*です。
- 3 つの異なる CSS ファイルで、`Styles`フォルダーです。
- 4 つのイメージ ファイルの ASP.NET のロゴ、3 つの確認済みのブックのカバーの画像で Powered - すべてにある、`Images`フォルダーです。
- A`Web.sitemap`サイト マップを定義し、メニューを表示するために使用するファイル、`Default.aspx`ルート ディレクトリ内のページと`Fiction`と`Tech`フォルダーです。
- という名前のクラス ファイル`BasePage.vb`、基数を定義する`Page`クラスです。 このクラスの機能を拡張する、`Page`クラスを自動的に設定して、`Title`サイト マップ内のページの位置に基づいてプロパティです。 簡単に言うと、すべての ASP.NET 分離コード クラスを拡張`BasePage`(の代わりに`System.Web.UI.Page`) によっては、サイト マップ内での位置の値に設定のタイトルになります。 たとえば、表示するときに、~/`Tech/CYOW.aspx`  ページで、タイトルを「ホーム:: テクノロジ:: 作成する独自の web サイトの」に設定します。

図 1 は、ブラウザーで表示したときに、書評の web サイトのスクリーン ショットを示します。 ここで、ページが表示 ~ Tech/TYASP35.aspx、書籍をレビューする/*学べる自分で ASP.NET 3.5 24 時間以内に*です。 定義されているサイト マップ構造に基づく、ページおよび左側にあるメニューの上部にまたがっている階層リンク`Web.sitemap`です。 右上隅にイメージは、本の表紙にイメージがある 1 つ、`Images`フォルダーです。 内の CSS ファイルで略さず、カスケード スタイル シートの規則を使用して、web サイトのルック アンド フィールが定義されている、`Styles`包括的なページ レイアウトが、マスター ページで定義されている間は、フォルダー、`Site.master`です。


[![書籍のレビューの web サイトは、多種多様なタイトルのレビューを提供しています](determining-what-files-need-to-be-deployed-vb/_static/image2.png)](determining-what-files-need-to-be-deployed-vb/_static/image1.png)

**図 1**: web サイトを Book レビューは、多種多様なタイトルのレビューを提供しています ([フルサイズのイメージを表示するをクリックして](determining-what-files-need-to-be-deployed-vb/_static/image3.png))


このアプリケーションは、データベースを使用しません。各レビューは、アプリケーションで別の web ページとして実装されます。 このチュートリアル (および次のいくつかのチュートリアル) データベースがない web アプリケーションの配置について説明します。 しかし、今後のチュートリアルでレビュー、リーダーのコメント、および、データベース内にあるその他の情報を格納するには、このアプリケーションを強化し、は、データ ドリブンの web アプリケーションを正しく展開を実行する必要がある手順について説明します。

> [!NOTE]
> これらのチュートリアルでは、web ホスト プロバイダーでの ASP.NET アプリケーションをホストに注目し、ASP のような補助的なトピックの情報を表示しません。NET のマップのサイト システムまたは基本ページ クラスを使用します。 これらのテクノロジの詳細については、チュートリアルで説明されているその他のトピックの背景情報については、それぞれのチュートリアルの最後関連項目」セクションを参照してください。


このチュートリアルのダウンロード、web アプリケーションの 2 つのコピーには、別の Visual Studio プロジェクトの種類としてそれぞれ実装: BookReviewsWAP、Web アプリケーション プロジェクト、および BookReviewsWSP、Web サイト プロジェクト。 両方のプロジェクトでは、Visual Web Developer 2008 SP1 で作成され、ASP.NET 3.5 SP1 を使用します。 使用するには、は、これらのプロジェクトは、デスクトップにコンテンツを解凍して起動します。 Web アプリケーション プロジェクト (BookReviewsWAP) を開くに移動、`BookReviewsWAP`フォルダー ソリューション ファイルをダブルクリックして`BookReviewsWAP.sln`です。 Web サイト プロジェクト (BookReviewsWSP) を開くには、Visual Studio を起動し、次に、ファイル メニューからオプションを選択、Web サイトを開くを参照、`BookReviewsWSP`デスクトップ上のフォルダーと、ok をクリックします。


このチュートリアルの外観にどのようなファイルで残りの 2 つのセクションをアプリケーションを展開するときに、運用環境にコピーする必要があります。 次の 2 つのチュートリアル - [ *、サイトを使用して FTP の展開*](deploying-your-site-using-an-ftp-client-vb.md)と[*を展開する、サイトを使用して Visual Studio* ](deploying-your-site-using-visual-studio-vb.md) -するさまざまな方法を表示します。web ホスト プロバイダーには、これらのファイルをコピーします。

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Web アプリケーション プロジェクトを配置するファイルを決定します。

Web アプリケーション プロジェクト モデルの明示的なコンパイルを使用して、1 つのアセンブリ、アプリケーションをビルドするたびに、プロジェクトのソース コードがコンパイルされます。 このコンパイルには、ASP.NET ページの分離コード ファイルが含まれています (~/`Default.aspx.vb`、~/`About.aspx.vb`など)、だけでなく`BasePage.vb`クラスです。 結果として得られるアセンブリに名前が`BookReviewsWAP.dll`アプリケーションの配置と`Bin`ディレクトリ。

図 2 は、レビュー Web アプリケーション プロジェクトを構成するファイルを示します。


[![ソリューション エクスプ ローラーでは、Web アプリケーション プロジェクトを構成するファイルが一覧表示します。](determining-what-files-need-to-be-deployed-vb/_static/image5.png)](determining-what-files-need-to-be-deployed-vb/_static/image4.png)

**図 2**: ソリューション エクスプ ローラーは、Web アプリケーション プロジェクトを構成するファイルを一覧表示


> [!NOTE]
> 図 2 に示す ASP.NET ページの分離コード ファイルには表示されませんソリューション エクスプ ローラーで、Visual Basic Web アプリケーション プロジェクト。 ページの分離コード クラスを表示するには、 ページのソリューション エクスプ ローラーでを右クリックし、コードの表示を選択します。


明示的に最新のソース コードをアセンブリにコンパイルするためにアプリケーションを構築して Web アプリケーション プロジェクト モデルの開始日を使用して開発された ASP.NET アプリケーションを展開します。 次に、実稼働環境に、次のファイルをコピーします。

- すべての ASP.NET の宣言型マークアップを含んでいるファイル ページなど ~/`Default.aspx`、~/`About.aspx`のようにします。 また、コピーを使用して、任意のマスター ページやユーザー コントロールの宣言型マークアップをします。
- アセンブリ (`.dll`ファイル) で、`Bin`フォルダーです。 プログラム データベース ファイルをコピーする必要はありません (`.pdb`) または任意の XML ファイルで見つけることがあります、`Bin`ディレクトリ。

運用環境に、ASP.NET ページのソース コード ファイルをコピーする必要はありませんもコピーする必要があります、`BasePage.vb`クラス ファイルです。

> [!NOTE]
> 図 2 に示す、`BasePage`という名前のフォルダーに配置されて、プロジェクトのクラス ファイルとしてクラスが実装されている`HelperClasses`です。 プロジェクトをコンパイルするときに、コードで、`BasePage.vb`ファイルは、ASP.NET ページの分離コード クラスと共に 1 つのアセンブリにコンパイル`BookReviewsWAP.dll`です。 ASP.NET には、という名前の特別なフォルダー`App_Code`は Web サイト プロジェクトのクラス ファイルを保持するために設計されています。 内のコード、`App_Code`フォルダーは自動的にコンパイルされ、そのため、Web アプリケーション プロジェクトでは使用できません。 代わりに、通常という名前のフォルダーで、アプリケーションのクラス ファイルを配置する必要があります`HelperClasses`、または`Classes`、したりすることです。 または、別のクラス ライブラリ プロジェクトにクラス ファイルを配置できます。


ASP.NET に関連するマークアップ ファイル内のアセンブリのコピーだけでなく、`Bin`フォルダーもコピーする必要がクライアント側のサポート ファイルのイメージと CSS ファイルで他のサーバー サイドのサポート ファイルだけでなくある`Web.config`と`Web.sitemap`です。 これらクライアント側とサーバー側では、明示的または自動コンパイルを使用するかどうかにかかわらず、実稼働環境にコピーするファイルの必要性をサポートします。

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Web サイト プロジェクトのファイルを展開するファイルの決定

Web サイト プロジェクト モデルは、自動コンパイルでは、Web アプリケーション プロジェクトのモデルを使用するときに使用できない機能をサポートします。 明示的なコンパイルとは、プロジェクトのソース コードをアセンブリにコンパイルしを実稼働環境にそのアセンブリをコピーする必要があります。 その一方で、自動コンパイルを使用するだけで、ソース コードをコピー、運用環境にし、必要に応じて、要求時にランタイムによってコンパイルします。

Visual Studio のビルド メニュー オプションは、Web アプリケーション プロジェクトと Web サイト プロジェクトの両方に存在します。 ある 1 つのアセンブリに、プロジェクトのソース コードをコンパイルする Web アプリケーション プロジェクトをビルド、`Bin`ディレクトリ以外の構築、Web サイト プロジェクトのコンパイル時エラー チェックがすべてのアセンブリでは作成されません。 行う必要があるすべての Web サイト プロジェクト モデルを使用して開発された ASP.NET アプリケーションを展開するコピーを実稼働環境に適切なファイルが、ことをお勧めする最初のビルドにプロジェクトをコンパイル時エラーがないことを確認してください。

図 3 は、レビューの Web サイト プロジェクトを構成するファイルを示します。


[![ソリューション エクスプ ローラーには、Web サイト プロジェクトを構成するファイルが一覧表示します。](determining-what-files-need-to-be-deployed-vb/_static/image7.png)](determining-what-files-need-to-be-deployed-vb/_static/image6.png)

**図 3**: ソリューション エクスプ ローラーは、Web サイト プロジェクトを構成するファイルを一覧表示


Web サイト プロジェクトを配置するには、コード ファイルと共に、ASP.NET ページ、マスター ページ、およびユーザー コントロールのマークアップ ページを含む、運用環境へのすべての ASP.NET に関連するファイルのコピーが含まれます。 など、任意のクラス ファイルをコピーする必要があります`BasePage.vb`です。 なお、`BasePage.vb`ファイルにある、`App_Code`クラス ファイルを Web サイト プロジェクトで使用される特別な ASP.NET フォルダーのフォルダーです。 内のクラス ファイルと同様に、運用環境に作成する必要がある特別なフォルダー、`App_Code`開発環境でのフォルダーにコピーする必要があります、`App_Code`運用上のフォルダーです。

ASP.NET マークアップとソース コード ファイル以外にも、必要もありますだけでなく、その他のサーバー サイドのサポート ファイルのイメージと CSS ファイルでのクライアント側のサポート ファイルをコピーする`Web.config`と`Web.sitemap`です。

> [!NOTE]
> Web サイト プロジェクトには、明示的なコンパイルを使用できます。 今後のチュートリアルでは、明示的に Web サイト プロジェクトをコンパイルする方法を確認します。


## <a name="summary"></a>まとめ

ASP.NET アプリケーションを配置するには、開発環境から運用環境に必要なファイルをコピーする必要があります。 同期する必要があるファイルの正確なセットは、ASP.NET アプリケーションのコードは明示的にまたは自動的にコンパイルされたかによって異なります。 コンパイル戦略を採用されているが、Web アプリケーション プロジェクト モデルまたは Web サイト プロジェクトのモデルを使用して ASP.NET アプリケーションを管理する Visual Studio が構成されているかどうかによって影響を受けます。

Web アプリケーション プロジェクト モデルは、明示的なコンパイルを使用し、は、プロジェクトのコードで 1 つのアセンブリにコンパイル、`Bin`フォルダーです。 アプリケーション、ASP.NET ページのマークアップの部分とのコンテンツを展開するときに、`Bin`フォルダーは、実稼働環境プッシュする必要がありますソース コードのコード ファイルと例については、分離コード クラスのアプリケーションで必要はありません。実稼働環境にコピーされます。

Web サイト プロジェクト モデルがわかるように今後のチュートリアルを明示的に Web サイト プロジェクトをコンパイルすることはできますが、既定では、自動コンパイルを使用します。 自動コンパイルを使用する ASP.NET アプリケーションを展開する必要がありますマークアップ部分*と*ソース コードを運用環境にコピーする必要があります。 コードは、最初に要求されたときに自動的に実稼働環境でコンパイルされます。

これで、開発環境と運用環境間で同期する必要があるファイルを調べてお書評アプリケーション、web ホスト プロバイダーを展開する準備ができました。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET コンパイルの概要](https://msdn.microsoft.com/library/ms178466.aspx)
- [ASP.NET ユーザー コントロール](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [ASP を検査中です。NET のサイトのナビゲーション](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Web アプリケーション プロジェクトの概要](https://msdn.microsoft.com/library/aa730880.aspx)
- [マスター ページのチュートリアル](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [コード ページ間の共有](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [ASP.NET ページの分離コード クラスのカスタムの基本クラスの使用](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Visual Studio 2005 Web サイト プロジェクト システム: の説明となぜこのようなことですか?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [チュートリアル: Web サイト プロジェクトを Visual Studio で Web アプリケーション プロジェクトに変換します。](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [前へ](asp-net-hosting-options-vb.md)
> [次へ](deploying-your-site-using-an-ftp-client-vb.md)
