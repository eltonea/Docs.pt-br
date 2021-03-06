---
title: Auxiliar de Marca de Âncora no ASP.NET Core
author: pkellner
description: Descubra os atributos do Auxiliar de Marca de Âncora do ASP.NET e a função que cada atributo desempenha no comportamento de extensão da marca de âncora de HTML.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 31ff62b6bedb5e577a51f341c89d241d06a83ad3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="anchor-tag-helper-in-aspnet-core"></a>Auxiliar de Marca de Âncora no ASP.NET Core

De [Peter Kellner](http://peterkellner.net) e [Scott Addie](https://github.com/scottaddie)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

O [Auxiliar de Marca de Âncora](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) aprimora a marca de âncora HTML padrão (`<a ... ></a>`) adicionando novos atributos. Por convenção, os nomes de atributos são prefixados com `asp-`. O valor do atributo `href` do elemento de âncora renderizado é determinado pelos valores dos atributos `asp-`.

*SpeakerController* é usado em exemplos ao longo de todo este documento:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

Segue um inventário dos atributos `asp-`.

## <a name="asp-controller"></a>asp-controller

O atributo [asp-controller](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) designa o controlador usado para gerar a URL. A marcação a seguir lista todos os palestrantes:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

O HTML gerado:

```html
<a href="/Speaker">All Speakers</a>
```

Se o atributo `asp-controller` for especificado e `asp-action` não for, o valor `asp-action` padrão é a ação do controlador associada ao modo de exibição em execução no momento. Se `asp-action` for omitido da marcação anterior e o Auxiliar de Marca de Âncora for usado no modo de exibição *Índice* (*/home*) do *HomeController*, o HTML gerado será:

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>asp-action

O valor do atributo [asp-action](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) representa o nome da ação do controlador incluído no atributo `href` gerado. A seguinte marcação define o valor do atributo `href` gerado para a página de avaliações do palestrante:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

O HTML gerado:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Se nenhum atributo `asp-controller` for especificado, o controlador padrão que está chamando a exibição que executa a exibição atual é usado.

Se o valor do atributo `asp-action` for `Index`, nenhuma ação será anexada à URL, causando a invocação da ação `Index` padrão. A ação especificada (ou padrão), deve existir no controlador referenciado em `asp-controller`.

## <a name="asp-route-value"></a>asp-route-{value}

O atributo [asp-route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) permite um prefixo de roteamento de caractere curinga. Qualquer valor que esteja ocupando o espaço reservado `{value}` é interpretado como um possível parâmetro de roteamento. Se uma rota padrão não for encontrada, esse prefixo de rota será anexado ao atributo `href` gerado como um valor e parâmetro de solicitação. Caso contrário, ele será substituído no modelo de rota.

Considere a seguinte ação do controlador:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

Com um modelo de rota padrão definido em *Startup.Configure*:

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

O modo de exibição MVC usa o modelo fornecido pela ação, da seguinte forma:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

O espaço reservado `{id?}` da rota padrão foi correspondido. O HTML gerado:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Suponha que o prefixo de roteamento não faça parte do modelo de roteamento de correspondência, como com o seguinte modo de exibição de MVC:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker" 
       asp-action="Detail" 
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

O seguinte HTML é gerado porque o `speakerid` não foi encontrado na rota correspondente:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Se `asp-controller` ou `asp-action` não forem especificados, o mesmo processamento padrão será seguido, como no atributo `asp-route`.

## <a name="asp-route"></a>asp-route

O atributo [asp-route](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) é usado para criar um link de URL diretamente para uma rota nomeada. Usando [atributos de roteamento](xref:mvc/controllers/routing#attribute-routing), uma rota pode ser nomeada como mostrado em `SpeakerController` e usada em sua ação `Evaluations`:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

Na seguinte marcação, o atributo `asp-route` faz referência à rota nomeada:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

O Auxiliar de Marca de Âncora gera uma rota diretamente para essa ação de controlador usando a URL */Speaker/Evaluations*. O HTML gerado:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Se `asp-controller` ou `asp-action` for especificado além de `asp-route`, a rota gerada poderá não ser a esperada. Para evitar um conflito de rota, `asp-route` não deve ser usado com os atributos `asp-controller` ou `asp-action`.

## <a name="asp-all-route-data"></a>asp-all-route-data

O atributo [asp-all-route-data](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) oferece suporte à criação de um dicionário ou par chave-valor. A chave é o nome do parâmetro e o valor é o valor do parâmetro.

No exemplo a seguir, um dicionário é inicializado e passado para um modo de exibição Razor. Como alternativa, os dados podem ser passado com seu modelo.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

O código anterior gera o seguinte HTML:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

O dicionário `asp-all-route-data` é simplificado para produzir um querystring que atenda aos requisitos da ação `Evaluations` sobrecarregada:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

Se todas as chaves no dicionário corresponderem aos parâmetros, esses valores serão substituídos na rota conforme apropriado. Os outros valores não correspondentes são gerados como parâmetros de solicitação.

## <a name="asp-fragment"></a>asp-fragment

O atributo [asp-fragment](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) define um fragmento de URL para anexar à URL. O Auxiliar de Marca de Âncora adiciona o caractere de hash (#). Considere a seguinte marcação:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

O HTML gerado:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

As marcas de hash são úteis ao criar aplicativos do lado do cliente. Elas podem ser usadas para marcar e pesquisar com facilidade em JavaScript, por exemplo.

## <a name="asp-area"></a>asp-area

O atributo [asp-area](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) define o nome de área usado para definir a rota apropriada. O exemplo a seguir ilustra como o atributo de área causa um remapeamento das rotas. Definir `asp-area` como "Blogs" faz com que o diretório *Áreas/Blogs* seja prefixado nas rotas dos controladores e exibições associados a essa marca de âncora.

* **<Nome do projeto\>**
  * **wwwroot**
  * **Áreas**
    * **Blogs**
      * **Controladores**
        * *HomeController.cs*
      * **Exibições**
        * **Início**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *_ViewStart.cshtml*
  * **Controladores**

Dada a hierarquia do diretório anterior, a marcação para referenciar o arquivo *AboutBlog.cshtml* é:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

O HTML gerado:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Para que as áreas funcionem em um aplicativo MVC, o modelo de rota deve incluir uma referência à área, se ela existir. Esse modelo é representado pelo segundo parâmetro da chamada do método `routes.MapRoute` em *Startup.Configure*:[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>asp-protocol

O atributo [asp-protocol](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) é destinado a especificar um protocolo (como `https`) em sua URL. Por exemplo:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

O HTML gerado:

```html
<a href="https://localhost/Home/About">About</a>
```

O nome do host no exemplo é localhost, mas o Auxiliar de Marca de Âncora usará o domínio público do site ao gerar a URL.

## <a name="asp-host"></a>asp-host

O atributo [asp-host](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) é para especificar um nome do host na sua URL. Por exemplo:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

O HTML gerado:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>asp-page

O atributo [asp-page](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) é usado com as Páginas Razor. Use-o para definir um valor de atributo `href` da marca de âncora para uma página específica. Prefixar o nome de página com uma barra ("/") cria a URL.

O exemplo a seguir aponta para o participante da Página Razor:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

O HTML gerado:

```html
<a href="/Attendee">All Attendees</a>
```

O atributo `asp-page` é mutuamente exclusivo com os atributos `asp-route`, `asp-controller` e `asp-action`. No entanto, o `asp-page` pode ser usado com o `asp-route-{value}` para controlar o roteamento, como mostra a marcação a seguir:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

O HTML gerado:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>asp-page-handler

O atributo [asp-page-handler](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) é usado com as Páginas Razor. Ele se destina à vinculação a manipuladores de página específicos.

Considere o seguinte manipulador de página:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

A marcação associada do modelo de página vincula ao manipulador de página `OnGetProfile`. Observe que o prefixo `On<Verb>` do nome do método do manipulador de página é omitido no valor do atributo `asp-page-handler`. Se esse fosse um método assíncrono, o sufixo `Async` também seria omitido.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

O HTML gerado:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Recursos adicionais

* [Áreas](xref:mvc/controllers/areas)
* [Introdução às Páginas Razor](xref:mvc/razor-pages/index)
