---
title: Open Web Interface for .NET (OWIN)
author: ardalis
description: "ASP.NET Core のサポートについて Open Web Interface の .NET (OWIN)、これにより web サーバーから切り離すことが可能に web アプリを検出します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e819037e2ebd1566c778879516e20de8dc7603ea
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="42788-103">開くには .NET (OWIN) 用 Web インターフェイスの概要</span><span class="sxs-lookup"><span data-stu-id="42788-103">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="42788-104">によって[Steve Smith](https://ardalis.com/)と[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="42788-104">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="42788-105">ASP.NET Core は、Open Web Interface for .NET (OWIN) をサポートします。</span><span class="sxs-lookup"><span data-stu-id="42788-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="42788-106">OWIN により、Web アプリを Web サーバーから切り離すことが可能になります。</span><span class="sxs-lookup"><span data-stu-id="42788-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="42788-107">要求と関連付けられている応答を処理するパイプラインで使用されるミドルウェアの標準的な方法を定義します。</span><span class="sxs-lookup"><span data-stu-id="42788-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="42788-108">ASP.NET Core アプリケーションとミドルウェアは、OWIN ベースのアプリケーション、サーバー、およびミドルウェアと相互運用できます。</span><span class="sxs-lookup"><span data-stu-id="42788-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="42788-109">OWIN は、2 つのフレームワークに一緒に使用するさまざまなオブジェクト モデルでは、分離のレイヤーを提供します。</span><span class="sxs-lookup"><span data-stu-id="42788-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="42788-110">`Microsoft.AspNetCore.Owin`パッケージは次の 2 つのアダプター実装を提供します。</span><span class="sxs-lookup"><span data-stu-id="42788-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="42788-111">OWIN に ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42788-111">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="42788-112">ASP.NET Core を OWIN</span><span class="sxs-lookup"><span data-stu-id="42788-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="42788-113">これにより、ホストの上、OWIN 互換サーバー/、または ASP.NET Core 上に実行するその他の OWIN 互換コンポーネントをホストする ASP.NET Core できます。</span><span class="sxs-lookup"><span data-stu-id="42788-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="42788-114">注: これらのアダプターを使用して、パフォーマンス コストが付属します。</span><span class="sxs-lookup"><span data-stu-id="42788-114">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="42788-115">ASP.NET Core コンポーネントのみを使用するアプリケーションでは、Owin パッケージまたはアダプターは使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="42788-115">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

<span data-ttu-id="42788-116">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="42788-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="42788-117">ASP.NET パイプラインで実行中の OWIN ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="42788-117">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="42788-118">ASP.NET Core の OWIN のサポートがの一部として展開されている、`Microsoft.AspNetCore.Owin`パッケージです。</span><span class="sxs-lookup"><span data-stu-id="42788-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="42788-119">OWIN のサポートは、このパッケージをインストールすることによって、プロジェクトにインポートできます。</span><span class="sxs-lookup"><span data-stu-id="42788-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="42788-120">OWIN ミドルウェアに準拠している、 [OWIN 仕様](http://owin.org/spec/spec/owin-1.0.0.html)、する必要があります、`Func<IDictionary<string, object>, Task>`インターフェイス、および特定のキーを設定する (など`owin.ResponseBody`)。</span><span class="sxs-lookup"><span data-stu-id="42788-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="42788-121">次の単純な OWIN ミドルウェアには、"Hello World"が表示されます。</span><span class="sxs-lookup"><span data-stu-id="42788-121">The following simple OWIN middleware displays "Hello World":</span></span>

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

<span data-ttu-id="42788-122">サンプルの署名を返します、`Task`を受け入れると、 `IDictionary<string, object>` OWIN による要求どおりです。</span><span class="sxs-lookup"><span data-stu-id="42788-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="42788-123">次のコードを追加する方法を示しています、 `OwinHello` (前に示した) での ASP.NET パイプラインにミドルウェア、`UseOwin`拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="42788-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="42788-124">OWIN パイプライン内で実行するには、その他のアクションを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="42788-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="42788-125">応答ヘッダーは、応答ストリームへの最初の書き込みの前にのみ変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="42788-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="42788-126">複数回呼び出す`UseOwin`をパフォーマンス上の理由からお勧めします。</span><span class="sxs-lookup"><span data-stu-id="42788-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="42788-127">OWIN コンポーネントは、一緒にグループ化する場合、最適動作します。</span><span class="sxs-lookup"><span data-stu-id="42788-127">OWIN components will operate best if grouped together.</span></span>

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="42788-128">OWIN ベースのサーバーで ASP.NET ホストの使用</span><span class="sxs-lookup"><span data-stu-id="42788-128">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="42788-129">OWIN ベースのサーバーには、ASP.NET アプリケーションをホストできます。</span><span class="sxs-lookup"><span data-stu-id="42788-129">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="42788-130">このような 1 つのサーバーが[Nowin](https://github.com/Bobris/Nowin)、.NET OWIN web サーバー。</span><span class="sxs-lookup"><span data-stu-id="42788-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="42788-131">この記事のサンプルでは、含めました Nowin を参照し、それを使用して作成されるプロジェクト、 `IServer` ASP.NET Core 自己ホスト型の対応します。</span><span class="sxs-lookup"><span data-stu-id="42788-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="42788-132">`IServer`インターフェイスを必要とする、`Features`プロパティおよび`Start`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="42788-132">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="42788-133">`Start`構成して、サーバーは、ここでは、一連の IServerAddressesFeature から解析されたアドレスを設定する fluent API 呼び出しを担当します。</span><span class="sxs-lookup"><span data-stu-id="42788-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="42788-134">なお fluent 構成の`_builder`変数は、要求で処理することを指定します、`appFunc`メソッドで定義しました。</span><span class="sxs-lookup"><span data-stu-id="42788-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="42788-135">これは、`Func`は着信要求を処理する要求のたびに呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="42788-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="42788-136">追加しても、`IWebHostBuilder`を追加して Nowin サーバーを構成するが簡単に拡張します。</span><span class="sxs-lookup"><span data-stu-id="42788-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

<span data-ttu-id="42788-137">こうすると、そのために必要なこのカスタムのサーバー拡張機能の呼び出しを使用した ASP.NET アプリケーションを実行する*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="42788-137">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

<span data-ttu-id="42788-138">詳細については、ASP.NET[サーバー](servers/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="42788-138">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="42788-139">OWIN ベースのサーバーで ASP.NET Core を実行し、Websocket のサポートを使用</span><span class="sxs-lookup"><span data-stu-id="42788-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="42788-140">OWIN ベースのサーバーの機能の別の例で利用できる ASP.NET Core は、Websocket などの機能にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="42788-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="42788-141">前の例で使用される .NET OWIN web サーバーには、ASP.NET Core アプリケーションによって利用されるように組み込まれており、Web ソケットのサポートがいます。</span><span class="sxs-lookup"><span data-stu-id="42788-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="42788-142">次の例では、Web ソケットをサポートし、Websocket 経由でサーバーに送信されるすべてのものでエコー バックする単純な web アプリを示します。</span><span class="sxs-lookup"><span data-stu-id="42788-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

<span data-ttu-id="42788-143">これは、[サンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)が構成されている同じ`NowinServer`唯一の違いはで、アプリケーションを構成する方法として、1 つ前の`Configure`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="42788-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="42788-144">使用してテスト[単純な websocket クライアント](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en)アプリケーションを示します。</span><span class="sxs-lookup"><span data-stu-id="42788-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web ソケットのテスト用クライアント](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="42788-146">OWIN 環境</span><span class="sxs-lookup"><span data-stu-id="42788-146">OWIN environment</span></span>

<span data-ttu-id="42788-147">使用する OWIN 環境を構築することができます、`HttpContext`です。</span><span class="sxs-lookup"><span data-stu-id="42788-147">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="42788-148">OWIN キー</span><span class="sxs-lookup"><span data-stu-id="42788-148">OWIN keys</span></span>

<span data-ttu-id="42788-149">OWIN によって異なります、 `IDictionary<string,object>` HTTP 要求/応答の交換全体の情報を通信するオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="42788-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="42788-150">ASP.NET Core では、以下に示すキーを実装します。</span><span class="sxs-lookup"><span data-stu-id="42788-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="42788-151">参照してください、[プライマリ仕様、拡張機能](http://owin.org/#spec)、および[OWIN キー ガイドラインと一般的なキー](http://owin.org/spec/spec/CommonKeys.html)です。</span><span class="sxs-lookup"><span data-stu-id="42788-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="42788-152">要求データ (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="42788-152">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="42788-153">キー</span><span class="sxs-lookup"><span data-stu-id="42788-153">Key</span></span>               | <span data-ttu-id="42788-154">値 (型)</span><span class="sxs-lookup"><span data-stu-id="42788-154">Value (type)</span></span> | <span data-ttu-id="42788-155">説明</span><span class="sxs-lookup"><span data-stu-id="42788-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="42788-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="42788-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="42788-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="42788-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="42788-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="42788-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="42788-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="42788-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="42788-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="42788-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="42788-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="42788-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="42788-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="42788-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="42788-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="42788-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="42788-164">要求データ (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="42788-164">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="42788-165">キー</span><span class="sxs-lookup"><span data-stu-id="42788-165">Key</span></span>               | <span data-ttu-id="42788-166">値 (型)</span><span class="sxs-lookup"><span data-stu-id="42788-166">Value (type)</span></span> | <span data-ttu-id="42788-167">説明</span><span class="sxs-lookup"><span data-stu-id="42788-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="42788-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="42788-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="42788-169">Optional</span><span class="sxs-lookup"><span data-stu-id="42788-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="42788-170">応答データ (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="42788-170">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="42788-171">キー</span><span class="sxs-lookup"><span data-stu-id="42788-171">Key</span></span>               | <span data-ttu-id="42788-172">値 (型)</span><span class="sxs-lookup"><span data-stu-id="42788-172">Value (type)</span></span> | <span data-ttu-id="42788-173">説明</span><span class="sxs-lookup"><span data-stu-id="42788-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="42788-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="42788-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="42788-175">Optional</span><span class="sxs-lookup"><span data-stu-id="42788-175">Optional</span></span> |
| <span data-ttu-id="42788-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="42788-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="42788-177">Optional</span><span class="sxs-lookup"><span data-stu-id="42788-177">Optional</span></span> |
| <span data-ttu-id="42788-178">owin.ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="42788-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="42788-179">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="42788-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="42788-180">その他のデータ (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="42788-180">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="42788-181">キー</span><span class="sxs-lookup"><span data-stu-id="42788-181">Key</span></span>               | <span data-ttu-id="42788-182">値 (型)</span><span class="sxs-lookup"><span data-stu-id="42788-182">Value (type)</span></span> | <span data-ttu-id="42788-183">説明</span><span class="sxs-lookup"><span data-stu-id="42788-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="42788-184">owin.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="42788-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="42788-185">owin.Version</span><span class="sxs-lookup"><span data-stu-id="42788-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="42788-186">共通キー</span><span class="sxs-lookup"><span data-stu-id="42788-186">Common Keys</span></span>

| <span data-ttu-id="42788-187">キー</span><span class="sxs-lookup"><span data-stu-id="42788-187">Key</span></span>               | <span data-ttu-id="42788-188">値 (型)</span><span class="sxs-lookup"><span data-stu-id="42788-188">Value (type)</span></span> | <span data-ttu-id="42788-189">説明</span><span class="sxs-lookup"><span data-stu-id="42788-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="42788-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="42788-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="42788-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="42788-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="42788-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="42788-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="42788-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="42788-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="42788-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="42788-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="42788-195">server.LocalPort</span><span class="sxs-lookup"><span data-stu-id="42788-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="42788-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="42788-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="42788-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="42788-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="42788-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="42788-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="42788-199">キー</span><span class="sxs-lookup"><span data-stu-id="42788-199">Key</span></span>               | <span data-ttu-id="42788-200">値 (型)</span><span class="sxs-lookup"><span data-stu-id="42788-200">Value (type)</span></span> | <span data-ttu-id="42788-201">説明</span><span class="sxs-lookup"><span data-stu-id="42788-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="42788-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="42788-202">sendfile.SendAsync</span></span> | <span data-ttu-id="42788-203">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="42788-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="42788-204">1 回の要求</span><span class="sxs-lookup"><span data-stu-id="42788-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="42788-205">Opaque v0.3.0</span><span class="sxs-lookup"><span data-stu-id="42788-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="42788-206">キー</span><span class="sxs-lookup"><span data-stu-id="42788-206">Key</span></span>               | <span data-ttu-id="42788-207">値 (型)</span><span class="sxs-lookup"><span data-stu-id="42788-207">Value (type)</span></span> | <span data-ttu-id="42788-208">説明</span><span class="sxs-lookup"><span data-stu-id="42788-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="42788-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="42788-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="42788-210">opaque.Upgrade</span><span class="sxs-lookup"><span data-stu-id="42788-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="42788-211">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="42788-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="42788-212">opaque.Stream</span><span class="sxs-lookup"><span data-stu-id="42788-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="42788-213">opaque.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="42788-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="42788-214">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="42788-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="42788-215">キー</span><span class="sxs-lookup"><span data-stu-id="42788-215">Key</span></span>               | <span data-ttu-id="42788-216">値 (型)</span><span class="sxs-lookup"><span data-stu-id="42788-216">Value (type)</span></span> | <span data-ttu-id="42788-217">説明</span><span class="sxs-lookup"><span data-stu-id="42788-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="42788-218">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="42788-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="42788-219">websocket.Accept</span><span class="sxs-lookup"><span data-stu-id="42788-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="42788-220">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="42788-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="42788-221">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="42788-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="42788-222">非仕様</span><span class="sxs-lookup"><span data-stu-id="42788-222">Non-spec</span></span> |
| <span data-ttu-id="42788-223">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="42788-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="42788-224">参照してください[RFC6455 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2)手順 5.5</span><span class="sxs-lookup"><span data-stu-id="42788-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="42788-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="42788-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="42788-226">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="42788-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="42788-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="42788-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="42788-228">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="42788-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="42788-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="42788-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="42788-230">参照してください[デリゲートのシグネチャ](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="42788-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="42788-231">websocket.CallCancelled</span><span class="sxs-lookup"><span data-stu-id="42788-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="42788-232">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="42788-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="42788-233">Optional</span><span class="sxs-lookup"><span data-stu-id="42788-233">Optional</span></span> |
| <span data-ttu-id="42788-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="42788-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="42788-235">Optional</span><span class="sxs-lookup"><span data-stu-id="42788-235">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="42788-236">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="42788-236">Additional Resources</span></span>

* [<span data-ttu-id="42788-237">ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="42788-237">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="42788-238">サーバー</span><span class="sxs-lookup"><span data-stu-id="42788-238">Servers</span></span>](servers/index.md)