---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: "Adicionando conteúdo dinâmico para uma página em cache (VB) | Microsoft Docs"
author: microsoft
description: "Saiba como combinar o conteúdo dinâmico e armazenados em cache na mesma página. Substituição post-cache permite que você exiba o conteúdo dinâmico, como o de anúncios de banner..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f07f4ecec36e71679dbc471b65f26d260349a07e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="a66ea-104">Adicionando conteúdo dinâmico para uma página em cache (VB)</span><span class="sxs-lookup"><span data-stu-id="a66ea-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="a66ea-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a66ea-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a66ea-106">Saiba como combinar o conteúdo dinâmico e armazenados em cache na mesma página.</span><span class="sxs-lookup"><span data-stu-id="a66ea-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="a66ea-107">Substituição post-cache permite que você exiba o conteúdo dinâmico, como anúncios de banner ou itens de notícias, dentro de uma página que tenha sido saída armazenado em cache.</span><span class="sxs-lookup"><span data-stu-id="a66ea-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="a66ea-108">Aproveitando o cache de saída, você pode melhorar drasticamente o desempenho de um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a66ea-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="a66ea-109">Em vez de gerar novamente uma página de cada vez que a página é solicitada, a página pode ser gerada uma vez e armazenado em cache na memória para vários usuários.</span><span class="sxs-lookup"><span data-stu-id="a66ea-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="a66ea-110">Mas há um problema.</span><span class="sxs-lookup"><span data-stu-id="a66ea-110">But there is a problem.</span></span> <span data-ttu-id="a66ea-111">Se você precisa exibir o conteúdo dinâmico na página?</span><span class="sxs-lookup"><span data-stu-id="a66ea-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="a66ea-112">Por exemplo, imagine que você deseja exibir um anúncio de faixa na página.</span><span class="sxs-lookup"><span data-stu-id="a66ea-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="a66ea-113">Você não deseja que o anúncio de cabeçalho a ser armazenado em cache para que cada usuário vê o anúncio do mesmo.</span><span class="sxs-lookup"><span data-stu-id="a66ea-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="a66ea-114">Você não faz qualquer dinheiro dessa forma!</span><span class="sxs-lookup"><span data-stu-id="a66ea-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="a66ea-115">Felizmente, há uma solução fácil.</span><span class="sxs-lookup"><span data-stu-id="a66ea-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="a66ea-116">Você pode tirar proveito de um recurso da estrutura do ASP.NET chamado *pós-cache*.</span><span class="sxs-lookup"><span data-stu-id="a66ea-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="a66ea-117">Substituição post-cache permite que você substitua o conteúdo dinâmico em uma página que foi armazenado em cache na memória.</span><span class="sxs-lookup"><span data-stu-id="a66ea-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="a66ea-118">Normalmente, quando você saída cache uma página usando o &lt;OutputCache&gt; atributo, a página é armazenada em cache no servidor e o cliente (navegador da web).</span><span class="sxs-lookup"><span data-stu-id="a66ea-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="a66ea-119">Quando você usar substituição post-cache, uma página é armazenada em cache somente no servidor.</span><span class="sxs-lookup"><span data-stu-id="a66ea-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="a66ea-120">Usar substituição POST-Cache</span><span class="sxs-lookup"><span data-stu-id="a66ea-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="a66ea-121">Usar substituição post-cache requer duas etapas.</span><span class="sxs-lookup"><span data-stu-id="a66ea-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="a66ea-122">Primeiro, você precisa definir um método que retorna uma cadeia de caracteres que representa o conteúdo dinâmico que você deseja exibir na página em cache.</span><span class="sxs-lookup"><span data-stu-id="a66ea-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="a66ea-123">Em seguida, você chamar o método HttpResponse.WriteSubstitution() para inserir o conteúdo dinâmico na página.</span><span class="sxs-lookup"><span data-stu-id="a66ea-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="a66ea-124">Por exemplo, imagine que você deseja exibir itens de notícias diferentes aleatoriamente em uma página em cache.</span><span class="sxs-lookup"><span data-stu-id="a66ea-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="a66ea-125">A classe na listagem 1 expõe um método único, chamado RenderNews(), que retorna aleatoriamente um item de notícias em uma lista de três itens de notícias.</span><span class="sxs-lookup"><span data-stu-id="a66ea-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="a66ea-126">**Listando 1 – Models\News.vb**</span><span class="sxs-lookup"><span data-stu-id="a66ea-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="a66ea-127">Para tirar proveito de substituição post-cache, você chamar o método HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="a66ea-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="a66ea-128">O método WriteSubstitution() define o código para substituir uma região de página em cache com conteúdo dinâmico.</span><span class="sxs-lookup"><span data-stu-id="a66ea-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="a66ea-129">O método WriteSubstitution() é usado para exibir o item de notícias aleatório no modo de exibição na listagem 2.</span><span class="sxs-lookup"><span data-stu-id="a66ea-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="a66ea-130">**A listagem 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a66ea-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="a66ea-131">O método RenderNews é passado para o método WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="a66ea-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="a66ea-132">Observe que o método RenderNews não é chamado.</span><span class="sxs-lookup"><span data-stu-id="a66ea-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="a66ea-133">Em vez disso, uma referência para o método é passada para WriteSubstitution() com a Ajuda do operador AddressOf.</span><span class="sxs-lookup"><span data-stu-id="a66ea-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="a66ea-134">O modo de exibição do índice é armazenado em cache.</span><span class="sxs-lookup"><span data-stu-id="a66ea-134">The Index view is cached.</span></span> <span data-ttu-id="a66ea-135">O modo de exibição é retornado pelo controlador na listagem 3.</span><span class="sxs-lookup"><span data-stu-id="a66ea-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="a66ea-136">Observe que a ação Index () é decorada com um &lt;OutputCache&gt; atributo que faz com que a exibição do índice a ser armazenado em cache por 60 segundos.</span><span class="sxs-lookup"><span data-stu-id="a66ea-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="a66ea-137">**A listagem 3 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="a66ea-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="a66ea-138">Embora o modo de exibição do índice é armazenado em cache, itens de notícias aleatórios diferentes são exibidos quando você solicitar a página de índice.</span><span class="sxs-lookup"><span data-stu-id="a66ea-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="a66ea-139">Quando você solicitar a página de índice, não altera a hora exibida pela página por 60 segundos (consulte a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="a66ea-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="a66ea-140">O fato de que não altera a hora comprova que a página é armazenada em cache.</span><span class="sxs-lookup"><span data-stu-id="a66ea-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="a66ea-141">No entanto, o conteúdo é injetado pelas alterações de método – o item de notícias aleatório – WriteSubstitution() com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="a66ea-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="a66ea-142">**Figura 1 – injeção de itens de notícias dinâmico em uma página em cache**</span><span class="sxs-lookup"><span data-stu-id="a66ea-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="a66ea-144">Usar substituição POST-Cache em métodos auxiliares</span><span class="sxs-lookup"><span data-stu-id="a66ea-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="a66ea-145">É uma maneira mais fácil para tirar proveito de substituição post-cache encapsular a chamada ao método WriteSubstitution() dentro de um método auxiliar personalizado.</span><span class="sxs-lookup"><span data-stu-id="a66ea-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="a66ea-146">Essa abordagem é ilustrada pelo método auxiliar na listagem 4.</span><span class="sxs-lookup"><span data-stu-id="a66ea-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="a66ea-147">**A listagem 4 – Helpers\AdHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="a66ea-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="a66ea-148">A listagem 4 contém um módulo do Visual Basic que expõe dois métodos: RenderBanner() e RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="a66ea-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="a66ea-149">O método RenderBanner() representa o método auxiliar real.</span><span class="sxs-lookup"><span data-stu-id="a66ea-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="a66ea-150">Este método estende a classe HtmlHelper do ASP.NET MVC padrão, para que você possa chamar Html.RenderBanner() em uma exibição, assim como qualquer outro método auxiliar.</span><span class="sxs-lookup"><span data-stu-id="a66ea-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="a66ea-151">O método RenderBanner() chama o método de HttpResponse.WriteSubstitution() passando o método RenderBannerInternal() para o método WriteSubsitution().</span><span class="sxs-lookup"><span data-stu-id="a66ea-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="a66ea-152">O método RenderBannerInternal() é um método privado.</span><span class="sxs-lookup"><span data-stu-id="a66ea-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="a66ea-153">Esse método não ser exposto como um método auxiliar.</span><span class="sxs-lookup"><span data-stu-id="a66ea-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="a66ea-154">O método RenderBannerInternal() retorna aleatoriamente uma imagem de anúncio de faixa de uma lista de três imagens de anúncio de faixa.</span><span class="sxs-lookup"><span data-stu-id="a66ea-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="a66ea-155">O modo de exibição do índice modificado na listagem 5 ilustra como você pode usar o método auxiliar RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="a66ea-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="a66ea-156">Observe que adicional &lt;% @ importação %&gt; diretiva está incluída na parte superior do modo de exibição para importar o namespace MvcApplication1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="a66ea-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="a66ea-157">Se você não importar esse namespace, o método RenderBanner() não aparecerá como um método na propriedade de Html.</span><span class="sxs-lookup"><span data-stu-id="a66ea-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="a66ea-158">**Listando 5 – Views\Home\Index.aspx (com o método RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="a66ea-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="a66ea-159">Quando você solicitar a página renderizada pela exibição na listagem 5, uma faixa diferentes de anúncio é exibido com cada solicitação (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="a66ea-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="a66ea-160">A página é armazenada em cache, mas o anúncio de faixa é injetado dinamicamente pelo método auxiliar RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="a66ea-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="a66ea-161">**Figura 2 – o modo de exibição do índice exibindo um anúncio de faixa aleatória**</span><span class="sxs-lookup"><span data-stu-id="a66ea-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="a66ea-163">Resumo</span><span class="sxs-lookup"><span data-stu-id="a66ea-163">Summary</span></span>

<span data-ttu-id="a66ea-164">Este tutorial explica como você pode atualizar dinamicamente o conteúdo em uma página em cache.</span><span class="sxs-lookup"><span data-stu-id="a66ea-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="a66ea-165">Você aprendeu a usar o método HttpResponse.WriteSubstitution() para habilitar o conteúdo dinâmico a ser inserida em uma página em cache.</span><span class="sxs-lookup"><span data-stu-id="a66ea-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="a66ea-166">Você também aprendeu como encapsulam a chamada ao método WriteSubstitution() dentro de um método auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="a66ea-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="a66ea-167">Tirar proveito do armazenamento em cache sempre que possível – ele pode ter um impacto significativo sobre o desempenho de seus aplicativos web.</span><span class="sxs-lookup"><span data-stu-id="a66ea-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="a66ea-168">Conforme explicado neste tutorial, você pode tirar proveito do armazenamento em cache mesmo quando você precisa exibir conteúdo dinâmico em suas páginas.</span><span class="sxs-lookup"><span data-stu-id="a66ea-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a66ea-169">[Anterior](improving-performance-with-output-caching-vb.md)
[Próximo](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a66ea-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>