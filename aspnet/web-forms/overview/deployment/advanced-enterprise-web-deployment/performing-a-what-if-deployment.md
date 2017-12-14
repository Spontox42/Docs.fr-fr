---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: "Effectuer le déploiement | Documents Microsoft"
author: jrjlee
description: "Cette rubrique explique comment effectuer « que se passe-t-il si » (ou simulé) déploiements à l’aide de l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy) et V..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 62be7c9636fb74c40bec812e9ac76b360995da50
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="performing-a-what-if-deployment"></a><span data-ttu-id="a112c-103">Exécution d’un déploiement « Que se passe-t-il si »</span><span class="sxs-lookup"><span data-stu-id="a112c-103">Performing a "What If" Deployment</span></span>
====================
<span data-ttu-id="a112c-104">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="a112c-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="a112c-105">Télécharger le PDF</span><span class="sxs-lookup"><span data-stu-id="a112c-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="a112c-106">Cette rubrique explique comment effectuer « que se passe-t-il si » (ou simulé) déploiements à l’aide de l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy) et VSDBCMD.</span><span class="sxs-lookup"><span data-stu-id="a112c-106">This topic describes how to perform "what if" (or simulated) deployments using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) and VSDBCMD.</span></span> <span data-ttu-id="a112c-107">Cela vous permet de déterminer l’impact de votre logique de déploiement sur un environnement cible particulier avant de déployer votre application.</span><span class="sxs-lookup"><span data-stu-id="a112c-107">This lets you determine the effects of your deployment logic on a particular target environment before you actually deploy your application.</span></span>


<span data-ttu-id="a112c-108">Cette rubrique fait partie d’une série de didacticiels basées sur les spécifications de déploiement d’entreprise d’une société fictive nommée Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution l’a & #x 2014 ; le [solution Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014 ; pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, Windows Service de communication Foundation (WCF) et un projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="a112c-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="a112c-109">La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet de fractionnement décrite dans [présentation du fichier de projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération et de déploiement est contrôlé par deux fichiers projet & #x 2014 ; o ne contenant les instructions de génération qui s’appliquent à chaque environnement de destination et celui qui contient les paramètres de génération et de déploiement spécifiques à l’environnement.</span><span class="sxs-lookup"><span data-stu-id="a112c-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build and deployment process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="a112c-110">Au moment de la génération, le fichier de projet spécifique à un environnement est fusionné dans le fichier de projet d’indépendant de l’environnement pour former un ensemble complet d’instructions de génération.</span><span class="sxs-lookup"><span data-stu-id="a112c-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="performing-a-what-if-deployment-for-web-packages"></a><span data-ttu-id="a112c-111">Exécution d’un déploiement « Que se passe-t-il si » pour les Packages Web</span><span class="sxs-lookup"><span data-stu-id="a112c-111">Performing a "What If" Deployment for Web Packages</span></span>

<span data-ttu-id="a112c-112">Web Deploy inclut des fonctionnalités qui vous permet d’effectuer des déploiements dans « que se passe-t-il si » (ou une version d’évaluation) mode.</span><span class="sxs-lookup"><span data-stu-id="a112c-112">Web Deploy includes functionality that lets you perform deployments in "what if" (or trial) mode.</span></span> <span data-ttu-id="a112c-113">Lorsque vous déployez des artefacts en mode « que se passe-t-il si », Web Deploy génère un fichier journal, comme si vous avez effectué le déploiement, mais il réellement ne modifie en rien sur le serveur de destination.</span><span class="sxs-lookup"><span data-stu-id="a112c-113">When you deploy artifacts in "what if" mode, Web Deploy generates a log file as if you had performed the deployment, but it doesn't actually change anything on the destination server.</span></span> <span data-ttu-id="a112c-114">Examiner le fichier journal peut vous aider à comprendre les incidences de votre déploiement aura sur le serveur de destination, en particulier :</span><span class="sxs-lookup"><span data-stu-id="a112c-114">Reviewing the log file can help you to understand what impact your deployment will have on the destination server, in particular:</span></span>

- <span data-ttu-id="a112c-115">Ce qui est ajouté.</span><span class="sxs-lookup"><span data-stu-id="a112c-115">What will get added.</span></span>
- <span data-ttu-id="a112c-116">Ce qui sera mis à jour.</span><span class="sxs-lookup"><span data-stu-id="a112c-116">What will get updated.</span></span>
- <span data-ttu-id="a112c-117">Ce qui sera supprimé.</span><span class="sxs-lookup"><span data-stu-id="a112c-117">What will get deleted.</span></span>

<span data-ttu-id="a112c-118">Car un déploiement « que se passe-t-il si » réellement ne modifie en rien sur le serveur de destination, ce qu’il ne peut pas toujours faire est de prédire si un déploiement réussira.</span><span class="sxs-lookup"><span data-stu-id="a112c-118">Because a "what if" deployment doesn't actually change anything on the destination server, what it can't always do is predict whether a deployment will succeed.</span></span>

<span data-ttu-id="a112c-119">Comme décrit dans [déploiement des Packages Web](../web-deployment-in-the-enterprise/deploying-web-packages.md), vous pouvez déployer des packages web à l’aide de Web Deploy dans deux façons & #x 2014 ; à l’aide de l’utilitaire de ligne de commande MSDeploy.exe directement ou en exécutant la *. deploy.cmd* fichier qui génère des processus de génération.</span><span class="sxs-lookup"><span data-stu-id="a112c-119">As described in [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md), you can deploy web packages using Web Deploy in two ways&#x2014;by using the MSDeploy.exe command-line utility directly or by running the *.deploy.cmd* file that the build process generates.</span></span>

<span data-ttu-id="a112c-120">Si vous utilisez MSDeploy.exe directement, vous pouvez exécuter un déploiement « que se passe-t-il si » en ajoutant le **– whatif** indicateur à votre commande.</span><span class="sxs-lookup"><span data-stu-id="a112c-120">If you're using MSDeploy.exe directly, you can run a "what if" deployment by adding the **–whatif** flag to your command.</span></span> <span data-ttu-id="a112c-121">Par exemple, pour évaluer ce qui se passerait si vous avez déployé le package ContactManager.Mvc.zip dans un environnement intermédiaire, la commande MSDeploy doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="a112c-121">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package to a staging environment, the MSDeploy command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


<span data-ttu-id="a112c-122">Lorsque vous êtes satisfait des résultats de votre déploiement « que se passe-t-il si », vous pouvez supprimer la **– whatif** indicateur pour exécuter un déploiement.</span><span class="sxs-lookup"><span data-stu-id="a112c-122">When you're satisfied with the results of your "what if" deployment, you can remove the **–whatif** flag to run a live deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="a112c-123">Pour plus d’informations sur les options de ligne de commande pour MSDeploy.exe, consultez [Web Deploy opération Settings](https://technet.microsoft.com/en-us/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="a112c-123">For more information on command-line options for MSDeploy.exe, see [Web Deploy Operation Settings](https://technet.microsoft.com/en-us/library/dd569089(WS.10).aspx).</span></span>


<span data-ttu-id="a112c-124">Si vous utilisez la *. deploy.cmd* fichier, vous pouvez exécuter un déploiement « que se passe-t-il si » en incluant le **/t** drapeau de (mode d’évaluation) au lieu du **/y** indicateur (« Oui », ou en mode de mise à jour) dans votre commande.</span><span class="sxs-lookup"><span data-stu-id="a112c-124">If you're using the *.deploy.cmd* file, you can run a "what if" deployment by including the **/t** flag (trial mode) flag instead of the **/y** flag ("yes," or update mode) in your command.</span></span> <span data-ttu-id="a112c-125">Par exemple, pour évaluer ce qui se passerait si vous avez déployé le package ContactManager.Mvc.zip en exécutant la *. deploy.cmd* fichier, votre commande doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="a112c-125">For example, to evaluate what would happen if you deployed the ContactManager.Mvc.zip package by running the *.deploy.cmd* file, your command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


<span data-ttu-id="a112c-126">Lorsque vous êtes satisfait des résultats de votre déploiement de « mode d’évaluation », vous pouvez remplacer le **/t** indicateur avec un **/y** indicateur pour l’exécution d’un déploiement en direct :</span><span class="sxs-lookup"><span data-stu-id="a112c-126">When you're satisfied with the results of your "trial mode" deployment, you can replace the **/t** flag with a **/y** flag to run a live deployment:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="a112c-127">Pour plus d’informations sur les options de ligne de commande pour *. deploy.cmd* fichiers, voir [Comment : installer un Package de déploiement à l’aide du fichier deploy.cmd](https://msdn.microsoft.com/en-us/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="a112c-127">For more information on command-line options for *.deploy.cmd* files, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/en-us/library/ff356104.aspx).</span></span> <span data-ttu-id="a112c-128">Si vous exécutez le *. deploy.cmd* fichier sans spécifier de tous les indicateurs, l’invite de commandes affiche une liste des indicateurs disponibles.</span><span class="sxs-lookup"><span data-stu-id="a112c-128">If you run the *.deploy.cmd* file without specifying any flags, the command prompt will display a list of available flags.</span></span>


## <a name="performing-a-what-if-deployment-for-databases"></a><span data-ttu-id="a112c-129">Exécution d’un déploiement « Que se passe-t-il si » pour les bases de données</span><span class="sxs-lookup"><span data-stu-id="a112c-129">Performing a "What If" Deployment for Databases</span></span>

<span data-ttu-id="a112c-130">Cette section part du principe que vous utilisez l’utilitaire VSDBCMD pour effectuer le déploiement incrémentiel, basée sur un schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="a112c-130">This section assumes that you're using the VSDBCMD utility to perform incremental, schema-based database deployment.</span></span> <span data-ttu-id="a112c-131">Cette approche est décrite plus en détail dans [déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span><span class="sxs-lookup"><span data-stu-id="a112c-131">This approach is described in more detail in [Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md).</span></span> <span data-ttu-id="a112c-132">Nous vous recommandons de vous familiariser avec cette rubrique avant d’appliquer les concepts décrits ici.</span><span class="sxs-lookup"><span data-stu-id="a112c-132">We recommend that you familiarize yourself with this topic before you apply the concepts described here.</span></span>

<span data-ttu-id="a112c-133">Lorsque vous utilisez VSDBCMD dans **déployer** mode, vous pouvez utiliser la **/dd** (ou **/DeployToDatabase**) indicateur contrôle VSDBCMD réellement déploie la base de données ou génère simplement une script de déploiement.</span><span class="sxs-lookup"><span data-stu-id="a112c-133">When you use VSDBCMD in **Deploy** mode, you can use the **/dd** (or **/DeployToDatabase**) flag to control whether VSDBCMD actually deploys the database or just generates a deployment script.</span></span> <span data-ttu-id="a112c-134">Si vous déployez un fichier .dbschema, il s’agit du comportement :</span><span class="sxs-lookup"><span data-stu-id="a112c-134">If you're deploying a .dbschema file, this is the behavior:</span></span>

- <span data-ttu-id="a112c-135">Si vous spécifiez **/dd+** ou **/dd**, VSDBCMD génère un script de déploiement et déployer la base de données.</span><span class="sxs-lookup"><span data-stu-id="a112c-135">If you specify **/dd+** or **/dd**, VSDBCMD will generate a deployment script and deploy the database.</span></span>
- <span data-ttu-id="a112c-136">Si vous spécifiez **/dd-** ou omettez le commutateur, VSDBCMD génère un script de déploiement uniquement.</span><span class="sxs-lookup"><span data-stu-id="a112c-136">If you specify **/dd-** or omit the switch, VSDBCMD will generate a deployment script only.</span></span>

> [!NOTE]
> <span data-ttu-id="a112c-137">Si vous déployez un fichier .deploymanifest plutôt que dans un fichier .dbschema, le comportement de la **/dd** commutateur est beaucoup plus compliqué.</span><span class="sxs-lookup"><span data-stu-id="a112c-137">If you're deploying a .deploymanifest file rather than a .dbschema file, the behavior of the **/dd** switch is a lot more complicated.</span></span> <span data-ttu-id="a112c-138">Essentiellement, VSDBCMD ignore la valeur de la **/dd** passer si le fichier .deploymanifest inclut un **DeployToDatabase** élément avec la valeur **True**.</span><span class="sxs-lookup"><span data-stu-id="a112c-138">Essentially, VSDBCMD will ignore the value of the **/dd** switch if the .deploymanifest file includes a **DeployToDatabase** element with a value of **True**.</span></span> <span data-ttu-id="a112c-139">[Déploiement de projets de base de données](../web-deployment-in-the-enterprise/deploying-database-projects.md) décrit ce comportement dans sa totalité.</span><span class="sxs-lookup"><span data-stu-id="a112c-139">[Deploying Database Projects](../web-deployment-in-the-enterprise/deploying-database-projects.md) describes this behavior in full.</span></span>


<span data-ttu-id="a112c-140">Par exemple, pour générer un script de déploiement pour le **ContactManager** base de données sans réellement déployer la base de données, votre commande VSDBCMD doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="a112c-140">For example, to generate a deployment script for the **ContactManager** database without actually deploying the database, your VSDBCMD command should resemble this:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


<span data-ttu-id="a112c-141">VSDBCMD est un outil de déploiement de base de données différentielle, et par conséquent le script de déploiement est généré dynamiquement pour contenir toutes les commandes SQL nécessaires pour mettre à jour la base de données actuel, s’il en existe, pour le schéma spécifié.</span><span class="sxs-lookup"><span data-stu-id="a112c-141">VSDBCMD is a differential database deployment tool, and as such the deployment script is dynamically generated to contain all the SQL commands necessary to update the current database, if one exists, to the specified schema.</span></span> <span data-ttu-id="a112c-142">Examiner le script de déploiement est un moyen utile pour déterminer l’impact de votre déploiement aura sur la base de données actuelle et les données qu’il contient.</span><span class="sxs-lookup"><span data-stu-id="a112c-142">Reviewing the deployment script is a useful way to determine what impact your deployment will have on the current database and the data it contains.</span></span> <span data-ttu-id="a112c-143">Par exemple, vous souhaiterez déterminer :</span><span class="sxs-lookup"><span data-stu-id="a112c-143">For example, you might want to determine:</span></span>

- <span data-ttu-id="a112c-144">Si des tables existantes seront supprimés, et si cela provoquera une perte de données.</span><span class="sxs-lookup"><span data-stu-id="a112c-144">Whether any existing tables will be removed, and whether that will result in data loss.</span></span>
- <span data-ttu-id="a112c-145">Indique si l’ordre des opérations comporte un risque de perte de données, par exemple, si vous êtes fractionner ou fusionner des tables.</span><span class="sxs-lookup"><span data-stu-id="a112c-145">Whether the order of operations carries a risk of data loss, for example, if you're splitting or merging tables.</span></span>

<span data-ttu-id="a112c-146">Si vous êtes satisfait avec le script de déploiement, vous pouvez répéter le VSDBCMD avec un **/dd+** indicateur pour apporter les modifications.</span><span class="sxs-lookup"><span data-stu-id="a112c-146">If you're happy with the deployment script, you can repeat the VSDBCMD with a **/dd+** flag to make the changes.</span></span> <span data-ttu-id="a112c-147">Vous pouvez également modifier le script de déploiement pour répondre à vos besoins, puis l’exécuter manuellement sur le serveur de base de données.</span><span class="sxs-lookup"><span data-stu-id="a112c-147">Alternatively, you can edit the deployment script to meet your requirements and then execute it manually on the database server.</span></span>

## <a name="integrating-what-if-functionality-into-custom-project-files"></a><span data-ttu-id="a112c-148">Intégration de fonctionnalité « Que se passe-t-il si » dans les fichiers de projet personnalisés</span><span class="sxs-lookup"><span data-stu-id="a112c-148">Integrating "What If" Functionality into Custom Project Files</span></span>

<span data-ttu-id="a112c-149">Dans les scénarios de déploiement plus complexes, que vous souhaitez utiliser un fichier de projet Microsoft Build Engine (MSBuild) personnalisé pour encapsuler votre logique de génération et de déploiement, comme décrit dans [présentation du fichier de projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span><span class="sxs-lookup"><span data-stu-id="a112c-149">In more complex deployment scenarios, you'll want to use a custom Microsoft Build Engine (MSBuild) project file to encapsulate your build and deployment logic, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="a112c-150">Par exemple, dans le [le Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) exemple de solution, le *Publish.proj* fichier :</span><span class="sxs-lookup"><span data-stu-id="a112c-150">For example, in the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, the *Publish.proj* file:</span></span>

- <span data-ttu-id="a112c-151">Génère la solution.</span><span class="sxs-lookup"><span data-stu-id="a112c-151">Builds the solution.</span></span>
- <span data-ttu-id="a112c-152">Utilise Web Deploy pour empaqueter et déployer l’application ContactManager.Mvc.</span><span class="sxs-lookup"><span data-stu-id="a112c-152">Uses Web Deploy to package and deploy the ContactManager.Mvc application.</span></span>
- <span data-ttu-id="a112c-153">Utilise Web Deploy pour empaqueter et déployer l’application ContactManager.Service.</span><span class="sxs-lookup"><span data-stu-id="a112c-153">Uses Web Deploy to package and deploy the ContactManager.Service application.</span></span>
- <span data-ttu-id="a112c-154">Déploie le **ContactManager** base de données.</span><span class="sxs-lookup"><span data-stu-id="a112c-154">Deploys the **ContactManager** database.</span></span>

<span data-ttu-id="a112c-155">Lorsque vous intégrez le déploiement de plusieurs bases de données et/ou les packages web un processus de cette façon, vous pouvez également la possibilité d’effectuer le déploiement complet en mode « que se passe-t-il si ».</span><span class="sxs-lookup"><span data-stu-id="a112c-155">When you integrate the deployment of multiple web packages and/or databases into a single-step process in this way, you may also want the option of performing the entire deployment in a "what if" mode.</span></span>

<span data-ttu-id="a112c-156">Le *Publish.proj* fichier montre comment vous pouvez faire cela.</span><span class="sxs-lookup"><span data-stu-id="a112c-156">The *Publish.proj* file demonstrates how you can do this.</span></span> <span data-ttu-id="a112c-157">Tout d’abord, vous devez créer une propriété pour stocker la valeur « que se passe-t-il si » :</span><span class="sxs-lookup"><span data-stu-id="a112c-157">First, you need to create a property to store the "what if" value:</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


<span data-ttu-id="a112c-158">Dans ce cas, vous avez créé une propriété nommée **WhatIf** avec la valeur par défaut **false**.</span><span class="sxs-lookup"><span data-stu-id="a112c-158">In this case, you've created a property named **WhatIf** with a default value of **false**.</span></span> <span data-ttu-id="a112c-159">Les utilisateurs peuvent remplacer cette valeur en affectant à la propriété **true** dans un paramètre de ligne de commande, comme vous le verrez bientôt.</span><span class="sxs-lookup"><span data-stu-id="a112c-159">Users can override this value by setting the property to **true** in a command-line parameter, as you'll see shortly.</span></span>

<span data-ttu-id="a112c-160">L’étape suivante consiste à paramétrer les Web Deploy et des commandes de VSDBCMD afin que les indicateurs de reflètent la **WhatIf** valeur de propriété.</span><span class="sxs-lookup"><span data-stu-id="a112c-160">The next stage is to parameterize any Web Deploy and VSDBCMD commands so that the flags reflect the **WhatIf** property value.</span></span> <span data-ttu-id="a112c-161">Par exemple, la cible suivante (effectuée à partir de la *Publish.proj* de fichiers et de simplifier la) s’exécute le *. deploy.cmd* fichier pour déployer un package web.</span><span class="sxs-lookup"><span data-stu-id="a112c-161">For example, the next target (taken from the *Publish.proj* file and simplified) runs the *.deploy.cmd* file to deploy a web package.</span></span> <span data-ttu-id="a112c-162">Par défaut, la commande inclut un **/Y** commutateur (« Oui » ou en mode de mise à jour).</span><span class="sxs-lookup"><span data-stu-id="a112c-162">By default, the command includes a **/Y** switch ("yes," or update mode).</span></span> <span data-ttu-id="a112c-163">Si **WhatIf** a la valeur **true**, il est remplacé par un **/T** commutateur (version d’évaluation ou le mode « que se passe-t-il si »).</span><span class="sxs-lookup"><span data-stu-id="a112c-163">If **WhatIf** is set to **true**, this is replaced by a **/T** switch (trial, or "what if" mode).</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


<span data-ttu-id="a112c-164">De même, la cible suivante utilise l’utilitaire VSDBCMD pour déployer une base de données.</span><span class="sxs-lookup"><span data-stu-id="a112c-164">Similarly, the next target uses the VSDBCMD utility to deploy a database.</span></span> <span data-ttu-id="a112c-165">Par défaut, un **/dd** commutateur n’est pas inclus.</span><span class="sxs-lookup"><span data-stu-id="a112c-165">By default, a **/dd** switch is not included.</span></span> <span data-ttu-id="a112c-166">Cela signifie que VSDBCMD génère un script de déploiement, mais ne déploiera pas de la base de données & #x 2014 ; en d’autres termes, un scénario « que se passe-t-il si ».</span><span class="sxs-lookup"><span data-stu-id="a112c-166">This means that VSDBCMD will generate a deployment script but will not deploy the database&#x2014;in other words, a "what if" scenario.</span></span> <span data-ttu-id="a112c-167">Si le **WhatIf** propriété n’est pas définie sur **true**, un **/dd** commutateur est ajouté et VSDBCMD déployez la base de données.</span><span class="sxs-lookup"><span data-stu-id="a112c-167">If the **WhatIf** property is not set to **true**, a **/dd** switch is added and VSDBCMD will deploy the database.</span></span>


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


<span data-ttu-id="a112c-168">Vous pouvez utiliser la même approche pour paramétrer toutes les commandes appropriées dans votre fichier projet.</span><span class="sxs-lookup"><span data-stu-id="a112c-168">You can use the same approach to parameterize all the relevant commands in your project file.</span></span> <span data-ttu-id="a112c-169">Lorsque vous souhaitez exécuter un déploiement « que se passe-t-il si », vous pouvez ensuite simplement fournir un **WhatIf** valeur de propriété à partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="a112c-169">When you want to run a "what if" deployment, you can then simply provide a **WhatIf** property value from the command line:</span></span>


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


<span data-ttu-id="a112c-170">De cette façon, vous pouvez exécuter un déploiement « que se passe-t-il si » pour tous les composants de votre projet en une seule étape.</span><span class="sxs-lookup"><span data-stu-id="a112c-170">In this way, you can run a "what if" deployment for all your project components in a single step.</span></span>

## <a name="conclusion"></a><span data-ttu-id="a112c-171">Conclusion</span><span class="sxs-lookup"><span data-stu-id="a112c-171">Conclusion</span></span>

<span data-ttu-id="a112c-172">Cette rubrique décrit comment exécuter « que se passe-t-il si » les déploiements à l’aide de Web Deploy VSDBCMD et MSBuild.</span><span class="sxs-lookup"><span data-stu-id="a112c-172">This topic described how to run "what if" deployments using Web Deploy, VSDBCMD, and MSBuild.</span></span> <span data-ttu-id="a112c-173">Un déploiement « que se passe-t-il si » vous permet d’évaluer l’impact d’un déploiement proposé avant de procéder réellement des modifications à l’environnement de destination.</span><span class="sxs-lookup"><span data-stu-id="a112c-173">A "what if" deployment lets you evaluate the impact of a proposed deployment before you actually make any changes to the destination environment.</span></span>

## <a name="further-reading"></a><span data-ttu-id="a112c-174">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="a112c-174">Further Reading</span></span>

<span data-ttu-id="a112c-175">Pour plus d’informations sur la syntaxe de ligne de commande de Web Deploy, consultez [Web Deploy opération Settings](https://technet.microsoft.com/en-us/library/dd569089(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="a112c-175">For more information on Web Deploy command-line syntax, see [Web Deploy Operation Settings](https://technet.microsoft.com/en-us/library/dd569089(WS.10).aspx).</span></span> <span data-ttu-id="a112c-176">Pour obtenir des conseils sur les options de ligne de commande lorsque vous utilisez la *. deploy.cmd* de fichiers, consultez [Comment : installer un Package de déploiement à l’aide du fichier deploy.cmd](https://msdn.microsoft.com/en-us/library/ff356104.aspx).</span><span class="sxs-lookup"><span data-stu-id="a112c-176">For guidance on command-line options when you use the *.deploy.cmd* file, see [How to: Install a Deployment Package Using the deploy.cmd File](https://msdn.microsoft.com/en-us/library/ff356104.aspx).</span></span> <span data-ttu-id="a112c-177">Pour obtenir des conseils sur la syntaxe de ligne de commande de VSDBCMD, consultez [référence de ligne de commande pour VSDBCMD. EXE (déploiement et importation de schéma)](https://msdn.microsoft.com/en-us/library/dd193283.aspx).</span><span class="sxs-lookup"><span data-stu-id="a112c-177">For guidance on VSDBCMD command-line syntax, see [Command-Line Reference for VSDBCMD.EXE (Deployment and Schema Import)](https://msdn.microsoft.com/en-us/library/dd193283.aspx).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a112c-178">[Précédent](advanced-enterprise-web-deployment.md)
[Suivant](customizing-database-deployments-for-multiple-environments.md)</span><span class="sxs-lookup"><span data-stu-id="a112c-178">[Previous](advanced-enterprise-web-deployment.md)
[Next](customizing-database-deployments-for-multiple-environments.md)</span></span>