---
title: Bien démarrer avec NSwag et ASP.NET Core
author: zuckerthoben
description: Découvrez comment utiliser NSwag pour générer des pages d’aide et de documentation pour une API web ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 0f0aa0eeaa174ef40f03aab2500a8b3ce37e9448
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="c1c88-103">Bien démarrer avec NSwag et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1c88-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="c1c88-104">Par [Christoph Nienaber](https://twitter.com/zuckerthoben) et [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="c1c88-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="c1c88-105">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c1c88-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="c1c88-106">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c1c88-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="c1c88-107">L’utilisation de [NSwag](https://github.com/RSuter/NSwag) avec un intergiciel (middleware) ASP.NET Core nécessite le package NuGet [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="c1c88-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="c1c88-108">Le package se compose d’un générateur Swagger, de l’IU Swagger (v2 et v3) et de [l’IU ReDoc](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="c1c88-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="c1c88-109">Nous vous recommandons vivement d’utiliser les fonctionnalités de génération de code de NSwag.</span><span class="sxs-lookup"><span data-stu-id="c1c88-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="c1c88-110">Choisissez l’une des options suivantes pour la génération de code :</span><span class="sxs-lookup"><span data-stu-id="c1c88-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="c1c88-111">Utiliser [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), une application de bureau Windows pour générer du code client en C# et TypeScript pour votre API.</span><span class="sxs-lookup"><span data-stu-id="c1c88-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="c1c88-112">Utiliser les packages NuGet [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) ou [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) pour générer du code dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="c1c88-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="c1c88-113">Utiliser NSwag à partir de la [ligne de commande](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="c1c88-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="c1c88-114">Utiliser le package NuGet [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="c1c88-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="c1c88-115">Fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="c1c88-115">Features</span></span>

<span data-ttu-id="c1c88-116">La principale raison d’utiliser NSwag est non seulement de pouvoir utiliser l’IU Swagger et le générateur Swagger, mais aussi d’utiliser ses fonctionnalités de génération de code flexibles.</span><span class="sxs-lookup"><span data-stu-id="c1c88-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="c1c88-117">Vous n’avez pas besoin d’une API existante, vous pouvez utiliser des API de tiers qui intègrent Swagger et laissent NSwag générer une implémentation du client.</span><span class="sxs-lookup"><span data-stu-id="c1c88-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="c1c88-118">Dans les deux cas, le cycle de développement est rapide et vous pouvez vous adapter plus facilement aux changements d’API.</span><span class="sxs-lookup"><span data-stu-id="c1c88-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="c1c88-119">Installation de package</span><span class="sxs-lookup"><span data-stu-id="c1c88-119">Package installation</span></span>

<span data-ttu-id="c1c88-120">Vous pouvez ajouter le package NuGet NSwag avec l’une des méthodes suivantes :</span><span class="sxs-lookup"><span data-stu-id="c1c88-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c1c88-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c1c88-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c1c88-122">À partir de la fenêtre **Console du Gestionnaire de package** :</span><span class="sxs-lookup"><span data-stu-id="c1c88-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="c1c88-123">Accédez à **Affichage** > **Autres fenêtres** > **Console du Gestionnaire de package**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="c1c88-124">Accédez au répertoire où se trouve le fichier *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c1c88-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="c1c88-125">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c1c88-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="c1c88-126">À partir de la boîte de dialogue **Gérer les packages NuGet** :</span><span class="sxs-lookup"><span data-stu-id="c1c88-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="c1c88-127">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions** > **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="c1c88-128">Affectez la valeur « nuget.org » à **Source du package**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="c1c88-129">Entrez « NSwag.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="c1c88-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="c1c88-130">Sélectionnez le package « NSwag.AspNetCore » sous l’onglet **Parcourir** et cliquez sur **Installer**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c1c88-131">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c1c88-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c1c88-132">Cliquez avec le bouton droit sur le dossier *Packages* dans **Panneau Solutions** > **Ajouter des packages**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="c1c88-133">Dans la fenêtre **Ajouter des packages**, sélectionnez « nuget.org » dans la liste déroulante **Source**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="c1c88-134">Entrez « NSwag.AspNetCore » dans la zone de recherche.</span><span class="sxs-lookup"><span data-stu-id="c1c88-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="c1c88-135">Sélectionnez le package « NSwag.AspNetCore » dans le volet de résultats, puis cliquez sur **Ajouter un package**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c1c88-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c1c88-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c1c88-137">Exécutez la commande suivante à partir du **Terminal intégré** :</span><span class="sxs-lookup"><span data-stu-id="c1c88-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c1c88-138">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="c1c88-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c1c88-139">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="c1c88-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="c1c88-140">Ajouter et configurer l’intergiciel (middleware) Swagger</span><span class="sxs-lookup"><span data-stu-id="c1c88-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="c1c88-141">Importez les espaces de noms suivants dans la classe `Startup` :</span><span class="sxs-lookup"><span data-stu-id="c1c88-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="c1c88-142">Dans la méthode `Startup.Configure`, activez l’intergiciel pour traiter la spécification Swagger générée et l’IU Swagger :</span><span class="sxs-lookup"><span data-stu-id="c1c88-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="c1c88-143">Lancez l’application.</span><span class="sxs-lookup"><span data-stu-id="c1c88-143">Launch the app.</span></span> <span data-ttu-id="c1c88-144">Accédez à `http://localhost:<port>/swagger` pour voir l’IU Swagger.</span><span class="sxs-lookup"><span data-stu-id="c1c88-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="c1c88-145">Accédez à `http://localhost:<port>/swagger/v1/swagger.json` pour voir la spécification Swagger.</span><span class="sxs-lookup"><span data-stu-id="c1c88-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="c1c88-146">Génération de code</span><span class="sxs-lookup"><span data-stu-id="c1c88-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="c1c88-147">Via NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="c1c88-147">Via NSwagStudio</span></span>

* <span data-ttu-id="c1c88-148">Installez NSwagStudio à partir du [dépôt GitHub](https://github.com/RSuter/NSwag/wiki/NSwagStudio) officiel.</span><span class="sxs-lookup"><span data-stu-id="c1c88-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="c1c88-149">Lancez NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="c1c88-149">Launch NSwagStudio.</span></span> <span data-ttu-id="c1c88-150">Entrez l’URL du fichier *swagger.json* dans la zone de texte **Swagger Specification URL** (URL de spécification Swagger), puis cliquez sur le bouton **Create local Copy** (Créer une copie locale).</span><span class="sxs-lookup"><span data-stu-id="c1c88-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="c1c88-151">Sélectionnez le type de sortie client **CSharp Client** (Client CSharp).</span><span class="sxs-lookup"><span data-stu-id="c1c88-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="c1c88-152">Parmi les autres options, citons **TypeScript Client** (Client TypeScript) et **CSharp Web API Controller** (Contrôleur d’API web CSharp).</span><span class="sxs-lookup"><span data-stu-id="c1c88-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="c1c88-153">Un contrôleur d’API web vous permet fondamentalement d’effectuer une génération inverse.</span><span class="sxs-lookup"><span data-stu-id="c1c88-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="c1c88-154">Il utilise la spécification d’un service pour regénérer le service.</span><span class="sxs-lookup"><span data-stu-id="c1c88-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="c1c88-155">Cliquez sur le bouton **Generate Outputs** (Générer des sorties).</span><span class="sxs-lookup"><span data-stu-id="c1c88-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="c1c88-156">Une implémentation client C# complète du projet *TodoApi.NSwag* est produite.</span><span class="sxs-lookup"><span data-stu-id="c1c88-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="c1c88-157">Cliquez sur l’onglet **CSharp Client** (Client CSharp) de la section **Outputs** (Sorties) pour afficher le code client généré :</span><span class="sxs-lookup"><span data-stu-id="c1c88-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> <span data-ttu-id="c1c88-158">Le code client C# est généré en fonction des paramètres définis sous l’onglet **Settings** (Paramètres) de l’onglet **CSharp Client** (Client CSharp). Modifiez les paramètres pour effectuer des tâches telles que le renommage de l’espace de noms par défaut et la génération de méthodes synchrones.</span><span class="sxs-lookup"><span data-stu-id="c1c88-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="c1c88-159">Copiez le code C# généré dans un fichier dans un projet client (par exemple, une application [Xamarin.Forms](/xamarin/xamarin-forms/)).</span><span class="sxs-lookup"><span data-stu-id="c1c88-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="c1c88-160">Démarrez l’utilisation de l’API web :</span><span class="sxs-lookup"><span data-stu-id="c1c88-160">Start consuming the web API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="c1c88-161">Vous pouvez injecter une URL de base et/ou un client HTTP dans le client d’API.</span><span class="sxs-lookup"><span data-stu-id="c1c88-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="c1c88-162">En général, il vaut mieux toujours [réutiliser HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span><span class="sxs-lookup"><span data-stu-id="c1c88-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="c1c88-163">Autres façons de générer du code client</span><span class="sxs-lookup"><span data-stu-id="c1c88-163">Other ways to generate client code</span></span>

<span data-ttu-id="c1c88-164">Vous pouvez générer le code client d’autres manières, plus adaptées à votre flux de travail :</span><span class="sxs-lookup"><span data-stu-id="c1c88-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="c1c88-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="c1c88-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="c1c88-166">Dans le code</span><span class="sxs-lookup"><span data-stu-id="c1c88-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="c1c88-167">Modèles T4</span><span class="sxs-lookup"><span data-stu-id="c1c88-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="c1c88-168">Personnaliser</span><span class="sxs-lookup"><span data-stu-id="c1c88-168">Customize</span></span>

<span data-ttu-id="c1c88-169">Swagger fournit des options pour documenter le modèle objet pour faciliter l’utilisation de l’API web.</span><span class="sxs-lookup"><span data-stu-id="c1c88-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="c1c88-170">Informations sur l’API et description</span><span class="sxs-lookup"><span data-stu-id="c1c88-170">API info and description</span></span>

<span data-ttu-id="c1c88-171">Dans la méthode `Startup.Configure`, une action de configuration passée à la méthode `UseSwagger` ajoute des informations comme l’auteur, la licence et la description :</span><span class="sxs-lookup"><span data-stu-id="c1c88-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="c1c88-172">L’IU Swagger affiche les informations de la version :</span><span class="sxs-lookup"><span data-stu-id="c1c88-172">The Swagger UI displays the version's information:</span></span>

![Interface utilisateur de Swagger avec des informations de version](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="c1c88-174">commentaires XML</span><span class="sxs-lookup"><span data-stu-id="c1c88-174">XML comments</span></span>

<span data-ttu-id="c1c88-175">Vous pouvez activer les commentaires XML de l’une des façons suivantes :</span><span class="sxs-lookup"><span data-stu-id="c1c88-175">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="c1c88-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c1c88-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="c1c88-177">Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="c1c88-178">Cochez la case **Fichier de documentation XML** dans la section **Sortie** sous l’onglet **Générer**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="c1c88-179">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="c1c88-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="c1c88-180">Ouvrez la boîte de dialogue **Options du projet** > **Générer** > **Compilateur**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-180">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="c1c88-181">Cochez la case **Générer la documentation XML** dans la section **Options générales**.</span><span class="sxs-lookup"><span data-stu-id="c1c88-181">Check the **Generate xml documentation** box under the **General Options** section</span></span>

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="c1c88-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c1c88-182">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="c1c88-183">Ajoutez manuellement l’extrait de code suivant au fichier *.csproj* :</span><span class="sxs-lookup"><span data-stu-id="c1c88-183">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a><span data-ttu-id="c1c88-184">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="c1c88-184">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="c1c88-185">NSwag utilise la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection), et le type de retour recommandé pour les actions d’API web est [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="c1c88-185">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="c1c88-186">Par conséquent, NSwag ne peut pas déduire votre action et ce qu’elle retourne.</span><span class="sxs-lookup"><span data-stu-id="c1c88-186">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="c1c88-187">Prenons l'exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="c1c88-187">Consider the following example:</span></span>

<span data-ttu-id="c1c88-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="c1c88-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="c1c88-189">L’action précédente retourne `IActionResult`, mais à l’intérieur de l’action, [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) ou [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) sont retournés.</span><span class="sxs-lookup"><span data-stu-id="c1c88-189">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="c1c88-190">Les annotations de données sont utilisées pour indiquer aux clients les codes d’état HTTP que cette action est susceptible de retourner.</span><span class="sxs-lookup"><span data-stu-id="c1c88-190">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="c1c88-191">Décorez l’action avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="c1c88-191">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="c1c88-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="c1c88-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="c1c88-193">NSwag utilise la [réflexion](/dotnet/csharp/programming-guide/concepts/reflection), et le type de retour recommandé pour les actions d’API web est [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="c1c88-193">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="c1c88-194">NSwag peut donc uniquement déduire le type de retour défini par `T`.</span><span class="sxs-lookup"><span data-stu-id="c1c88-194">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="c1c88-195">Il n’est pas possible de déduire d’autres types de retour possibles.</span><span class="sxs-lookup"><span data-stu-id="c1c88-195">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="c1c88-196">Prenons l'exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="c1c88-196">Consider the following example:</span></span>

<span data-ttu-id="c1c88-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="c1c88-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="c1c88-198">L’action précédente retourne `ActionResult<T>`, mais à l’intérieur de l’action, [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) est retourné.</span><span class="sxs-lookup"><span data-stu-id="c1c88-198">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="c1c88-199">Étant donné que le contrôleur est décoré avec l’attribut [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), une réponse [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) est également possible.</span><span class="sxs-lookup"><span data-stu-id="c1c88-199">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="c1c88-200">Pour plus d’informations, consultez [Réponses HTTP 400 automatiques](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="c1c88-200">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="c1c88-201">Les annotations de données sont utilisées pour indiquer aux clients les codes d’état HTTP que cette action est susceptible de retourner.</span><span class="sxs-lookup"><span data-stu-id="c1c88-201">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="c1c88-202">Décorez l’action avec les attributs suivants :</span><span class="sxs-lookup"><span data-stu-id="c1c88-202">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="c1c88-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="c1c88-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end

<span data-ttu-id="c1c88-204">Le générateur Swagger peut maintenant décrire précisément cette action et les clients générés savent ce qu’ils reçoivent quand ils appellent le point de terminaison.</span><span class="sxs-lookup"><span data-stu-id="c1c88-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="c1c88-205">Nous vous recommandons vivement de décorer toutes les actions avec ces attributs.</span><span class="sxs-lookup"><span data-stu-id="c1c88-205">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="c1c88-206">Pour obtenir des indications sur les réponses HTTP que doivent retourner vos actions d’API, consultez la [spécification RFC 7231](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="c1c88-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>