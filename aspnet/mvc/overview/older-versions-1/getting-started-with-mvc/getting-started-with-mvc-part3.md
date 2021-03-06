---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Adicionando uma exibição | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que leituras e gravações de banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 978d7980274c072ed559b54ed69ab86245b6c5a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-view"></a>Adicionando uma exibição
====================
by [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que leituras e gravações de banco de dados. Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.


Nesta seção, vamos examinar como podemos ter nossa classe HelloWorldController usar um arquivo de modelo de exibição para encapsular corretamente gerar respostas HTML para um cliente.

Vamos começar com o uso de um modelo de exibição com nosso método de índice. Nosso método é chamado de índice e está no HelloWorldController. Atualmente, nosso método Index () retorna uma cadeia de caracteres com uma mensagem que é inserido no código dentro da classe do controlador.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Agora vamos alterar o método de índice em vez disso, ter esta aparência:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Agora vamos adicionar um modelo de exibição ao nosso projeto que podemos usar nosso método Index (). Para fazer isso, clique com o mouse em algum lugar no meio do método de índice e clique em Adicionar modo de exibição...

![imagem](getting-started-with-mvc-part3/_static/image1.png)

Isso abrirá a caixa de diálogo "Adicionar modo de exibição", que fornece algumas opções para como quer criar um modelo de exibição pode ser usado pelo método nosso índice. Por enquanto, não altere nada e simplesmente clique no botão Adicionar.

[![Adicionar caixa de diálogo de exibição](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Depois de clicar em Adicionar, uma nova pasta e um novo arquivo serão exibida na pasta da solução, como visto aqui. Agora, tenho uma pasta HelloWorld em modos de exibição e um arquivo Index.aspx dentro dessa pasta.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

O novo arquivo de índice também já está aberto e pronto para edição. Adicione um texto em primeiro &lt;h2&gt;índice&lt;/h2&gt; como "Olá, mundo".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Execute o aplicativo e visite [ `http://localhost:xx/HelloWorld` ](http://localhostxx) novamente em seu navegador. O método de índice em nosso controlador neste exemplo não realiza nenhum trabalho, mas chamada "retorno View()" que indicou que queremos usar um arquivo de modelo de exibição para renderizar uma resposta de volta ao cliente. Porque estamos não especificar explicitamente o nome do arquivo do modelo de exibição para usar, ASP.NET MVC padrão usando o arquivo de exibição Index.aspx dentro da pasta \Views\HelloWorld. Agora podemos ver a cadeia de caracteres que é embutida em nossa visualização.

[![Índice - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Parece muito bom. No entanto, observe que o título do navegador diz "Index" e grande título na página diz "Meu aplicativo MVC". Vamos alterar os.

### <a name="changing-views-and-master-pages"></a>Alterando modos de exibição e páginas mestras

Primeiro, vamos alterar o texto "Meu aplicativo MVC". Esse texto é compartilhado e aparece em cada página. Na verdade, aparece em apenas um local em nosso código, mesmo que seja em cada página em nosso aplicativo. Vá para a pasta /Views/Shared no Gerenciador de soluções e abra o arquivo Site.Master. Esse arquivo é chamado de uma página mestra e é o compartilhado "shell" que usam todas as nossas páginas.

Observe que diz ContentPlaceholder "MainContent" texto neste arquivo.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Espaço reservado é onde todas as páginas que você criar aparecerão, "encapsuladas" na página mestra. Tente alterar o título, em seguida, executar o aplicativo e visite várias páginas. Você observará que a uma alteração é exibido em várias páginas.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

Agora, todas as páginas terão o cabeçalho principal - que é H1 - de "Meu aplicativo de filme MVC". Que manipula o texto em branco na parte superior existe que é compartilhada por todas as páginas.

Aqui está o Site.Master em sua totalidade com nosso título alterado:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

Agora, vamos alterar o título da página de índice.

Open /HelloWorld/Index.aspx. Há dois locais para alterar. Primeiro, o título que aparece no título do navegador, o cabeçalho secundário - que também é H2. Vou deixá-los ligeiramente diferentes para ver qual parte do código altera qual parte do aplicativo.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Executar seu aplicativo e visite /Movies. Observe que o título do navegador, o cabeçalho principal e os cabeçalhos da secundários foram alteradas. É fácil fazer grandes alterações em seu aplicativo com pequenas alterações ao modo de exibição.

[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Nosso pouco de "dados" (no caso "Hello World!" mensagem) foi codificado embora. Temos V (Views), e temos C (controladores), mas ainda não M (modelo). Em breve examinaremos como criar um banco de dados e recuperar dados de modelo dele.

## <a name="passing-a-viewmodel"></a>Passando um ViewModel

Antes de ir para um banco de dados e falar sobre modelos, no entanto, vamos primeiro falar sobre "ViewModels". Esses são objetos que representam o que requer um modelo de exibição para renderizar uma resposta HTML para um cliente. Eles normalmente são criados e passado por uma classe de controlador para um modelo de exibição e devem conter apenas os dados que requer que o modelo de exibição - e nada mais.

Anteriormente com nosso exemplo de HelloWorld, nosso método de ação Welcome() obteve um nome e um parâmetro numTimes e enviá-lo para o navegador. Em vez de fazer com que o controlador de continuar a processar essa resposta diretamente, vamos fazer em vez disso, uma pequena classe para manter esses dados e passá-lo de um modelo de exibição para renderizar a resposta HTML usá-lo de volta. Dessa forma, o controlador está preocupado com algo e o modelo de exibição outro – que nos permite manter "separação limpa de preocupações" em nosso aplicativo.

Retornar para o arquivo HelloWorldController.cs e adicione uma nova classe de "WelcomeViewModel" e alterar o método de boas-vindo dentro de seu controlador. Aqui está o HelloWorldController.cs completa com a nova classe no mesmo arquivo.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Mesmo que ele esteja em várias linhas, nosso método bem-vindo é realmente apenas duas instruções de código. A primeira instrução empacota os dois parâmetros em um objeto ViewModel e passa a segundo objeto resultante para o modo de exibição.

Agora precisamos de um modelo de exibição bem-vindo! Clique com o botão direito no método de boas-vindo e selecione Adicionar modo de exibição. Neste momento, vamos verificar "Criar uma exibição fortemente tipada" e selecione nossa classe WelcomeViewModel na lista suspensa. Esse novo modo de exibição somente saberá sobre WelcomeViewModels e não outros tipos de objetos.

> *Observação: Você precisará ter compiladas uma vez depois de adicionar o WelcomeViewModel para aparecerão na lista suspensa.*


Aqui está o que deve ser a aparência de sua caixa de diálogo Adicionar modo de exibição. Clique no botão Adicionar. ![Adicionar que modo de exibição dentro de um círculo](getting-started-with-mvc-part3/_static/image10.png)

Adicione este código sob o &lt;h2&gt; no seu novo Welcome.aspx. Vamos fazer um loop e diga Olá quantas vezes o usuário diz que deveríamos!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Além disso, observe enquanto você estiver digitando que porque disse este modo de exibição sobre o WelcomeViewModel (são casados, lembre-se?) que obtemos Intellisense útil sempre que referenciar nosso objeto de modelo, como visto na captura de tela abaixo:

[![Código-fonte NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Execute o aplicativo e visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` novamente. Agora estamos dando dados da URL, ele é passado para o nosso controlador automaticamente, nosso controlador empacota os dados em um ViewModel e passa esse objeto para nossa visão. O modo de exibição que exibe os dados como HTML para o usuário.

[![Bem-vindo - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Bem, isso é um tipo de um "M" para o modelo, mas não o tipo de banco de dados. Vejamos o que podemos aprendeu e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part2.md)
> [Próximo](getting-started-with-mvc-part4.md)
