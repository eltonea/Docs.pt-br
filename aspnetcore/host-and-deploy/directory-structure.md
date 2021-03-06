---
title: Estrutura do diretório do ASP.NET Core
author: guardrex
description: Saiba mais sobre a estrutura do diretório de aplicativos publicados do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: a5cc1f23d624643facddc9e2006fb246e5ae66dc
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838430"
---
# <a name="aspnet-core-directory-structure"></a>Estrutura do diretório do ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

No ASP.NET Core, o diretório de aplicativo publicado, *publicar*, é composto de arquivos de aplicativo, arquivos de configuração, ativos estáticos, pacotes e o tempo de execução (para [implantações autossuficientes](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Tipo de aplicativo | Estrutura de diretórios |
| -------- | ------------------- |
| [Implantação dependente de estrutura](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>logs&dagger; (opcional, a menos que necessário para receber logs de stdout)</li><li>Exibições&dagger; (aplicativos MVC; se as exibições não são pré-compiladas)</li><li>Páginas&dagger; (aplicativos de Páginas do Razor ou MVC, se as páginas não são pré-compiladas)</li><li>wwwroot&dagger;</li><li>arquivos *\.dll</li><li>\<nome-do-assembly>.deps.json</li><li>\<nome-do-assembly>.dll</li><li>\<nome-do-assembly>.pdb</li><li>\<nome-do-assembly>.PrecompiledViews.dll</li><li>\<nome-do-assembly>.PrecompiledViews.pdb</li><li>\<nome-do-assembly>.runtimeconfig.json</li><li>web.config (implantações do IIS)</li></ul></li></ul> |
| [Implantação autossuficiente](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>logs&dagger; (opcional, a menos que necessário para receber logs de stdout)</li><li>refs&dagger;</li><li>Exibições&dagger; (aplicativos MVC; se as exibições não são pré-compiladas)</li><li>Páginas&dagger; (aplicativos de Páginas do Razor ou MVC, se as páginas não são pré-compiladas)</li><li>wwwroot&dagger;</li><li>arquivos \*.dll</li><li>\<nome-do-assembly>.deps.json</li><li>\<nome-do-assembly>.exe</li><li>\<nome-do-assembly>.pdb</li><li>\<nome-do-assembly>.PrecompiledViews.dll</li><li>\<nome-do-assembly>.PrecompiledViews.pdb</li><li>\<nome-do-assembly>.runtimeconfig.json</li><li>web.config (implantações do IIS)</li></ul></li></ul> |

&dagger;Indica um diretório

O diretório *publish* representa o *caminho raiz de conteúdo* (também chamado de *caminho base do aplicativo*) da implantação. Qualquer que seja o nome fornecido para o diretório *publish* do aplicativo implantado no servidor, o local dele serve como o caminho físico do servidor para o aplicativo hospedado.

O diretório *wwwroot*, se presente, contém somente ativos estáticos.

O diretório *logs* de stdout pode ser criado para a implantação usando uma das duas abordagens a seguir:

* Adicione o seguinte elemento `<Target>` ao arquivo de projeto:

   ```xml
   <Target Name="CreateLogsFolder" AfterTargets="Publish">
     <MakeDir Directories="$(PublishDir)Logs" 
              Condition="!Exists('$(PublishDir)Logs')" />
     <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                       Lines="Generated file" 
                       Overwrite="True" 
                       Condition="!Exists('$(PublishDir)Logs\.log')" />
   </Target>
   ```

   O elemento `<MakeDir>` cria uma pasta *Logs* vazia na saída publicada. O elemento usa a propriedade `PublishDir` para determinar o local de destino no qual criar a pasta. Vários métodos de implantação, como a Implantação da Web, ignoram pastas vazias durante a implantação. O elemento `<WriteLinesToFile>` gera um arquivo na pasta *Logs*, o que garante a implantação da pasta no servidor. Observe que a criação de pasta ainda pode falhar se o processo de trabalho não tem acesso de gravação para a pasta de destino.

* Crie fisicamente o diretório *Logs* no servidor na implantação.

O diretório de implantação requer permissões de leitura/execução. O diretório *Logs* requer permissões de leitura/gravação. Diretórios adicionais em que os arquivos são gravados exigem permissões de leitura/gravação.
