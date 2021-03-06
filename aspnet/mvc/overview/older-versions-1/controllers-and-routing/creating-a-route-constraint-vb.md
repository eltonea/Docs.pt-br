---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Criar uma restrição de rota (VB) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita rotas de correspondência, criando restrições da rota com expressões regulares.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f50b371ac679218b06c4848e6d33516d29d3a82
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-route-constraint-vb"></a>Criar uma restrição de rota (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita rotas de correspondência, criando restrições da rota com expressões regulares.


Você pode usar restrições da rota para restringir as solicitações do navegador que correspondam a uma rota específica. Você pode usar uma expressão regular para especificar uma restrição de rota.

Por exemplo, imagine que você definiu a rota na listagem 1 no arquivo global. asax.

**Listando 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Listar 1 contém uma rota denominada produto. Você pode usar a rota de produto para mapear solicitações do navegador para o ProductController contido na listagem 2.

**A listagem 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Observe que a ação de Details() exposta pelo controlador de produto aceita um único parâmetro nomeado productId. Esse parâmetro é um parâmetro de número inteiro.

A rota definida na listagem 1 corresponderá a qualquer uma das seguintes URLs:

- / Produto/23
- /Product/7

Infelizmente, a rota também serão compatíveis com as seguintes URLs:

- / Produtos/blah
- / Produtos/apple

Porque a ação Details() espera um parâmetro de número inteiro, fazer uma solicitação que contém algo diferente de um valor inteiro causará um erro. Por exemplo, se você digitar a URL /Product/apple em seu navegador, em seguida, você obterá a página de erro na Figura 1.


[![A caixa de diálogo Novo projeto](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Figura 01**: vendo uma página Detalhar ([clique para exibir a imagem em tamanho normal](creating-a-route-constraint-vb/_static/image2.png))


O que você realmente deseja fazer é corresponder somente URLs que contêm um número inteiro apropriado productId. Você pode usar uma restrição ao definir uma rota para restringir as URLs que correspondem à rota. A rota de produto modificada na listagem 3 contém uma restrição de expressão regular que corresponde apenas números inteiros.

**Listing 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

\D+ a expressão regular corresponde a um ou mais inteiros. Essa restrição faz com que a rota de produto coincidir com as seguintes URLs:

- / Produto/3
- /Product/8999

Mas não as seguintes URLs:

- / Produtos/apple
- / Produto

Essas solicitações do navegador serão tratadas por outra rota ou, se não houver nenhuma rota correspondente, um *não foi possível encontrar o recurso* erro será retornado.

> [!div class="step-by-step"]
> [Anterior](creating-custom-routes-vb.md)
> [Próximo](creating-a-custom-route-constraint-vb.md)
