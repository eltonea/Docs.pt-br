---
title: Trabalhar com um cache distribuído no núcleo do ASP.NET
author: ardalis
description: Saiba como usar o ASP.NET Core distribuída cache para melhorar o desempenho do aplicativo e a escalabilidade, especialmente em um ambiente de farm de servidor ou de nuvem.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: 6c595572641604d241c0c8f702d4f392afe34f71
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734452"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a>Trabalhar com um cache distribuído no núcleo do ASP.NET

Por [Steve Smith](https://ardalis.com/)

Os caches distribuídos podem melhorar o desempenho e escalabilidade de aplicativos do ASP.NET Core, especialmente quando hospedados em um ambiente de farm de servidor ou de nuvem. Este artigo explica como trabalhar com implementações e abstrações de cache distribuído internos do ASP.NET Core.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-a-distributed-cache"></a>O que é um cache distribuído

Um cache distribuído é compartilhado por vários servidores de aplicativo (consulte [Noções básicas de Cache](memory.md#caching-basics)). As informações em cache não são armazenadas na memória dos servidores web individuais, e os dados armazenados em cache estão disponíveis para todos os servidores do aplicativo. Isso oferece várias vantagens:

1. Dados armazenados em cache são coerentes em todos os servidores web. Os usuários não ver resultados diferentes dependendo de qual web server manipula a solicitação

2. Dados armazenados em cache sobrevive reinicializações do servidor web e implantações. Servidores web individuais podem ser removidas ou adicionadas sem afetar o cache.

3. O repositório de dados de origem tem menos solicitações feitas a ele (de cache em todos os com vários caches de memória ou não).

> [!NOTE]
> Se usar um Cache distribuído do SQL Server, algumas dessas vantagens são somente verdadeiras se uma instância separada do banco de dados é usada para o cache de dados de origem do aplicativo.

Como qualquer cache, um cache distribuído pode melhorar drasticamente capacidade de resposta do aplicativo, como normalmente os dados podem ser recuperados do cache muito mais rápido do que de um banco de dados relacional (ou serviço da web).

Configuração de cache é específico da implementação. Este artigo descreve como configurar ambos Redis e distribuídas do SQL Server armazena em cache. Independentemente de qual implementação for selecionada, o aplicativo interage com o cache usando uma comum `IDistributedCache` interface.

## <a name="the-idistributedcache-interface"></a>A Interface IDistributedCache

O `IDistributedCache` interface inclui métodos síncronos e assíncronos. A interface permite que itens sejam adicionados, recuperar e removido da implementação de cache distribuído. O `IDistributedCache` interface inclui os seguintes métodos:

**Get, GetAsync**

Usa uma chave de cadeia de caracteres e recupera um item em cache como um `byte[]` se encontrado no cache.

**Set, SetAsync**

Adiciona um item (como `byte[]`) para o cache usando uma chave de cadeia de caracteres.

**Atualização de RefreshAsync**

Atualiza um item em cache com base em sua chave, redefinir seu tempo limite de expiração deslizante (se houver).

**Remove, RemoveAsync**

Remove uma entrada de cache com base em sua chave.

Para usar o `IDistributedCache` interface:

   1. Adicione os pacotes do NuGet necessários para o arquivo de projeto.

   2. Configurar a implementação específica de `IDistributedCache` no seu `Startup` da classe `ConfigureServices` método e adicione-o para o contêiner existe.

   3. O aplicativo [Middleware](xref:fundamentals/middleware/index) ou classes do controlador MVC, solicite uma instância do `IDistributedCache` a partir do construtor. A instância será fornecida pelo [injeção de dependência](../../fundamentals/dependency-injection.md) (DI).

> [!NOTE]
> Não é necessário usar um tempo de vida Singleton ou escopo para `IDistributedCache` instâncias (pelo menos para as implementações internas). Você também pode criar uma instância sempre que talvez seja necessário um (em vez de usar [injeção de dependência](../../fundamentals/dependency-injection.md)), mas isso pode tornar seu código mais difícil de teste e viola o [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/).

O exemplo a seguir mostra como usar uma instância de `IDistributedCache` em um componente de middleware simples:

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

No código acima, o valor armazenado em cache é de leitura, mas nunca foi gravado. Neste exemplo, o valor é definido apenas quando um servidor é inicializado e não é alterado. Em um cenário de vários servidores, o servidor mais recente para iniciar substituirá quaisquer valores anteriores que foram definidas por outros servidores. O `Get` e `Set` métodos usam o `byte[]` tipo. Portanto, o valor de cadeia de caracteres deve ser convertido usando `Encoding.UTF8.GetString` (para `Get`) e `Encoding.UTF8.GetBytes` (para `Set`).

O código a seguir de *Startup.cs* mostra o valor que está sendo definido:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> Como `IDistributedCache` é configurado no `ConfigureServices` método, ele está disponível para o `Configure` método como um parâmetro. Adicioná-lo como um parâmetro permitirá que a instância configurada ser fornecido por meio de injeção de dependência.

## <a name="using-a-redis-distributed-cache"></a>Usando um cache Redis distribuído

[Redis](https://redis.io/) é um repositório de dados na memória do código-fonte aberto, que geralmente é usado como um cache distribuído. Você pode usá-lo localmente, e você pode configurar um [Cache Redis do Azure](https://azure.microsoft.com/services/cache/) para seus aplicativos hospedados do Azure ASP.NET Core. Seu aplicativo ASP.NET Core configura a implementação de cache usando uma `RedisDistributedCache` instância.

Configurar a implementação do Redis em `ConfigureServices` e acessá-lo no código do aplicativo solicitando uma instância de `IDistributedCache` (consulte o código acima).

No código de exemplo, um `RedisCache` implementação é usada quando o servidor está configurado para um `Staging` ambiente. Assim, o `ConfigureStagingServices` método configura o `RedisCache`:

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> Para instalar o Redis em seu computador local, instale o pacote chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) e executar `redis-server` em um prompt de comando.

## <a name="using-a-sql-server-distributed-cache"></a>Usando um SQL Server de cache distribuído

A implementação de SqlServerCache permite que o cache distribuído usar um banco de dados do SQL Server como seu armazenamento de backup. Para SQL Server de criar tabela, você pode usar a ferramenta de cache de sql, a ferramenta cria uma tabela com o nome e o esquema que você especificar.

::: moniker range="< aspnetcore-2.1"

Adicionar `SqlConfig.Tools` para o `<ItemGroup>` elemento do arquivo de projeto e executar `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Teste SqlConfig.Tools executando o seguinte comando:

```console
dotnet sql-cache create --help
```

SqlConfig.Tools exibe a Ajuda do comando de uso e opções.

Criar uma tabela no SQL Server executando o `sql-cache create` comando:

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

A tabela criada tem o esquema a seguir:

![Tabela de Cache do SQL Server](distributed/_static/SqlServerCacheTable.png)

Como todas as implementações de cache, seu aplicativo deve obter e definir valores de cache usando uma instância de `IDistributedCache`, não um `SqlServerCache`. O exemplo implementa `SqlServerCache` no ambiente de produção (para que ele é configurado em `ConfigureProductionServices`).

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> O `ConnectionString` (e, opcionalmente, `SchemaName` e `TableName`) normalmente devem ser armazenadas fora do controle de origem (como UserSecrets), pois eles podem conter credenciais.

## <a name="recommendations"></a>Recomendações

Ao decidir qual implementação de `IDistributedCache` é ideal para seu aplicativo, escolha entre Redis e do SQL Server com base em sua infraestrutura existente e ambiente, seus requisitos de desempenho e experiência da sua equipe. Se sua equipe mais confortável com Redis, é uma excelente opção. Se sua equipe prefere SQL Server, você pode ter certeza de que implementação também. Observe que uma solução tradicional de cache armazena dados na memória que permite a rápida recuperação de dados. Você deve armazenar dados usados em um cache e armazenar todos os dados em um armazenamento persistente de back-end, como o SQL Server ou do armazenamento do Azure. Cache redis é uma solução de cache que oferece alta taxa de transferência e baixa latência em comparação com o Cache de SQL.

## <a name="additional-resources"></a>Recursos adicionais

* [Redis Cache no Azure](https://azure.microsoft.com/documentation/services/redis-cache/)
* [Banco de dados do SQL Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [Cache na memória](xref:performance/caching/memory)
* [Detectar alterações com tokens de alteração](xref:fundamentals/primitives/change-tokens)
* [Cache de resposta](xref:performance/caching/response)
* [Middleware de Cache de Resposta](xref:performance/caching/middleware)
* [Auxiliar de marca de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Auxiliar de marca de cache distribuído](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
