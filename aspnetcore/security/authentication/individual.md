---
title: Artigos com base em projetos do ASP.NET Core criados com as contas de usuário individuais
author: rick-anderson
description: Descobrir artigos com base em projetos do ASP.NET Core criados com as contas de usuário individuais.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 6d05cd8c0ee6c9029eb9bbafc15d9b19ee7633de
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252016"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artigos com base em projetos do ASP.NET Core criados com as contas de usuário individuais

Identidade do ASP.NET Core está incluída nos modelos de projeto no Visual Studio com a opção de "Contas de usuário individuais".

Os modelos de autenticação estão disponíveis no .NET Core CLI com `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Os artigos a seguir mostram como usar o código gerado em modelos do ASP.NET Core que usam contas de usuário individuais:

* [Autenticação de dois fatores com SMS](xref:security/authentication/2fa)
* [Confirmação de conta e de recuperação de senha no ASP.NET Core](xref:security/authentication/accconfirm)
* [Criar um aplicativo do ASP.NET Core com dados de usuário protegidos por autorização](xref:security/authorization/secure-data)
