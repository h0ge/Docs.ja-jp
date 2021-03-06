---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: 場合、何を実行する配置 |Microsoft ドキュメント
author: jrjlee
description: このトピック 'what-if' を実行する方法について説明します (またはシミュレート) V、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用して展開しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: c1a13f38c8e629bcd615190b00104109e25fb289
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879986"
---
<a name="performing-a-what-if-deployment"></a>"What If"配置の実行
====================
によって[Jason lee 著](https://github.com/jrjlee)

[PDF をダウンロードします。](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> このトピックの内容"what-if"を実行する方法について説明します (またはシミュレート) VSDBCMD、インターネット インフォメーション サービス (IIS) Web 配置ツール (Web 配置) を使用して展開します。 これにより、アプリケーションを実際に展開する前に、特定のターゲット環境で、デプロイ ロジックの結果が決定できます。


このトピックの Fabrikam, Inc. という架空の会社のエンタープライズ展開の要件に関するチュートリアル シリーズの一部を形成します。このチュートリアルの一連のサンプル ソリューションを使用する&#x2014;、 [Contact Manager ソリューション](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;現実的な ASP.NET MVC 3 アプリケーション、Windows Communication も含め、複雑さのレベルを持つ web アプリケーションを表すFoundation (WCF) サービスとデータベース プロジェクト。

説明されている分割プロジェクト ファイル アプローチに基づいて、これらのチュートリアルの中心に配置メソッド[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)、によってビルドおよび配置プロセスが制御されるは、2 つのプロジェクト ファイル&#x2014;いずれか各配置先の環境と環境固有のビルドと配置の設定を含む 1 つに適用されるビルドの手順を含むです。 ビルド時に環境固有のプロジェクト ファイルは、ビルドの手順の完全なセットを形成する環境に依存しないプロジェクト ファイルにマージされます。

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Web パッケージを"What If"の展開を実行します。

Web Deploy には、"what-if"で展開を実行するための機能 (または試用版) が含まれています。 モード。 "What-if"モードでの成果物を展開するときにの展開を実行したが、実際には変化しません、移行先サーバーにログ ファイルを生成する Web 配置します。 ログ ファイルを確認すると、デプロイは移行先サーバーで具体的にはどのような影響を理解することができます。

- 何が追加取得されます。
- 新機能が更新されます。
- 取得とは、削除されます。

"What-if"展開実際には変化しません移行先サーバーで、新機能が常にできないため、展開が成功するかどうかを予測します。

」の説明に従って[Web パッケージを展開する](../web-deployment-in-the-enterprise/deploying-web-packages.md)、2 つの方法で Web Deploy を使用して web パッケージを展開する&#x2014;直接、またはを実行して MSDeploy.exe コマンド ライン ユーティリティを使用して、 *. deploy.cmd*ファイルビルド プロセスのことを生成します。

追加することで、"what-if"展開を実行するには MSDeploy.exe を直接使用している場合、 **-whatif**には、コマンド フラグ。 たとえば、ContactManager.Mvc.zip パッケージをステージング環境に展開した場合になるかを評価する MSDeploy コマンドのようになりますこの。


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


削除することができます、"what-if"展開の結果に満足したら、 **-whatif**ライブの展開を実行するフラグ。

> [!NOTE]
> MSDeploy.exe のコマンド ライン オプションの詳細については、次を参照してください。 [Web Deploy 操作 Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx)です。


使用する場合、 *. deploy.cmd*ファイルを含めることによって、"what-if"展開を実行することができます、 **/t**フラグ (試用モード) のフラグの代わりに、 **/y**フラグ ("yes"、または更新モード)コマンド。 たとえばを実行して ContactManager.Mvc.zip パッケージを展開する場合になるかを評価するため、 *. deploy.cmd*ファイル、このコマンドのようになります。


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


「試用モード」配置の結果に満足したら、置き換えることができます、 **/t**でフラグを設定、 **/y**ライブの展開を実行するフラグ。


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> コマンド ライン オプションについて *. deploy.cmd*ファイルを参照してください[する方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)です。 実行する場合、 *. deploy.cmd*ファイル、コマンド プロンプトを任意のフラグを指定することがなく使用可能なフラグの一覧が表示されます。


## <a name="performing-a-what-if-deployment-for-databases"></a>データベースの"What If"展開を実行します。

このセクションでは、VSDBCMD ユーティリティを使用して、増分、スキーマ ベースのデータベースの配置を実行していることを前提としています。 このアプローチがで詳しく説明されている[データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)です。 理解このトピックでは、ここで説明する概念を適用する前にすることをお勧めします。

VSDBCMD を使用する場合**展開**モードでは、行うこともできます、 **/dd** (または **/DeployToDatabase**) VSDBCMD が実際には、データベースを展開またはだけを生成するかどうかを制御するフラグを設定します。配置スクリプト。 .Dbschema ファイルを配置する場合、動作を示します。

- 指定した場合 **/dd+** または **/dd**VSDBCMD は配置スクリプトを生成し、データベースを展開します。
- 指定した場合 **/dd-** またはスイッチは省略、VSDBCMD のみ配置スクリプトが生成されます。

> [!NOTE]
> .Dbschema ファイルの動作ではなく、.deploymanifest ファイルを配置する場合、 **/dd**スイッチがはるかに複雑です。 VSDBCMD 基本的には、値は無視されます、 **/dd**切り替える .deploymanifest ファイルが含まれる場合、 **DeployToDatabase**の値を持つ要素**True**です。 [データベース プロジェクトの配置](../web-deployment-in-the-enterprise/deploying-database-projects.md)完全には、この動作について説明します。


たとえば、配置スクリプトを生成するため、 **ContactManager**この実際には、データベース、VSDBCMD コマンドを配置することがなくデータベースのようになります。


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD は、データベースの差分の展開、およびツールを 1 つ存在する場合、指定されたスキーマに、現在のデータベースを更新するために必要なすべての SQL コマンドを格納する、配置スクリプトが動的に生成されるようです。 配置スクリプトを確認することは、デプロイに影響を与えるものを決定する便利な方法は、現在のデータベースとが含まれているデータです。 たとえばを確認します。

- 既存のテーブルを削除するかどうかと、データが失われるとなるかどうか。
- 操作の順序はたとえば、データ損失のリスクを実行するかどうか、分割またはテーブルを結合する場合は。

配置スクリプトに満足したら場合に、VSDBCMD を繰り返すことができます、 **/dd+** フラグの変更を行います。 また、お客様の要件を満たしているし、データベース サーバーに手動で実行するには、配置スクリプトを編集できます。

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>カスタム プロジェクト ファイルへの"What If"機能の統合

複雑な展開シナリオでおくを」の説明に従って、カスタムの Microsoft Build Engine (MSBuild) プロジェクト ファイルを使用して、ビルドおよび配置ロジックをカプセル化する[プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。 たとえば、[連絡先のマネージャー](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)サンプル ソリューションを*Publish.proj*ファイル。

- ソリューションをビルドします。
- パッケージ化し、ContactManager.Mvc アプリケーションを配置するには、Web Deploy を使用します。
- パッケージ化し、ContactManager.Service アプリケーションを配置するには、Web Deploy を使用します。
- 展開、 **ContactManager**データベース。

この方法で 1 つの手順を複数の web のパッケージやデータベースの配置を統合するときにすることも、オプションの"what-if"モードで配置全体を実行します。

*Publish.proj*ファイルでは、この操作方法を示します。 最初に、"what-if"の値を格納するプロパティを作成する必要があります。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


この場合、という名前のプロパティを作成した**WhatIf**で、既定値は**false**です。 ユーザーは、プロパティを設定してこの値を上書きできます**true** 、コマンド ライン パラメーターでも後ほどです。

次のステージでは、Web Deploy をパラメーター化し、VSDBCMD フラグを反映するためのコマンド、 **WhatIf**プロパティの値。 たとえば、[次へ] のターゲット (から取得した、 *Publish.proj*ファイルおよび簡素化されます) を実行、 *. deploy.cmd* web パッケージを配置するファイル。 既定では、コマンドは、含まれています、 **/Y** ("yes"または更新モード) スイッチ。 場合**WhatIf**に設定されている**true**で置き換えられます、 **/T** (評価版、または"what-if"モード) スイッチ。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


同様に、次のターゲットは、データベースを配置するのに VSDBCMD ユーティリティを使用します。 既定では、 **/dd**スイッチは含まれません。 つまり、VSDBCMD では、配置スクリプトが生成されますが、データベースに展開できなくなります&#x2014;言い換えると、"what-if"シナリオです。 場合、 **WhatIf**プロパティに設定されていない**true**、 **/dd**スイッチが追加され、VSDBCMD は、データベースを展開します。


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


同じアプローチを使用するには、プロジェクト ファイルですべての関連するコマンドをパラメーター化します。 "What-if"展開を実行する場合は、単に提供できます、 **WhatIf**コマンドラインからプロパティ値。


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


この方法で、プロジェクトのすべてのコンポーネントをシングル ステップで"what-if"展開を実行できます。

## <a name="conclusion"></a>まとめ

このトピックでは、Web デプロイ、VSDBCMD、および MSBuild を使用する展開"what-if"を実行する方法について説明します。 "What-if"展開では、変更する前に実際には、送信先の環境には、提案された展開の影響を評価することができます。

## <a name="further-reading"></a>関連項目

Web Deploy のコマンドライン構文の詳細については、次を参照してください。 [Web Deploy 操作 Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx)です。 コマンド ライン オプションを使用するときのガイダンスについては、 *. deploy.cmd*ファイルを参照してください[する方法: 展開パッケージを使用して、deploy.cmd ファイルをインストール](https://msdn.microsoft.com/library/ff356104.aspx)です。 VSDBCMD コマンドライン構文については、次を参照してください。 [VSDBCMD のコマンド ライン リファレンスです。EXE (配置とスキーマのインポート)](https://msdn.microsoft.com/library/dd193283.aspx)です。

> [!div class="step-by-step"]
> [前へ](advanced-enterprise-web-deployment.md)
> [次へ](customizing-database-deployments-for-multiple-environments.md)
