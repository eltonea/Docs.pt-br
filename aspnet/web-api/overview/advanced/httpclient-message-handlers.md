---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Manipuladores de mensagens de HttpClient na API da Web ASP.NET | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Manipuladores de mensagens de HttpClient na API da Web ASP.NET
====================
por [Mike Wasson](https://github.com/MikeWasson)

Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta HTTP.

Normalmente, uma série de manipuladores de mensagens são encadeados juntos. O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o manipulador de Avançar. Em algum momento, a resposta é criada e retorna a cadeia. Esse padrão é chamado um *delegando* manipulador.

![](httpclient-message-handlers/_static/image1.png)

No lado do cliente, o **HttpClient** classe usa um manipulador de mensagens para processar solicitações. O manipulador padrão é **HttpClientHandler**, que envia a solicitação através da rede e obtém a resposta do servidor. Você pode inserir os manipuladores de mensagens personalizadas no pipeline de cliente:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API também utiliza manipuladores de mensagens no lado do servidor. Para obter mais informações, consulte [manipuladores de mensagens HTTP](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Manipuladores de mensagens personalizadas

Para escrever um manipulador de mensagens personalizadas, derivam **System.Net.Http.DelegatingHandler** e substituir o **SendAsync** método. Aqui está a assinatura do método:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

O método usa um **HttpRequestMessage** como entrada e retorna de maneira assíncrona um **HttpResponseMessage**. Uma implementação típica faz o seguinte:

1. Processe a mensagem de solicitação.
2. Chamar `base.SendAsync` para enviar a solicitação para o manipulador interno.
3. O manipulador interno retorna uma mensagem de resposta. (Esta etapa é assíncrona).
4. Processar a resposta e retorná-lo ao chamador.

O exemplo a seguir mostra um manipulador de mensagens que adiciona um cabeçalho personalizado para a solicitação de saída:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

A chamada para `base.SendAsync` é assíncrona. Se o manipulador faz qualquer trabalho após essa chamada, use o **await** palavra-chave para retomar a execução depois que o método é concluído. O exemplo a seguir mostra um manipulador que registra os códigos de erro. O registro em log em si não é muito interessante, mas o exemplo mostra como obter a resposta dentro do manipulador.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Adicionando manipuladores de mensagens para o Pipeline de cliente

Para adicionar manipuladores personalizados para **HttpClient**, use o **HttpClientFactory.Create** método:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Manipuladores de mensagens são chamados na ordem em que você os transmite para o **criar** método. Porque manipuladores são aninhados, a mensagem de resposta são transferidos na outra direção. Ou seja, o último manipulador é a primeira a receber a mensagem de resposta.
