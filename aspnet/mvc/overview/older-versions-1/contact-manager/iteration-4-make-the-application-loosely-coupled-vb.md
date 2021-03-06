---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: 'Iteração #4 – tornar o aplicativo flexível (VB) | Microsoft Docs'
author: microsoft
description: Essa terceira iteração, podemos aproveitar vários padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contato. Para...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: d953a1b786c802c070619e553e27d88f2ded149c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="iteration-4--make-the-application-loosely-coupled-vb"></a>Iteração #4 – tornar o aplicativo flexível (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar o código](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> Essa terceira iteração, podemos aproveitar vários padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contato. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Criando um aplicativo ASP.NET MVC de gerenciamento de contatos (VB)

Esta série de tutoriais, vamos criar um aplicativo de gerenciamento de entre em contato com toda a partir do início ao fim. O aplicativo Gerenciador de entrar em contato com permite armazenar informações de contato - nomes, números de telefone e endereços de email - para obter uma lista de pessoas.

Vamos criar o aplicativo em várias iterações. Com cada iteração, podemos melhorar gradualmente o aplicativo. O objetivo dessa abordagem iteração vários é para que você possa entender o motivo para cada alteração.

- Iteração #1 - criar o aplicativo. A primeira iteração, criamos o gerente do contato da maneira mais simples possível. Adicionamos suporte para operações de banco de dados básicos: criar, leitura, atualização e exclusão (CRUD).

- Iteração #2 - Verifique o aplicativo parecer adequado. Essa iteração, podemos melhorar a aparência do aplicativo, modificando a página mestra do ASP.NET MVC exibição padrão e em cascata a folha de estilos.

- Iteração #3 - adicionar validação do formulário. Na terceira iteração, podemos adicionar validação de forma básica. Vamos impedir que pessoas enviando um formulário sem concluir os campos obrigatórios do formulário. Além disso, podemos validar endereços de email e números de telefone.

- Iteração #4 - Verifique o aplicativo acoplados de forma flexível. Essa terceira iteração, podemos aproveitar vários padrões de design de software para facilitar a manutenção e modificar o aplicativo Gerenciador de contato. Por exemplo, podemos refatorar nosso aplicativo para usar o padrão de repositório e o padrão de injeção de dependência.

- Iteração #5 - criar testes de unidade. Na quinta iteração, fazemos nosso aplicativo mais fácil de manter e modificar adicionando testes de unidade. Vamos simular nossas classes de modelo de dados e criar testes de unidade para nossos controladores e lógica de validação.

- Iteração #6 - Use desenvolvimento controlado por testes. Essa iteração do sexto, adicionamos novas funcionalidades para nosso aplicativo escrevendo testes de unidade primeiro e escrever código os testes de unidade. Essa iteração, adicionamos grupos de contatos.

- Iteração #7 - adicionar funcionalidade Ajax. A sétima iteração, podemos melhorar a capacidade de resposta e o desempenho do nosso aplicativo, adicionando suporte para Ajax.

## <a name="this-iteration"></a>Essa iteração

Essa iteração quarto do aplicativo Gerenciador de contato, podemos refatorar o aplicativo para tornar o aplicativo mais flexível. Quando um aplicativo é flexível, você pode modificar o código em uma parte do aplicativo sem a necessidade de modificar o código em outras partes do aplicativo. Aplicativos acoplados de forma flexível são mais resilientes a alterar.

No momento, toda a lógica de acesso e a validação de dados usada pelo aplicativo Gerenciador de contato está contida nas classes de controlador. Isso não é recomendável. Sempre que você precisa modificar uma parte do seu aplicativo, você corre o risco de introduzir erros em outra parte do seu aplicativo. Por exemplo, se você modificar sua lógica de validação, você corre o risco de introduzir novos bugs em sua lógica de acesso ou controlador de dados.

> [!NOTE] 
> 
> (SRP), uma classe nunca deve ter mais de um motivo para alterar. Combinação de controlador, validação e lógica de banco de dados é uma violação em grandes quantidades do princípio da responsabilidade única.


Há várias razões pelas quais você talvez precise modificar seu aplicativo. Talvez seja necessário adicionar um novo recurso ao seu aplicativo, talvez seja necessário corrigir um bug em seu aplicativo ou talvez você precise modificar como um recurso do aplicativo é implementado. Aplicativos raramente são estáticos. Eles tendem a crescer e modifica ao longo do tempo.

Por exemplo, imagine que você decidir alterar como implementar a camada de acesso a dados. Direita agora, o aplicativo Gerenciador de contato usa o Microsoft Entity Framework para acessar o banco de dados. No entanto, você pode decidir migrar para uma tecnologia de acesso de dados novos ou alternativos, como ADO.NET Data Services ou NHibernate. No entanto, como o código de acesso a dados não é isolado do código de validação e o controlador, não é possível modificar o código de acesso de dados em seu aplicativo sem modificar outro código que não está diretamente relacionado ao acesso a dados.

Quando um aplicativo é flexível, por outro lado, você pode fazer alterações em uma parte de um aplicativo sem tocar em outras partes de um aplicativo. Por exemplo, você pode alternar as tecnologias de acesso de dados sem modificar sua lógica de validação ou controlador.

Essa iteração, podemos aproveitar vários padrões de design de software que nos permitem refatorar nosso aplicativo Contact Manager em um aplicativo mais flexível. Quando são concluídas, o gerente do contato ganha t fazer qualquer coisa que ele não foi fazer antes. No entanto, ser capazes de alterar o aplicativo mais facilmente no futuro.

> [!NOTE] 
> 
> Refatoração é o processo de reconfiguração de um aplicativo de forma que ele não perderá qualquer funcionalidade existente.


## <a name="using-the-repository-software-design-pattern"></a>Usando o padrão de Design de Software do repositório

É nossa primeira alteração tirar proveito de um padrão de design de software chamado o padrão de repositório. Vamos usar o padrão de repositório para isolar o nosso código de acesso a dados do restante do nosso aplicativo.

Implementando o padrão de repositório exige concluir as etapas a seguir:

1. Criar uma interface
2. Criar uma classe concreta que implementa a interface

Primeiro, precisamos criar uma interface que descreve todos os métodos de acesso de dados que são necessárias para executar. A interface IContactManagerRepository está contida na listagem 1. Essa interface descreve cinco métodos: CreateContact(), DeleteContact(), EditContact(), GetContact e ListContacts().

**Listando 1 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

Em seguida, é preciso criar uma classe concreta que implementa a interface IContactManagerRepository. Como estamos usando o Entity Framework da Microsoft para acessar o banco de dados, vamos criar uma nova classe chamada EntityContactManagerRepository. Essa classe está contida na listagem 2.

**Listing 2 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Observe que a classe EntityContactManagerRepository implementa a interface IContactManagerRepository. A classe implementa todas as cinco dos métodos descritos por essa interface.

Você talvez esteja se perguntando por que é necessário se preocupar com uma interface. Por que precisamos criar uma interface e uma classe que implementa?

Com uma exceção, o restante do nosso aplicativo irá interagir com a interface e não a classe concreta. Em vez de chamar os métodos expostos pela classe EntityContactManagerRepository, que chamaremos os métodos expostos pela interface IContactManagerRepository.

Dessa forma, podemos implementar a interface com uma nova classe sem a necessidade de modificar o restante do nosso aplicativo. Por exemplo, no futuro, podemos talvez queira implementar uma classe DataServicesContactManagerRepository que implementa a interface IContactManagerRepository. A classe DataServicesContactManagerRepository pode usar o ADO.NET Data Services para acessar um banco de dados em vez de Microsoft Entity Framework.

Se nosso código do aplicativo é programado em relação a interface IContactManagerRepository em vez da classe EntityContactManagerRepository concreta, em seguida, é possível alternar classes concretas sem modificar o restante do nosso código. Por exemplo, é possível alternar da classe EntityContactManagerRepository para a classe DataServicesContactManagerRepository sem modificar nossa lógica de acesso ou validação de dados.

Programação em interfaces (abstrações) em vez de classes concretas torna o nosso aplicativo mais resiliente a alterar.

> [!NOTE] 
> 
> Você pode criar rapidamente uma interface de uma classe concreta dentro do Visual Studio, selecionando a opção de menu Refatorar, extrair Interface. Por exemplo, você pode primeiro criar a classe EntityContactManagerRepository e, em seguida, use extrair Interface para gerar a interface IContactManagerRepository automaticamente.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Usando o padrão de Design de Software de injeção de dependência

Agora que migramos nosso código de acesso a dados para uma classe separada do repositório, é necessário modificar o nosso controlador de contato para usar essa classe. Nós o conduziremos vantagem de um padrão de design de software chamado injeção de dependência para usar a classe de repositório em nosso controlador.

O controlador de contato modificado está contido na listagem 3.

**A listagem 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Observe que o controlador de contato na listagem 3 tem dois construtores. O primeiro construtor passa uma instância concreta da interface IContactManagerRepository para o segundo construtor. Os usos de classe do controlador de contato *injeção de dependência de construtor*.

Um e apenas local que a classe EntityContactManagerRepository é usada é no construtor primeiro. O restante da classe usa a interface IContactManagerRepository em vez de classe concreta EntityContactManagerRepository.

Isso torna fácil alternar implementações da classe IContactManagerRepository no futuro. Se você quiser usar a classe DataServicesContactRepository em vez da classe EntityContactManagerRepository, basta modificar o primeiro construtor.

Injeção de dependência de construtor também torna a classe do controlador contato muito testável. Em testes de unidade, você pode instanciar o controlador de contato, passando uma implementação fictícia da classe IContactManagerRepository. Esse recurso de injeção de dependência será muito importante para nós na próxima iteração quando criamos testes de unidade para o aplicativo Gerenciador de contato.

> [!NOTE] 
> 
> Se você deseja desassociar completamente a classe do controlador de contato de uma implementação específica da interface IContactManagerRepository, em seguida, você pode tirar proveito de uma estrutura que oferece suporte a injeção de dependência, como StructureMap ou o Microsoft Entity Framework (MEF). Tirando proveito de uma estrutura de injeção de dependência, você nunca precisa se referir a uma classe concreta no seu código.


## <a name="creating-a-service-layer"></a>Criando uma camada de serviço

Você deve ter notado que nossa lógica de validação ainda é misturada com nossa lógica do controlador na classe de controlador modificado na listagem 3. Pelo mesmo motivo que é uma boa ideia isolar nossa lógica de acesso a dados, é uma boa ideia isolar nossa lógica de validação.

Para corrigir esse problema, podemos criar um separado [camada de serviço](http://martinfowler.com/eaaCatalog/serviceLayer.html). A camada de serviço é uma camada separada que pode ser inserido entre nosso controlador e as classes de repositório. A camada de serviço contém nossa lógica de negócios, incluindo todos os nossos lógica de validação.

O ContactManagerService está contido na listagem 4. Contém a lógica de validação da classe do controlador de contato.

**A listagem 4 - Models\ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Observe que o construtor para o ContactManagerService requer um ValidationDictionary. A camada de serviço se comunica com a camada de controlador por este ValidationDictionary. Discutiremos ValidationDictionary detalhadamente na seção a seguir quando podemos discutir o padrão de decorador.

Além disso, observe que o ContactManagerService implementa a interface IContactManagerService. Você sempre deve tentar programar interfaces em vez de classes concretas. Outras classes no aplicativo Gerenciador de contato não interagir diretamente com a classe ContactManagerService. Em vez disso, com uma exceção, o restante do aplicativo Gerenciador de contato é programado em relação a interface IContactManagerService.

A interface IContactManagerService está contida na listagem 5.

**Listagem 5 - Models\IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

A classe do controlador contato modificada está contida na listagem 6. Observe que o controlador de contato não interage com o repositório de ContactManager. Em vez disso, o controlador de contato interage com o serviço ContactManager. Cada camada é isolada tanto quanto possível de outras camadas.

**Listando 6 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

Nosso aplicativo não executa afoul do princípio de responsabilidade única (SRP). O controlador de contato na listagem 6 foi eliminado de toda responsabilidade diferente de controlar o fluxo de execução do aplicativo. Toda a lógica de validação foi removida do controlador de contato e enviados por push para a camada de serviço. Toda a lógica de banco de dados foi pressionado na camada de repositório.

## <a name="using-the-decorator-pattern"></a>Usando o padrão de decorador

Queremos desacoplar completamente nossa camada de serviço de camada nosso controlador. Em princípio, podemos deve ser capazes de compilar nosso camada de serviço em um assembly separado da camada nosso controlador sem a necessidade de adicionar uma referência ao nosso aplicativo MVC.

No entanto, nossa camada de serviço precisa ser capaz de passar mensagens de erro de validação para a camada de controlador. Como podemos habilitar a camada de serviço para se comunicar mensagens de erro de validação sem acoplamento o controlador e a camada de serviço? Podemos pode tirar proveito de um padrão de design de software denominado o [padrão de decorador](http://en.wikipedia.org/wiki/Decorator_pattern).

Um controlador usa um ModelStateDictionary denominado ModelState para representar erros de validação. Portanto, talvez seja tentado a passar ModelState da camada de controlador para a camada de serviço. No entanto, usar ModelState na camada de serviço tornaria a camada de serviço dependente de um recurso da estrutura ASP.NET MVC. Isso seria incorreto porque, um dia, talvez você queira usar a camada de serviço com um aplicativo do WPF em vez de um aplicativo ASP.NET MVC. Nesse caso, você não seria t deseja referenciar a estrutura ASP.NET MVC para usar a classe ModelStateDictionary.

O padrão de decorador permite encapsular uma classe existente em uma nova classe para implementar uma interface. Nosso projeto Contact Manager inclui a classe de ModelStateWrapper contida na listagem 7. A classe ModelStateWrapper implementa a interface listagem 8.

**Listando 7 - Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Listing 8 - Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Se você dar uma olhada Fechar na listagem 5, em seguida, você verá que a camada de serviço ContactManager usa a interface IValidationDictionary exclusivamente. O serviço ContactManager não é dependente da classe ModelStateDictionary. Quando o controlador de contato cria o serviço ContactManager, o controlador encapsula seu ModelState assim:

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Resumo

Essa iteração, nós não adicionou nenhuma nova funcionalidade para o aplicativo Gerenciador de contato. O objetivo dessa iteração era refatorar o aplicativo Gerenciador de contato assim que é mais fácil de manter e modificar.

Primeiro, implementamos o padrão de design de software do repositório. Migramos todo o código de acesso de dados para uma classe de repositório ContactManager separada.

Podemos também isolado nossa lógica de validação de nossa lógica do controlador. Nós criamos uma camada de serviço separado que contém todos os nossos código de validação. A camada de controlador interage com a camada de serviço e a camada de serviço interage com a camada de repositório.

Quando criamos a camada de serviço, aproveitamos o padrão de decorador para isolar ModelState da nossa camada de serviço. Em nossa camada de serviço, é programado em relação a interface IValidationDictionary em vez de ModelState.

Por fim, aproveitamos um padrão de design de software denominado o padrão de injeção de dependência. Esse padrão permite programar em interfaces (abstrações) em vez de classes concretas. Implementando o padrão de design de injeção de dependência também torna nosso código mais testável. A próxima iteração, adicionamos testes de unidade ao nosso projeto.

> [!div class="step-by-step"]
> [Anterior](iteration-3-add-form-validation-vb.md)
> [Próximo](iteration-5-create-unit-tests-vb.md)
