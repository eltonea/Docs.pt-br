---
title: "Convenções de autorização de páginas Razor do ASP.NET Core"
author: guardrex
description: "Aprenda a controlar o acesso a páginas com as convenções na inicialização que autorizam usuários e permitir que usuários anônimos acessem páginas individuais ou pastas de páginas."
ms.author: riande
manager: wpickett
ms.date: 10/27/2017
ms.topic: article
ms.assetid: f65ad22d-9472-478a-856c-c59c8681fa71
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 36acf3c06a462882972c5f389d544d98cadc35f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="2cbef-103">Convenções de autorização de páginas Razor do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2cbef-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="2cbef-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2cbef-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2cbef-105">Uma maneira de controlar o acesso em seu aplicativo de páginas Razor é usar as convenções de autorização na inicialização.</span><span class="sxs-lookup"><span data-stu-id="2cbef-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="2cbef-106">As seguintes convenções permitem que você autorizar os usuários e permitir que usuários anônimos acessem páginas individuais ou pastas de páginas.</span><span class="sxs-lookup"><span data-stu-id="2cbef-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="2cbef-107">Aplicam as convenções descritas neste tópico automaticamente [filtros de autorização](xref:mvc/controllers/filters#authorization-filters) para controlar o acesso.</span><span class="sxs-lookup"><span data-stu-id="2cbef-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="2cbef-108">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2cbef-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="2cbef-109">Exigir autorização para acessar uma página</span><span class="sxs-lookup"><span data-stu-id="2cbef-109">Require authorization to access a page</span></span>

<span data-ttu-id="2cbef-110">Use o [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para a página no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="2cbef-110">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="2cbef-111">O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo do Razor páginas raiz sem uma extensão e contendo apenas barras invertidas.</span><span class="sxs-lookup"><span data-stu-id="2cbef-111">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="2cbef-112">Um [AuthorizePage sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.</span><span class="sxs-lookup"><span data-stu-id="2cbef-112">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="2cbef-113">Exigir autorização para acessar uma pasta de páginas</span><span class="sxs-lookup"><span data-stu-id="2cbef-113">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="2cbef-114">Use o [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para todas as páginas em uma pasta no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="2cbef-114">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="2cbef-115">O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo de raiz de páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="2cbef-115">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="2cbef-116">Um [AuthorizeFolder sobrecarga](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) estará disponível se você precisa especificar uma política de autorização.</span><span class="sxs-lookup"><span data-stu-id="2cbef-116">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="2cbef-117">Permitir acesso anônimo a uma página</span><span class="sxs-lookup"><span data-stu-id="2cbef-117">Allow anonymous access to a page</span></span>

<span data-ttu-id="2cbef-118">Use o [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para uma página no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="2cbef-118">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="2cbef-119">O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo do Razor páginas raiz sem uma extensão e contendo apenas barras invertidas.</span><span class="sxs-lookup"><span data-stu-id="2cbef-119">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="2cbef-120">Permitir o acesso anônimo para uma pasta de páginas</span><span class="sxs-lookup"><span data-stu-id="2cbef-120">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="2cbef-121">Use o [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convenção via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) para adicionar um [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para todas as páginas em uma pasta no caminho especificado:</span><span class="sxs-lookup"><span data-stu-id="2cbef-121">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="2cbef-122">O caminho especificado é o caminho do mecanismo de exibição, que é o caminho relativo de raiz de páginas Razor.</span><span class="sxs-lookup"><span data-stu-id="2cbef-122">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="2cbef-123">Observe na combinação de autorização e acesso anônimo</span><span class="sxs-lookup"><span data-stu-id="2cbef-123">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="2cbef-124">É perfeitamente válido para especificar que uma pasta de páginas exigem autorização e especifique que uma página dentro da pasta permite acesso anônimo:</span><span class="sxs-lookup"><span data-stu-id="2cbef-124">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="2cbef-125">O inverso, no entanto, não é true.</span><span class="sxs-lookup"><span data-stu-id="2cbef-125">The reverse, however, isn't true.</span></span> <span data-ttu-id="2cbef-126">Você não pode declarar uma pasta de páginas para acesso anônimo e especificar uma página dentro de autorização:</span><span class="sxs-lookup"><span data-stu-id="2cbef-126">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="2cbef-127">Exigir autorização na página privada não funcionará porque quando tanto o `AllowAnonymousFilter` e `AuthorizeFilter` filtros são aplicados para a página, o `AllowAnonymousFilter` wins e controla o acesso.</span><span class="sxs-lookup"><span data-stu-id="2cbef-127">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="see-also"></a><span data-ttu-id="2cbef-128">Consulte também</span><span class="sxs-lookup"><span data-stu-id="2cbef-128">See also</span></span>

* [<span data-ttu-id="2cbef-129">Razor páginas rota e página modelo provedores personalizados</span><span class="sxs-lookup"><span data-stu-id="2cbef-129">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* <span data-ttu-id="2cbef-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) classe</span><span class="sxs-lookup"><span data-stu-id="2cbef-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>