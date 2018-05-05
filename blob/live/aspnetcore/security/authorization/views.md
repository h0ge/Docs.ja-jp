---
title: "ASP.NET Core MVC でのビューに基づく承認"
author: rick-anderson
description: "このドキュメントでは、挿入および ASP.NET Core Razor ビュー内で承認サービスを利用する方法を示します。"
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 8f87ca90b77be1efd75688e8203cb57b1a3360ad
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="view-based-authorization"></a><span data-ttu-id="ebb80-103">ビューに基づく承認</span><span class="sxs-lookup"><span data-stu-id="ebb80-103">View-based authorization</span></span>

<span data-ttu-id="ebb80-104">多くの場合、開発者を表示、非表示、またはそれ以外の場合、現在のユーザー id に基づいて UI を変更したいです。</span><span class="sxs-lookup"><span data-stu-id="ebb80-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="ebb80-105">使用して MVC ビューの中で承認サービスにアクセスすることができます[依存性の注入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="ebb80-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="ebb80-106">Razor ビューには、承認サービスを挿入を使用して、`@inject`ディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="ebb80-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="ebb80-107">すべてのビューで、承認サービスを実行する場合に、配置、`@inject`にディレクティブ、 *_ViewImports.cshtml*のファイル、*ビュー*ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="ebb80-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="ebb80-108">詳細については、次を参照してください。[ビューに依存性の注入](xref:mvc/views/dependency-injection)です。</span><span class="sxs-lookup"><span data-stu-id="ebb80-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="ebb80-109">挿入された承認サービスを使用して呼び出す`AuthorizeAsync`ことを確認中にまったく同じ方法で[リソース ベースの承認](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="ebb80-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ebb80-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ebb80-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ebb80-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ebb80-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="ebb80-112">場合によっては、リソースは、モデルの表示になります。</span><span class="sxs-lookup"><span data-stu-id="ebb80-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="ebb80-113">呼び出す`AuthorizeAsync`ことを確認中にまったく同じ方法で[リソース ベースの承認](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="ebb80-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ebb80-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ebb80-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ebb80-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ebb80-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="ebb80-116">上記のコードで、モデルは、考慮に入れてポリシーの評価が実行をリソースとして渡されます。</span><span class="sxs-lookup"><span data-stu-id="ebb80-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="ebb80-117">アプリの UI 要素の唯一の承認チェックとの切り替えの可視性に依存しないようにします。</span><span class="sxs-lookup"><span data-stu-id="ebb80-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="ebb80-118">UI 要素を非表示にする可能性がありますいないを完全に防ぐアクセスその関連付けられたコント ローラー アクションにします。</span><span class="sxs-lookup"><span data-stu-id="ebb80-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="ebb80-119">たとえば、上記のコード スニペットのボタンがあるとします。</span><span class="sxs-lookup"><span data-stu-id="ebb80-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="ebb80-120">ユーザーが呼び出すことができます、`Edit`相対的なリソースを知っている場合、アクション メソッドの URL は*/Document/Edit/1*です。</span><span class="sxs-lookup"><span data-stu-id="ebb80-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="ebb80-121">このため、`Edit`アクション メソッドが独自の承認チェックを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ebb80-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>