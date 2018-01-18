---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: "À l’aide de $select, $expand et $value dans ASP.NET Web API 2 OData | Documents Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="87544-102">À l’aide de $select, $expand et $value dans ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="87544-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="87544-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="87544-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="87544-104">API Web 2 ajoute la prise en charge pour le $expand, $select et options $value OData.</span><span class="sxs-lookup"><span data-stu-id="87544-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="87544-105">Ces options permettent à un client contrôler la représentation, il récupère à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="87544-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="87544-106">**$expand** provoque des entités associées être incluses inline dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="87544-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="87544-107">**$select** sélectionne un sous-ensemble de propriétés à inclure dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="87544-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="87544-108">**$value** Obtient la valeur brute d’une propriété.</span><span class="sxs-lookup"><span data-stu-id="87544-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="87544-109">Schéma de l’exemple</span><span class="sxs-lookup"><span data-stu-id="87544-109">Example Schema</span></span>

<span data-ttu-id="87544-110">Pour cet article, je vais utiliser un service OData qui définit trois entités : produit, fournisseur et la catégorie.</span><span class="sxs-lookup"><span data-stu-id="87544-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="87544-111">Chaque produit possède une catégorie et un seul fournisseur.</span><span class="sxs-lookup"><span data-stu-id="87544-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="87544-112">Voici les classes c# qui définissent les modèles d’entité :</span><span class="sxs-lookup"><span data-stu-id="87544-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="87544-113">Notez que la `Product` classe définit les propriétés de navigation pour la `Supplier` et `Category`.</span><span class="sxs-lookup"><span data-stu-id="87544-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="87544-114">La `Category` classe définit une propriété de navigation pour les produits dans chaque catégorie.</span><span class="sxs-lookup"><span data-stu-id="87544-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="87544-115">Pour créer un point de terminaison OData pour ce schéma, utilisez la génération de modèles automatique Visual Studio 2013, comme décrit dans [création d’un point de terminaison OData dans ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="87544-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="87544-116">Ajouter des contrôleurs distincts pour le fournisseur, catégorie et produit.</span><span class="sxs-lookup"><span data-stu-id="87544-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="87544-117">L’activation de $développez et $select</span><span class="sxs-lookup"><span data-stu-id="87544-117">Enabling $expand and $select</span></span>

<span data-ttu-id="87544-118">Dans Visual Studio 2013, la génération de modèles automatique OData d’API Web crée un contrôleur qui prend automatiquement en charge $expand et un $select.</span><span class="sxs-lookup"><span data-stu-id="87544-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="87544-119">Pour référence, voici la configuration requise pour prendre en charge $développez et $select dans un contrôleur.</span><span class="sxs-lookup"><span data-stu-id="87544-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="87544-120">Pour les de collections, le contrôleur `Get` méthode doit retourner un **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="87544-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="87544-121">Pour les entités uniques, retourner un **SingleResult&lt;T&gt;**, où T est un **IQueryable** qui contient les entités zéro ou un.</span><span class="sxs-lookup"><span data-stu-id="87544-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="87544-122">Également, décorer votre `Get` méthodes avec les **[Queryable]** d’attribut, comme indiqué dans les extraits de code précédent.</span><span class="sxs-lookup"><span data-stu-id="87544-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="87544-123">Vous pouvez également appeler **EnableQuerySupport** sur la **HttpConfiguration** objet au démarrage.</span><span class="sxs-lookup"><span data-stu-id="87544-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="87544-124">(Pour plus d’informations, consultez [l’activation des Options de requête OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="87544-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="87544-125">À l’aide de $développez</span><span class="sxs-lookup"><span data-stu-id="87544-125">Using $expand</span></span>

<span data-ttu-id="87544-126">Lorsque vous interrogez une entité OData ou la collection, la réponse par défaut n’inclut pas les entités associées.</span><span class="sxs-lookup"><span data-stu-id="87544-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="87544-127">Par exemple, voici la réponse par défaut pour le jeu d’entités catégories :</span><span class="sxs-lookup"><span data-stu-id="87544-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="87544-128">Comme vous pouvez le voir, la réponse n’inclut pas tous les produits, même si l’entité Category possède un lien de navigation de produits.</span><span class="sxs-lookup"><span data-stu-id="87544-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="87544-129">Toutefois, le client peut utiliser $développer pour obtenir la liste des produits pour chaque catégorie.</span><span class="sxs-lookup"><span data-stu-id="87544-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="87544-130">Le $expand option s’intègre dans la chaîne de requête de la demande :</span><span class="sxs-lookup"><span data-stu-id="87544-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="87544-131">Le serveur inclut désormais les produits pour chaque catégorie, inline avec les catégories.</span><span class="sxs-lookup"><span data-stu-id="87544-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="87544-132">Voici la charge utile de réponse :</span><span class="sxs-lookup"><span data-stu-id="87544-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="87544-133">Notez que chaque entrée dans le tableau « value » contient une liste de produits.</span><span class="sxs-lookup"><span data-stu-id="87544-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="87544-134">Le $expand liste prend séparées par des virgules des options de propriétés de navigation à développer.</span><span class="sxs-lookup"><span data-stu-id="87544-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="87544-135">La requête suivante étend la catégorie et le fournisseur pour un produit.</span><span class="sxs-lookup"><span data-stu-id="87544-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="87544-136">Voici le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="87544-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="87544-137">Vous pouvez développer plus d’un niveau de la propriété de navigation.</span><span class="sxs-lookup"><span data-stu-id="87544-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="87544-138">L’exemple suivant inclut tous les produits pour une catégorie, ainsi que le fournisseur pour chaque produit.</span><span class="sxs-lookup"><span data-stu-id="87544-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="87544-139">Voici le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="87544-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="87544-140">Par défaut, les API Web limite la profondeur d’expansion maximale à 2.</span><span class="sxs-lookup"><span data-stu-id="87544-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="87544-141">Le client qui empêche d’envoyer des requêtes complexes telles que `$expand=Orders/OrderDetails/Product/Supplier/Region`, qui peut être inefficace pour interroger et créer des réponses de grande taille.</span><span class="sxs-lookup"><span data-stu-id="87544-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="87544-142">Pour substituer la valeur par défaut, définissez la **MaxExpansionDepth** propriété sur le **[Queryable]** attribut.</span><span class="sxs-lookup"><span data-stu-id="87544-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="87544-143">Pour plus d’informations sur la $développez option, consultez [développez système requête Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) dans la documentation officielle de OData.</span><span class="sxs-lookup"><span data-stu-id="87544-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="87544-144">À l’aide de $select</span><span class="sxs-lookup"><span data-stu-id="87544-144">Using $select</span></span>

<span data-ttu-id="87544-145">L’option $select spécifie un sous-ensemble de propriétés à inclure dans le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="87544-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="87544-146">Par exemple, pour obtenir uniquement le nom et le prix de chaque produit, utilisez la requête suivante :</span><span class="sxs-lookup"><span data-stu-id="87544-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="87544-147">Voici le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="87544-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="87544-148">Vous pouvez combiner $select et $expand dans la même requête.</span><span class="sxs-lookup"><span data-stu-id="87544-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="87544-149">Veillez à inclure la propriété développée dans l’option $select.</span><span class="sxs-lookup"><span data-stu-id="87544-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="87544-150">Par exemple, la requête suivante obtient le nom de produit et le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="87544-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="87544-151">Voici le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="87544-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="87544-152">Vous pouvez également sélectionner les propriétés dans une propriété développée.</span><span class="sxs-lookup"><span data-stu-id="87544-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="87544-153">La requête suivante étend les produits et sélectionne le nom de la catégorie et le nom du produit.</span><span class="sxs-lookup"><span data-stu-id="87544-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="87544-154">Voici le corps de réponse :</span><span class="sxs-lookup"><span data-stu-id="87544-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="87544-155">Pour plus d’informations sur l’option $select, consultez [système Option de requête Select ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) dans la documentation officielle de OData.</span><span class="sxs-lookup"><span data-stu-id="87544-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="87544-156">Obtention des propriétés individuelles d’une entité ($value)</span><span class="sxs-lookup"><span data-stu-id="87544-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="87544-157">Il existe deux façons pour un client OData obtenir une propriété individuelle d’une entité.</span><span class="sxs-lookup"><span data-stu-id="87544-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="87544-158">Le client peut obtenir la valeur dans le format OData, soit obtenir la valeur brute de la propriété.</span><span class="sxs-lookup"><span data-stu-id="87544-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="87544-159">La requête suivante obtient une propriété dans le format OData.</span><span class="sxs-lookup"><span data-stu-id="87544-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="87544-160">Voici un exemple de réponse au format JSON :</span><span class="sxs-lookup"><span data-stu-id="87544-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="87544-161">Pour obtenir la valeur brute de la propriété, ajoute $value à l’URI :</span><span class="sxs-lookup"><span data-stu-id="87544-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="87544-162">Voici la réponse.</span><span class="sxs-lookup"><span data-stu-id="87544-162">Here is the response.</span></span> <span data-ttu-id="87544-163">Notez que le type de contenu est « text/plain », pas JSON.</span><span class="sxs-lookup"><span data-stu-id="87544-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="87544-164">Pour prendre en charge ces requêtes dans votre contrôleur OData, ajoutez une méthode nommée `GetProperty`, où `Property` est le nom de la propriété.</span><span class="sxs-lookup"><span data-stu-id="87544-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="87544-165">Par exemple, la méthode à obtenir la propriété Name est nommée `GetName`.</span><span class="sxs-lookup"><span data-stu-id="87544-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="87544-166">La méthode doit retourner la valeur de cette propriété :</span><span class="sxs-lookup"><span data-stu-id="87544-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]