---
title: Componentes de exibição no ASP.NET Core
author: rick-anderson
description: Saiba como os componentes de exibição são usados no ASP.NET Core e como adicioná-los aos aplicativos.
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: cdf44fc15ac64497b2589e8b7b289beb94c5b2c4
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2018
---
# <a name="view-components-in-aspnet-core"></a>Componentes de exibição no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="view-components"></a>Componentes da exibição

Os componentes de exibição são semelhantes às exibições parciais, mas são muito mais eficientes. Os componentes de exibição não usam a associação de modelos e dependem apenas dos dados fornecidos durante uma chamada a eles. Este artigo foi escrito com o ASP.NET Core MVC, mas os componentes de exibição também funcionam com Páginas do Razor.

Um componente de exibição:

* Renderiza uma parte em vez de uma resposta inteira.
* Inclui os mesmos benefícios de capacidade de teste e separação de interesses e encontrados entre um controlador e uma exibição.
* Pode ter parâmetros e uma lógica de negócios.
* É geralmente invocado em uma página de layout.

Os componentes de exibição destinam-se a qualquer momento em que há uma lógica de renderização reutilizável muito complexa para uma exibição parcial, como:

* Menus de navegação dinâmica
* Nuvem de marcas (na qual ela consulta o banco de dados)
* Painel de logon
* Carrinho de compras
* Artigos publicados recentemente
* Conteúdo da barra lateral em um blog típico
* Um painel de logon que é renderizado em cada página e mostra os links para logoff ou logon, dependendo do estado de logon do usuário

Um componente de exibição consiste em duas partes: a classe (normalmente derivada de [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) e o resultado que ele retorna (normalmente, uma exibição). Assim como os controladores, um componente de exibição pode ser um POCO, mas a maioria dos desenvolvedores desejará aproveitar os métodos e as propriedades disponíveis com a derivação de `ViewComponent`.

## <a name="creating-a-view-component"></a>Criando um componente de exibição

Esta seção contém os requisitos de alto nível para a criação de um componente de exibição. Mais adiante neste artigo, examinaremos cada etapa em detalhes e criaremos um componente de exibição.

### <a name="the-view-component-class"></a>A classe de componente de exibição

Uma classe de componente de exibição pode ser criada por um dos seguintes:

* Derivação de *ViewComponent*
* Decoração de uma classe com o atributo `[ViewComponent]` ou derivação de uma classe com o atributo `[ViewComponent]`
* Criação de uma classe em que o nome termina com o sufixo *ViewComponent*

Assim como os controladores, os componentes de exibição precisam ser classes públicas, não aninhadas e não abstratas. O nome do componente de exibição é o nome da classe com o sufixo "ViewComponent" removido. Também pode ser especificado de forma explícita com a propriedade `ViewComponentAttribute.Name`.

Uma classe de componente de exibição:

* Dá suporte total à [injeção de dependência do construtor](../../fundamentals/dependency-injection.md)

* Não participa do ciclo de vida do controlador, o que significa que não é possível usar [filtros](../controllers/filters.md) em um componente de exibição

### <a name="view-component-methods"></a>Métodos de componente de exibição

Um componente de exibição define sua lógica em um método `InvokeAsync` que retorna um `IViewComponentResult`. Os parâmetros são recebidos diretamente da invocação do componente de exibição, não da associação de modelos. Um componente de exibição nunca manipula uma solicitação diretamente. Normalmente, um componente de exibição inicializa um modelo e passa-o para uma exibição chamando o método `View`. Em resumo, os métodos de componente de exibição:

* Definem um método `InvokeAsync` que retorna um `IViewComponentResult`
* Normalmente, inicializam um modelo e passam-o para uma exibição chamando o método `ViewComponent` `View`
* Os parâmetros são recebidos do método de chamada, não do HTTP e não há nenhuma associação de modelos
* Não podem ser acessados diretamente como um ponto de extremidade HTTP e são invocados no código (normalmente em uma exibição). Um componente de exibição nunca manipula uma solicitação
* São sobrecarregados na assinatura, em vez de nos detalhes da solicitação HTTP atual

### <a name="view-search-path"></a>Caminho de pesquisa de exibição

O tempo de execução pesquisa a exibição nos seguintes caminhos:

   * Views/\<nome_do_controlador>/Components/\<nome_do_componente_da_exibição>/\<nome_do_modo_de_exibição>
   * Views/Shared/Components/\<nome_do_componente_da_exibição>/\<nome_do_modo_de_exibição>

O nome de exibição padrão de um componente de exibição é *Default*, o que significa que o arquivo de exibição geralmente será nomeado *Default.cshtml*. Especifique outro nome de exibição ao criar o resultado do componente de exibição ou ao chamar o método `View`.

Recomendamos que você nomeie o arquivo de exibição *Default.cshtml* e use o caminho *Views/Shared/Components/\<nome_do_componente_da_exibição>/\<nome_do_modo_de_exibição>*. O componente de exibição `PriorityList` usado nesta amostra usa *Views/Shared/Components/PriorityList/Default.cshtml* como a exibição do componente de exibição.

## <a name="invoking-a-view-component"></a>Invocando um componente de exibição

Para usar o componente de exibição, chame o seguinte em uma exibição:

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

Os parâmetros serão passados para o método `InvokeAsync`. O componente de exibição `PriorityList` desenvolvido no artigo é invocado por meio do arquivo de exibição *Views/Todo/Index.cshtml*. A seguir, o método `InvokeAsync` é chamado com dois parâmetros:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Invocando um componente de exibição como um Auxiliar de Marca

Para o ASP.NET Core 1.1 e superior, invoque um componente de exibição como um [Auxiliar de Marca](xref:mvc/views/tag-helpers/intro):

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Os parâmetros de classe e de método na formatação Pascal Case para Auxiliares de Marca são convertidos em seu [kebab case em minúsculas](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Para invocar um componente de exibição, o Auxiliar de Marca usa o elemento `<vc></vc>`. O componente de exibição é especificado da seguinte maneira:

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Observação: para usar um Componente de Exibição como um Auxiliar de Marca, é necessário registrar o assembly que contém o Componente de Exibição usando a diretiva `@addTagHelper`. Por exemplo, se o Componente de Exibição está em um assembly chamado "MyWebApp", adicione a seguinte diretiva ao arquivo `_ViewImports.cshtml`:

```cshtml
@addTagHelper *, MyWebApp
```

Registre um Componente de Exibição como um Auxiliar de Marca em qualquer arquivo que referencia o Componente de Exibição. Consulte [Gerenciando o escopo do Auxiliar de Marca](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) para obter mais informações sobre como registrar Auxiliares de Marca.

O método `InvokeAsync` usado neste tutorial:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Na marcação do Auxiliar de Marca:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Na amostra acima, o componente de exibição `PriorityList` torna-se `priority-list`. Os parâmetros para o componente de exibição são passados como atributos em kebab case em minúsculas.

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Invocando um componente de exibição diretamente em um controlador

Os componentes de exibição normalmente são invocados em uma exibição, mas você pode invocá-los diretamente em um método do controlador. Embora os componentes de exibição não definam pontos de extremidade como controladores, você pode implementar com facilidade uma ação do controlador que retorna o conteúdo de um `ViewComponentResult`.

Neste exemplo, o componente de exibição é chamado diretamente no controlador:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Passo a passo: criando um componente de exibição simples

[Baixe](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), compile e teste o código inicial. É um projeto simples com um controlador `Todo` que exibe uma lista de itens *Todo*.

![Lista de ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Adicionar uma classe ViewComponent

Crie uma pasta *ViewComponents* e adicione a seguinte classe `PriorityListViewComponent`:

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Observações sobre o código:

* As classes de componente de exibição podem ser contidas em **qualquer** pasta do projeto.
* Como o nome da classe PriorityList**ViewComponent** termina com o sufixo **ViewComponent**, o tempo de execução usará a cadeia de caracteres "PriorityList" ao referenciar o componente de classe em uma exibição. Explicarei isso mais detalhadamente mais adiante.
* O atributo `[ViewComponent]` pode alterar o nome usado para referenciar um componente de exibição. Por exemplo, poderíamos nomear a classe `XYZ` e aplicar o atributo `ViewComponent`:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* O atributo `[ViewComponent]` acima instrui o seletor de componente de exibição a usar o nome `PriorityList` ao procurar as exibições associadas ao componente e a usar a cadeia de caracteres "PriorityList" ao referenciar o componente de classe em uma exibição. Explicarei isso mais detalhadamente mais adiante.
* O componente usa a [injeção de dependência](../../fundamentals/dependency-injection.md) para disponibilizar o contexto de dados.
* `InvokeAsync` expõe um método que pode ser chamado em uma exibição e pode usar um número arbitrário de argumentos.
* O método `InvokeAsync` retorna o conjunto de itens `ToDo` que atendem aos parâmetros `isDone` e `maxPriority`.

### <a name="create-the-view-component-razor-view"></a>Criar a exibição do Razor do componente de exibição

* Crie a pasta *Views/Shared/Components*. Essa pasta **deve** nomeada *Components*.

* Crie a pasta *Views/Shared/Components/PriorityList*. Esse nome de pasta deve corresponder ao nome da classe do componente de exibição ou ao nome da classe menos o sufixo (se seguimos a convenção e usamos o sufixo *ViewComponent* no nome da classe). Se você usou o atributo `ViewComponent`, o nome da classe precisa corresponder à designação de atributo.

* Crie uma exibição do Razor *Views/Shared/Components/PriorityList/Default.cshtml*: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   A exibição do Razor usa uma lista de `TodoItem` e exibe-os. Se o método `InvokeAsync` do componente de exibição não passar o nome da exibição (como em nossa amostra), *Default* será usado como o nome da exibição, por convenção. Mais adiante no tutorial, mostrarei como passar o nome da exibição. Para substituir o estilo padrão de um controlador específico, adicione uma exibição à pasta de exibição específica ao controlador (por exemplo, *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Se o componente de exibição é específico ao controlador, adicione-o à pasta específica ao controlador (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Adicione um `div` que contém uma chamada ao componente da lista de prioridades à parte inferior do arquivo *Views/Todo/index.cshtml*:

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

A marcação `@await Component.InvokeAsync` mostra a sintaxe para chamar componentes de exibição. O primeiro argumento é o nome do componente que queremos invocar ou chamar. Os parâmetros seguintes são passados para o componente. `InvokeAsync` pode usar um número arbitrário de argumentos.

Teste o aplicativo. A seguinte imagem mostra a lista ToDo e os itens de prioridade:

![lista de tarefas pendentes e itens de prioridade](view-components/_static/pi.png)

Também chame o componente de exibição diretamente no controlador:

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![itens de prioridade da ação IndexVC](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Especificando um nome de exibição

Um componente de exibição complexo pode precisar especificar uma exibição não padrão em algumas condições. O código a seguir mostra como especificar a exibição "PVC" no método `InvokeAsync`. Atualize o método `InvokeAsync` na classe `PriorityListViewComponent`.

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Copie o arquivo *Views/Shared/Components/PriorityList/Default.cshtml* para uma exibição nomeada *Views/Shared/Components/PriorityList/PVC.cshtml*. Adicione um cabeçalho para indicar que a exibição PVC está sendo usada.

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Atualize *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Execute o aplicativo e verifique a exibição PVC.

![Componente de exibição de prioridade](view-components/_static/pvc.png)

Se a exibição PVC não é renderizada, verifique se você está chamando o componente de exibição com uma prioridade igual a 4 ou superior.

### <a name="examine-the-view-path"></a>Examinar o caminho de exibição

* Altere o parâmetro de prioridade para três ou menos para que a exibição de prioridade não seja retornada.
* Renomeie temporariamente o *Views/Todo/Components/PriorityList/Default.cshtml* como *1Default.cshtml*.
* Teste o aplicativo. Você obterá o seguinte erro:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Copie *Views/Todo/Components/PriorityList/1Default.cshtml* para *Views/Shared/Components/PriorityList/Default.cshtml*.
* Adicione uma marcação ao componente de exibição Todo *Shared* para indicar que a exibição foi obtida da pasta *Shared*.
* Teste o componente de exibição **Shared**.

![Saída de ToDo com o componente de exibição Shared](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Evitando cadeias de caracteres mágicas

Se deseja obter segurança em tempo de compilação, substitua o nome do componente de exibição embutido em código pelo nome da classe. Crie o componente de exibição sem o sufixo "ViewComponent":

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Adicione uma instrução `using` ao arquivo de exibição do Razor e use o operador `nameof`:

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a>Recursos adicionais

* [Injeção de dependência em exibições](xref:mvc/views/dependency-injection)
