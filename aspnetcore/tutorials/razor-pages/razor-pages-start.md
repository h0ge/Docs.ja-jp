---
title: ASP.NET Core の Razor ページの概要
author: rick-anderson
description: ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。 Razor ページは、ASP.NET Core の Web ワークロードで推奨されています。
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: e317b49f2ad33c392de33bc32a87f67bb8cb72a0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278045"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>ASP.NET Core の Razor ページの概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-2.0"

このチュートリアルの ASP.NET Core 2.1 バージョンのご利用をお勧めします。 より簡単に使用でき、**より**多くの機能があります。 バージョン セレクターで、**[ASP.NET Core 2.1]** を選択します。

![TOC のバージョン セレクター](razor-pages-start/_static/v21.png)

::: moniker-end

このチュートリアルでは、ASP.NET Core の Razor ページ Web アプリの構築の基礎について説明します。 ASP.NET Core で Web アプリ用の UI を構築する際に Razor ページを使用することをお勧めします。

このチュートリアルには 3 つのバージョンがあります。

* Windows 向け: 本チュートリアル
* MacOS 向け: [Visual Studio for Mac 向けの Razor ページの概要 ](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS、Linux、Windows 向け: [Visual Studio Code を使用する ASP.NET Core の Razor ページの概要](xref:tutorials/razor-pages-vsc/razor-pages-start)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE [prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Razor Web アプリの作成

* Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。
* 新しい ASP.NET Core Web アプリケーションを作成します。 プロジェクトに **RazorPagesMovie** という名前を付けます。 コードのコピー/貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。
 ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2.1.png)
* ドロップダウン リストで **[ASP.NET Core 2.1]** を選択してから、**[Web アプリケーション]** を選択します。

 ![新しい ASP.NET Core Web アプリケーション](razor-pages-start/_static/np_2_2.1.png)

以下のように、Visual Studio のテンプレートでスタート プロジェクトを作成します。

![ソリューション エクスプローラー](razor-pages-start/_static/se2.1.png)

**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。 **[同意する]** を選択し、追跡に同意します。 このアプリでは個人情報は追跡されません。 テンプレートで生成されたコードには、[一般的なデータ保護規制 (GDPR)](xref:security/gdpr) を満たす資産が含まれます。

![ホームまたはインデックス ページ](razor-pages-start/_static/homeGDPR.png)

次の図は、追跡に同意した後のアプリを示しています。

![ホームまたはインデックス ページ](razor-pages-start/_static/home2.1.png)

* Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。 アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 localhost では、ローカル コンピューターからの Web 要求のみが処理されます。 Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。 上の図では、ポート番号は 5000 です。 アプリを実行する際には、別のポート番号が表示されます。
* **Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。 多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a>必須コンポーネント

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Razor Web アプリの作成

* Visual Studio の **[ファイル]** メニューから、**[新規作成]**、**[プロジェクト]** の順に選択します。
* 新しい ASP.NET Core Web アプリケーションを作成します。 プロジェクトに **RazorPagesMovie** という名前を付けます。 コードのコピー/貼り付けを行う際に名前空間が一致するように、プロジェクトに *RazorPagesMovie* という名前を付けることが重要です。
  ![新しい ASP.NET Core Web アプリケーション](../../razor-pages/index/_static/np.png)
* ドロップダウン リストで **[ASP.NET Core 2.0]** を選択してから、**[Web アプリケーション]** を選択します。

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

以下のように、Visual Studio のテンプレートでスタート プロジェクトを作成します。

![ソリューション エクスプローラー](razor-pages-start/_static/se.png)

**F5** キーを押して、デバッグ モードでアプリを実行するか、**Ctrl + F5** キーを押して、デバッガーをアタッチせずに実行します。

![ホームまたはインデックス ページ](razor-pages-start/_static/home.png)

* Visual Studio で [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。 アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。 これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。 localhost では、ローカル コンピューターからの Web 要求のみが処理されます。 Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。 上の図では、ポート番号は 5000 です。 アプリを実行する際には、別のポート番号が表示されます。
* **Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。 多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [次: モデルの追加](xref:tutorials/razor-pages/model)
