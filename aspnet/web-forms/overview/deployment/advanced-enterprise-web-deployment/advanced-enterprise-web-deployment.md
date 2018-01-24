---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: "Enterprise Web implantação avançada | Microsoft Docs"
author: jrjlee
description: "Este tutorial mostram como executar várias tarefas que são necessárias ou desejável em muitos cenários de implantação corporativa. Para um translati italiano..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: c3cb7f63cf7c0246a0c4da6038a65a6ac43a7b59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="advanced-enterprise-web-deployment"></a><span data-ttu-id="51b65-104">Implantação de Web Enterprise avançadas</span><span class="sxs-lookup"><span data-stu-id="51b65-104">Advanced Enterprise Web Deployment</span></span>
====================
<span data-ttu-id="51b65-105">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="51b65-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="51b65-106">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="51b65-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="51b65-107">Este tutorial mostram como executar várias tarefas que são necessárias ou desejável em muitos cenários de implantação corporativa.</span><span class="sxs-lookup"><span data-stu-id="51b65-107">This tutorial will show you how to perform various tasks that are required or desirable in a lot of enterprise deployment scenarios.</span></span>
> 
> <span data-ttu-id="51b65-108">Para obter uma tradução italiana destes tutoriais, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).</span><span class="sxs-lookup"><span data-stu-id="51b65-108">For an Italian translation of these tutorials, visit [http://www.lucamorelli.it](http://www.lucamorelli.it).</span></span>


<span data-ttu-id="51b65-109">Isso faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo & #x 2014; o [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) #x 2014; & solução para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, Windows Serviço do Communication Foundation (WCF) e um projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="51b65-109">This forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="51b65-110">O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), em que o processo de compilação é controlado por dois arquivos & #x 2014; projeto contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação.</span><span class="sxs-lookup"><span data-stu-id="51b65-110">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="51b65-111">No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.</span><span class="sxs-lookup"><span data-stu-id="51b65-111">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="51b65-112">Visão geral do cenário</span><span class="sxs-lookup"><span data-stu-id="51b65-112">Scenario Overview</span></span>

<span data-ttu-id="51b65-113">O cenário de alto nível para esses tutoriais é descrito em [implantação de Web corporativa: Visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-113">The high-level scenario for these tutorials is described in [Enterprise Web Deployment: Scenario Overview](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).</span></span> <span data-ttu-id="51b65-114">É recomendável que você examine este tópico antes de começar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="51b65-114">We recommend that you review this topic before you get started on this tutorial.</span></span>

## <a name="how-to-use-this-tutorial"></a><span data-ttu-id="51b65-115">Como usar esse tutorial</span><span class="sxs-lookup"><span data-stu-id="51b65-115">How to Use This Tutorial</span></span>

- <span data-ttu-id="51b65-116">Cada um dos tópicos neste tutorial é autossuficiente e corrige um problema que ocorre em cenários de implantação corporativa ou desafio em particular.</span><span class="sxs-lookup"><span data-stu-id="51b65-116">Each of the topics in this tutorial is self-contained and addresses a particular challenge or problem that occurs in enterprise deployment scenarios.</span></span> <span data-ttu-id="51b65-117">Você não precisa trabalhar com esses tópicos em nenhuma ordem específica.</span><span class="sxs-lookup"><span data-stu-id="51b65-117">You don't need to work through these topics in any particular order.</span></span> <span data-ttu-id="51b65-118">No entanto, este tutorial aborda algumas tarefas avançadas.</span><span class="sxs-lookup"><span data-stu-id="51b65-118">However, this tutorial covers some advanced tasks.</span></span> <span data-ttu-id="51b65-119">Como tal, você deve se familiarizar com os conceitos e técnicas que o [Web implantação na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) tutorial aborda para obter o máximo benefício deste conteúdo.</span><span class="sxs-lookup"><span data-stu-id="51b65-119">As such, you should familiarize yourself with the concepts and techniques that the [Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) tutorial covers in order to gain the most benefit from this content.</span></span>
- <span data-ttu-id="51b65-120">Este tutorial inclui os seguintes tópicos:</span><span class="sxs-lookup"><span data-stu-id="51b65-120">This tutorial includes these topics:</span></span>
- <span data-ttu-id="51b65-121">[Executando uma implantação "E se"](performing-a-what-if-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-121">[Performing a "What If" Deployment](performing-a-what-if-deployment.md).</span></span> <span data-ttu-id="51b65-122">Em muitos cenários, convém determinar o impacto de uma implantação proposto em um ambiente de destino ou todo o conteúdo existente antes de realmente fazer as alterações.</span><span class="sxs-lookup"><span data-stu-id="51b65-122">In a lot of scenarios, you'll want to determine the impact of a proposed deployment on a target environment or any existing content before you actually make any changes.</span></span> <span data-ttu-id="51b65-123">Este tópico descreve como você pode executar uma implantação "e se" para gerar arquivos de log e scripts de atualização de banco de dados como se for usado para implantar conteúdo em um ambiente de destino, sem fazer alterações.</span><span class="sxs-lookup"><span data-stu-id="51b65-123">This topic describes how you can run a "what if" deployment to generate log files and database update scripts as if you had deployed content to a target environment, without actually making any changes.</span></span> <span data-ttu-id="51b65-124">Analisar esses recursos pode ajudá-lo a identificar problemas potenciais com antecedência sobre uma implantação em tempo real.</span><span class="sxs-lookup"><span data-stu-id="51b65-124">Analyzing these resources can help you to spot any potential problems in advance of a live deployment.</span></span>
- <span data-ttu-id="51b65-125">[Personalizando as implantações de banco de dados para vários ambientes](customizing-database-deployments-for-multiple-environments.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-125">[Customizing Database Deployments for Multiple Environments](customizing-database-deployments-for-multiple-environments.md).</span></span> <span data-ttu-id="51b65-126">Quando você implanta um projeto de banco de dados para vários destinos, geralmente você desejará personalizar as propriedades de implantação para cada ambiente de destino.</span><span class="sxs-lookup"><span data-stu-id="51b65-126">When you deploy a database project to multiple destinations, you'll often want to customize the deployment properties for each target environment.</span></span> <span data-ttu-id="51b65-127">Por exemplo, em ambientes de teste você seria normalmente recriar o banco de dados em todas as implantações, enquanto em ambientes de produção ou preparo será muito mais provável que faça atualizações incrementais para preservar seus dados.</span><span class="sxs-lookup"><span data-stu-id="51b65-127">For example, in test environments you'd typically recreate the database on every deployment, whereas in staging or production environments you'd be a lot more likely to make incremental updates to preserve your data.</span></span> <span data-ttu-id="51b65-128">Este tópico descreve como você pode incorporar essas alterações de propriedade em sua lógica de implantação criando um arquivo de configuração (.sqldeployment) específico do ambiente de implantação para cada ambiente de destino.</span><span class="sxs-lookup"><span data-stu-id="51b65-128">This topic describes how you can incorporate these property changes into your deployment logic by creating an environment-specific deployment configuration (.sqldeployment) file for each target environment.</span></span>
- <span data-ttu-id="51b65-129">Implantando as associações de função de banco de dados em ambientes de teste.</span><span class="sxs-lookup"><span data-stu-id="51b65-129">Deploying Database Role Memberships to Test Environments.</span></span> <span data-ttu-id="51b65-130">Ao recriar um banco de dados em cada implantação & #x 2014; por exemplo, como parte de uma compilação de CI (integração contínua) e implantar como um ambiente de teste & #x 2014; normalmente você precisará configurar associações de função de banco de dados de cada vez.</span><span class="sxs-lookup"><span data-stu-id="51b65-130">When you recreate a database on every deployment&#x2014;for example, as part of a continuous integration (CI) build and deploy to a test environment&#x2014;you'll typically need to configure database role memberships every time.</span></span> <span data-ttu-id="51b65-131">Por exemplo, você geralmente precisará conceder permissões para a identidade do pool de aplicativos associado ao seu aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="51b65-131">For example, you'll usually need to grant permissions to the application pool identity associated with your web application.</span></span> <span data-ttu-id="51b65-132">Este tópico descreve como você pode automatizar esse processo, adicionando um script SQL de pós-implantação à sua lógica de implantação.</span><span class="sxs-lookup"><span data-stu-id="51b65-132">This topic describes how you can automate this process by adding a post-deployment SQL script to your deployment logic.</span></span>
- <span data-ttu-id="51b65-133">[Implantação de bancos de dados de associação em ambientes corporativos](deploying-membership-databases-to-enterprise-environments.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-133">[Deploying Membership Databases to Enterprise Environments](deploying-membership-databases-to-enterprise-environments.md).</span></span> <span data-ttu-id="51b65-134">Bancos de dados de associação ASP.NET têm várias características que podem complicar o processo de implantação.</span><span class="sxs-lookup"><span data-stu-id="51b65-134">ASP.NET membership databases have various characteristics that can complicate the deployment process.</span></span> <span data-ttu-id="51b65-135">Por exemplo, uma implantação somente de esquema deixará o banco de dados em um estado não-operacional.</span><span class="sxs-lookup"><span data-stu-id="51b65-135">For example, a schema-only deployment will leave the database in a non-operational state.</span></span> <span data-ttu-id="51b65-136">Na maioria dos cenários, é preferível para criar um banco de dados de associação diretamente em cada ambiente de destino.</span><span class="sxs-lookup"><span data-stu-id="51b65-136">In most scenarios, it's preferable to create a membership database directly in each destination environment.</span></span> <span data-ttu-id="51b65-137">No entanto, se você precisa implantar um banco de dados de associação, este tópico descreve algumas das abordagens que você pode usar para atender aos desafios inerentes.</span><span class="sxs-lookup"><span data-stu-id="51b65-137">However, if you do have to deploy a membership database, this topic describes some of the approaches you can use to meet the inherent challenges.</span></span>
- <span data-ttu-id="51b65-138">[Excluindo arquivos e pastas de implantação](excluding-files-and-folders-from-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-138">[Excluding Files and Folders from Deployment](excluding-files-and-folders-from-deployment.md).</span></span> <span data-ttu-id="51b65-139">Em alguns cenários, você desejará personalizar o conteúdo do pacote da web para ambientes de destino específico.</span><span class="sxs-lookup"><span data-stu-id="51b65-139">In some scenarios, you'll want to tailor the contents of your web package to specific destination environments.</span></span> <span data-ttu-id="51b65-140">Por exemplo, você talvez queira incluem versões completas de bibliotecas JavaScript ao implantar em um ambiente de teste, para oferecer suporte à depuração do lado do cliente, mas use minimizadas versões das bibliotecas de quando você implanta em um ambiente de preparo ou produção.</span><span class="sxs-lookup"><span data-stu-id="51b65-140">For example, you might want to include full versions of JavaScript libraries when you deploy to a test environment, to support client-side debugging, but use minified versions of the libraries when you deploy to a staging or production environment.</span></span> <span data-ttu-id="51b65-141">Este tópico descreve como você pode excluir arquivos e pastas específicos do processo de criação de pacote.</span><span class="sxs-lookup"><span data-stu-id="51b65-141">This topic describes how you can exclude specific files and folders from the package creation process.</span></span>
- <span data-ttu-id="51b65-142">[Implantar aplicativos de Web do colocando Offline com o Web](taking-web-applications-offline-with-web-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-142">[Taking Web Applications Offline with Web Deploy](taking-web-applications-offline-with-web-deploy.md).</span></span> <span data-ttu-id="51b65-143">Quando você implantar soluções em um ambiente de preparo ou produção, geralmente você desejará levar seus aplicativos web offline durante o processo de implantação.</span><span class="sxs-lookup"><span data-stu-id="51b65-143">When you deploy solutions to a staging or production environment, you'll often want to take your web applications offline for the duration of the deployment process.</span></span> <span data-ttu-id="51b65-144">Este tópico descreve como você pode adicionar uma *aplicativo\_offline.htm* o arquivo para seu aplicativo da web no início do processo de implantação e removê-lo no final.</span><span class="sxs-lookup"><span data-stu-id="51b65-144">This topic describes how you can add an *App\_offline.htm* file to your web application at the start of the deployment process and remove it at the end.</span></span> <span data-ttu-id="51b65-145">Enquanto o *aplicativo\_offline.htm* arquivo está em vigor, qualquer usuário que acesse o aplicativo da web sejam redirecionado automaticamente para o *aplicativo\_offline.htm* arquivo.</span><span class="sxs-lookup"><span data-stu-id="51b65-145">While the *App\_offline.htm* file is in place, any users who browse to the web application are automatically redirected to the *App\_offline.htm* file.</span></span>
- <span data-ttu-id="51b65-146">[Executando Scripts do Windows PowerShell do MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-146">[Running Windows PowerShell Scripts from MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md).</span></span> <span data-ttu-id="51b65-147">Muitos cenários de implantação exigem ações de pós-implantação mais complexas, como adicionar fontes de evento personalizado para o registro ou configurar a replicação entre instâncias do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="51b65-147">Many deployment scenarios require more complex post-deployment actions, like adding custom event sources to the registry or configuring replication between SQL Server instances.</span></span> <span data-ttu-id="51b65-148">Geralmente, essas ações são realizadas por meio de scripts do Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="51b65-148">These actions are often accomplished through Windows PowerShell scripts.</span></span> <span data-ttu-id="51b65-149">Este tópico descreve como executar scripts do Windows PowerShell de um arquivo de projeto do Microsoft Build Engine (MSBuild) como parte do processo de compilação e implantação.</span><span class="sxs-lookup"><span data-stu-id="51b65-149">This topic describes how to run Windows PowerShell scripts from a Microsoft Build Engine (MSBuild) project file as part of the build and deployment process.</span></span>
- <span data-ttu-id="51b65-150">[O processo de empacotamento de solução de problemas](troubleshooting-the-packaging-process.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-150">[Troubleshooting the Packaging Process](troubleshooting-the-packaging-process.md).</span></span> <span data-ttu-id="51b65-151">O Pipeline de publicação de Web (WPP) define uma propriedade de MSBuild chamada **EnablePackageProcessLoggingAndAssert** que você pode usar para gerar informações detalhadas sobre o processo de empacotamento para projetos de aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="51b65-151">The Web Publishing Pipeline (WPP) defines an MSBuild property named **EnablePackageProcessLoggingAndAssert** that you can use to generate in-depth information about the packaging process for web application projects.</span></span> <span data-ttu-id="51b65-152">Este tópico descreve o que a propriedade faz e como usá-lo.</span><span class="sxs-lookup"><span data-stu-id="51b65-152">This topic describes what the property does and how to use it.</span></span>

## <a name="key-technologies"></a><span data-ttu-id="51b65-153">Tecnologias de chave</span><span class="sxs-lookup"><span data-stu-id="51b65-153">Key Technologies</span></span>

<span data-ttu-id="51b65-154">Este tutorial se concentra em como usar esses produtos e tecnologias para dar suporte a compilação automatizada e implantação da web:</span><span class="sxs-lookup"><span data-stu-id="51b65-154">This tutorial focuses on how to use these products and technologies to support automated build and web deployment:</span></span>

- <span data-ttu-id="51b65-155">Visual Studio 2010 e o Team Foundation Server (TFS) 2010</span><span class="sxs-lookup"><span data-stu-id="51b65-155">Visual Studio 2010 and Team Foundation Server (TFS) 2010</span></span>
- <span data-ttu-id="51b65-156">MSBuild e compilação de equipe do TFS</span><span class="sxs-lookup"><span data-stu-id="51b65-156">MSBuild and TFS Team Build</span></span>
- <span data-ttu-id="51b65-157">Serviços de informações da Internet (IIS) 7.5</span><span class="sxs-lookup"><span data-stu-id="51b65-157">Internet Information Services (IIS) 7.5</span></span>
- <span data-ttu-id="51b65-158">Ferramenta de implantação da Web do IIS (implantação da Web) 2.1</span><span class="sxs-lookup"><span data-stu-id="51b65-158">IIS Web Deployment Tool (Web Deploy) 2.1</span></span>
- <span data-ttu-id="51b65-159">O utilitário de implantação de banco de dados VSDBCMD.exe</span><span class="sxs-lookup"><span data-stu-id="51b65-159">The VSDBCMD.exe database deployment utility</span></span>

## <a name="other-tutorials-in-this-series"></a><span data-ttu-id="51b65-160">Outros tutoriais nesta série</span><span class="sxs-lookup"><span data-stu-id="51b65-160">Other Tutorials in This Series</span></span>

<span data-ttu-id="51b65-161">Isso faz parte de uma série de cinco tutoriais na implantação da web de grande porte.</span><span class="sxs-lookup"><span data-stu-id="51b65-161">This forms part of a series of five tutorials on enterprise-scale web deployment.</span></span> <span data-ttu-id="51b65-162">Estes são os outros tutoriais na série:</span><span class="sxs-lookup"><span data-stu-id="51b65-162">These are other tutorials in the series:</span></span>

- <span data-ttu-id="51b65-163">[Implantando aplicativos da Web em cenários corporativos](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-163">[Deploying Web Applications in Enterprise Scenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span></span> <span data-ttu-id="51b65-164">Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais.</span><span class="sxs-lookup"><span data-stu-id="51b65-164">This introductory content provides the contextual background for the tutorial series.</span></span> <span data-ttu-id="51b65-165">Descreve o cenário do tutorial e ilustra como as tarefas e descrita em toda a série explicações passo a passo se encaixam um processo de gerenciamento de ciclo de vida do aplicativo (ALM) mais amplo.</span><span class="sxs-lookup"><span data-stu-id="51b65-165">It describes the tutorial scenario, and it illustrates how the tasks and walkthroughs described throughout the series fit into a broader Application Lifecycle Management (ALM) process.</span></span>
- <span data-ttu-id="51b65-166">[Implantação na empresa Web](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-166">[Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span></span> <span data-ttu-id="51b65-167">Este tutorial fornece uma introdução conceitual para arquivos de projeto do MSBuild, o WPP, Web Deploy e outras tecnologias relacionadas.</span><span class="sxs-lookup"><span data-stu-id="51b65-167">This tutorial provides a conceptual introduction to MSBuild project files, the WPP, Web Deploy, and other related technologies.</span></span> <span data-ttu-id="51b65-168">Ele explica como você pode usar essas ferramentas em conjunto para gerenciar processos complexos de implantação.</span><span class="sxs-lookup"><span data-stu-id="51b65-168">It explains how you can use these tools together to manage complex deployment processes.</span></span>
- <span data-ttu-id="51b65-169">[Configurando ambientes de servidor para a implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-169">[Configuring Server Environments for Web Deployment](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).</span></span> <span data-ttu-id="51b65-170">Este tutorial descreve como configurar os servidores Windows para oferecer suporte a vários cenários de implantação, incluindo a implantação de pacote via web remoto usando o serviço de agente de implantação da Web (o agente remoto) ou o manipulador de implantação da Web e implantação de banco de dados remoto.</span><span class="sxs-lookup"><span data-stu-id="51b65-170">This tutorial describes how to configure Windows servers to support various deployment scenarios, including remote web package deployment using the Web Deployment Agent Service (the remote agent) or the Web Deploy Handler and remote database deployment.</span></span> <span data-ttu-id="51b65-171">Fornece orientação sobre como escolher o método de implantação apropriadas para seu próprio ambiente, e descreve como usar o Web Farm Framework (WFF) para replicar os aplicativos web implantados em todos os servidores web em um farm de servidores.</span><span class="sxs-lookup"><span data-stu-id="51b65-171">It provides guidance on choosing the appropriate deployment method for your own environment, and it describes how to use the Web Farm Framework (WFF) to replicate deployed web applications across all the web servers in a server farm.</span></span>
- <span data-ttu-id="51b65-172">[Configurando o Team Foundation Server para a implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="51b65-172">[Configuring Team Foundation Server for Web Deployment](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).</span></span> <span data-ttu-id="51b65-173">Este tutorial descreve como configurar o TFS para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI e disparada manualmente as implantações de compilações específicas.</span><span class="sxs-lookup"><span data-stu-id="51b65-173">This tutorial describes how to configure TFS to support various deployment scenarios, including automated deployment as part of a CI process and manually triggered deployments of specific builds.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="51b65-174">Avançar</span><span class="sxs-lookup"><span data-stu-id="51b65-174">Next</span></span>](performing-a-what-if-deployment.md)