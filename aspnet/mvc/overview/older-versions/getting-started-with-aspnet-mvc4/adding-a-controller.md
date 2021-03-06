---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Adicionando um controlador | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples de seguir e demonstração...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: bb76c0a87d935322406b9d8e18fbdb3e41f327f5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30868861"
---
<a name="adding-a-controller"></a>Adicionando um controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É muito mais simples a seguir, mais segura e demonstra mais recursos.


Representa o MVC *model-view-controller*. MVC é um padrão para o desenvolvimento de aplicativos que são bem projetada, podem ser testados e fácil de manter. Aplicativos MVC contêm:

- **M** odels: Classes que representam os dados do aplicativo e que usam a lógica de validação para impor regras de negócio para os dados.
- **V** iews: arquivos de modelo que seu aplicativo usa para gerar dinamicamente as respostas HTML.
- **C** ontrollers: Classes que lidam com solicitações recebidas de navegador, recuperar dados de modelo e, em seguida, especificar modelos de exibição que retornam uma resposta para o navegador.

Vamos ser abrangendo todos esses conceitos nesta série de tutoriais e mostram como usá-las para criar um aplicativo.

Vamos começar criando uma classe de controlador. Em **Solution Explorer**, com o botão direito do *controladores* pasta e, em seguida, selecione **Adicionar controlador**.

![](adding-a-controller/_static/image1.png)

Nome do novo controlador &quot;HelloWorldController&quot;. Deixe o modelo padrão como **controlador MVC vazio** e clique em **adicionar**.

![Adicionar controlador](adding-a-controller/_static/image2.png)

Observe na **Solution Explorer** que um novo arquivo tem foi criado com o nome *HelloWorldController.cs*. O arquivo é aberto no IDE.

![](adding-a-controller/_static/image3.png)

Substitua o conteúdo do arquivo com o código a seguir.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Os métodos do controlador retornará uma cadeia de caracteres de HTML como um exemplo. O controlador é nomeado `HelloWorldController` e o primeiro método acima é chamado `Index`. Vamos chamá-la em um navegador. Execute o aplicativo (pressione F5 ou Ctrl + F5). No navegador, acrescente &quot;HelloWorld&quot; para o caminho na barra de endereços. (Por exemplo, na ilustração abaixo, ele `http://localhost:1234/HelloWorld.`) a página no navegador se parecerá com a captura de tela a seguir. No método, o código retornado diretamente uma cadeia de caracteres. Você disse que o sistema retornar apenas alguns HTML e fez isso!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC chama classes diferentes de controlador (e os métodos de ação diferente dentro delas) dependendo da URL de entrada. A lógica de roteamento URL padrão usada pelo ASP.NET MVC usa um formato como este para determinar o que o código para chamar:

`/[Controller]/[ActionName]/[Parameters]`

A primeira parte da URL determina a classe do controlador para executar. Portanto */HelloWorld* mapeia para o `HelloWorldController` classe. A segunda parte da URL determina o método de ação na classe para executar. Portanto *HelloWorld/índice* causaria o `Index` método o `HelloWorldController` classe para executar. Observe que tivemos somente para navegar até */HelloWorld* e `Index` método foi usado por padrão. Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador, se ainda não for explicitamente especificado.

Navegue para `http://localhost:xxxx/HelloWorld/Welcome`. O `Welcome` método é executado e retorna a cadeia de caracteres &quot;esse é o método de ação de boas-vindas... &quot;. O mapeamento de MVC padrão é `/[Controller]/[ActionName]/[Parameters]`. Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método de ação. Você ainda não usou a parte `[Parameters]` da URL.

![](adding-a-controller/_static/image5.png)

Vamos modificar o exemplo um pouco para que você pode passar algumas informações de parâmetro da URL para o controlador (por exemplo, *HelloWorld/boas-vindas? name = Scott&amp;numtimes = 4*). Alterar sua `Welcome` método incluir dois parâmetros, conforme mostrado abaixo. Observe que o código usa o recurso de parâmetro opcional do c# para indicar que o `numTimes` parâmetro deve 1 como padrão se nenhum valor é passado para esse parâmetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Execute o aplicativo e navegue até a URL de exemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Você pode tentar valores diferentes para `name` e `numtimes` na URL. O [sistema de associação do ASP.NET MVC modelo](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados da cadeia de consulta na barra de endereços para parâmetros em seu método.

![](adding-a-controller/_static/image6.png)

Nos dois exemplos do controlador está executando o &quot;VC&quot; parte do MVC — ou seja, o trabalho de exibição e controlador. O controlador está retornando HTML diretamente. Normalmente, você não deseja controladores retornando HTML diretamente, desde que se torna muito difícil de código. Em vez disso, usaremos normalmente um arquivo de modelo de exibição separada para ajudar a gerar a resposta HTML. Vamos Avançar como podemos fazer isso.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-4.md)
> [Próximo](adding-a-view.md)
