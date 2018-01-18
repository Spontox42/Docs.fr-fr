---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: "À l’aide d’Entity Framework 4.0 et du contrôle ObjectDataSource, partie 3 : tri et de filtrage | Documents Microsoft"
author: tdykstra
description: "Cette série de didacticiels s’appuie sur l’application web de l’université Contoso qui est créée par la prise en main de la série de didacticiels Entity Framework 4.0. JE..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 4accd3381a66bde1f87f0dc7dd95beeb54fcc6a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="8cb22-104">À l’aide d’Entity Framework 4.0 et du contrôle ObjectDataSource, partie 3 : tri et filtrage</span><span class="sxs-lookup"><span data-stu-id="8cb22-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>
====================
<span data-ttu-id="8cb22-105">Par [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8cb22-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="8cb22-106">Cette série de didacticiels s’appuie sur l’application web de l’université Contoso qui est créée par le [mise en route avec l’Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de didacticiels.</span><span class="sxs-lookup"><span data-stu-id="8cb22-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="8cb22-107">Si vous n’avez pas terminé les didacticiels antérieures, comme point de départ pour ce didacticiel vous pouvez [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous devriez avoir créé.</span><span class="sxs-lookup"><span data-stu-id="8cb22-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="8cb22-108">Vous pouvez également [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créé par la série de didacticiels terminée.</span><span class="sxs-lookup"><span data-stu-id="8cb22-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="8cb22-109">Si vous avez des questions sur les didacticiels, vous pouvez les valider pour le [forum de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="8cb22-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>


<span data-ttu-id="8cb22-110">Dans le didacticiel précédent, vous avez implémenté le modèle de référentiel dans une application multicouche web qui utilise Entity Framework et le `ObjectDataSource` contrôle.</span><span class="sxs-lookup"><span data-stu-id="8cb22-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="8cb22-111">Ce didacticiel montre comment effectuer le tri et de filtrage et de gérer les scénarios maître / détail.</span><span class="sxs-lookup"><span data-stu-id="8cb22-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="8cb22-112">Vous allez ajouter les améliorations suivantes apportées à la *Departments.aspx* page :</span><span class="sxs-lookup"><span data-stu-id="8cb22-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="8cb22-113">Une zone de texte pour permettre aux utilisateurs de sélectionner les services par nom.</span><span class="sxs-lookup"><span data-stu-id="8cb22-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="8cb22-114">Une liste de cours pour chaque département, qui est indiqué dans la grille.</span><span class="sxs-lookup"><span data-stu-id="8cb22-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="8cb22-115">La possibilité de trier en cliquant sur les en-têtes de colonne.</span><span class="sxs-lookup"><span data-stu-id="8cb22-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="8cb22-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8cb22-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="8cb22-117">Outre la possibilité de trier les colonnes GridView</span><span class="sxs-lookup"><span data-stu-id="8cb22-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="8cb22-118">Ouvrez le *Departments.aspx* page et ajoutez un `SortParameterName="sortExpression"` attribut le `ObjectDataSource` contrôle nommé `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="8cb22-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="8cb22-119">(Plus tard, vous allez créer un `GetDepartments` méthode qui accepte un paramètre nommé `sortExpression`.) Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="8cb22-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="8cb22-120">Ajouter le `AllowSorting="true"` d’attribut à la balise d’ouverture de la `GridView` contrôle.</span><span class="sxs-lookup"><span data-stu-id="8cb22-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="8cb22-121">Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="8cb22-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="8cb22-122">Dans *Departments.aspx.cs*, définir l’ordre de tri par défaut en appelant le `GridView` du contrôle `Sort` méthode à partir de la `Page_Load` méthode :</span><span class="sxs-lookup"><span data-stu-id="8cb22-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="8cb22-123">Vous pouvez ajouter du code trie ou les filtres dans la classe de logique métier ou de la classe de référentiel.</span><span class="sxs-lookup"><span data-stu-id="8cb22-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="8cb22-124">Si vous le faites dans la classe de logique métier, tri ou filtrage du travail est effectué une fois que les données sont récupérées à partir de la base de données, car l’utilisation de la classe de logique métier avec un `IEnumerable` objet retourné par l’espace de stockage.</span><span class="sxs-lookup"><span data-stu-id="8cb22-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="8cb22-125">Si vous ajoutez triée et filtrée de code de la classe de l’espace de stockage et de vous faire avant d’une expression LINQ ou de requête d’objet a été converti en un `IEnumerable` de l’objet, vos commandes sont transmises à la base de données pour le traitement, qui est généralement plus efficace.</span><span class="sxs-lookup"><span data-stu-id="8cb22-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="8cb22-126">Dans ce didacticiel, vous allez implémenter le tri et le filtrage d’une manière qui provoque le traitement doit être fait par la base de données, autrement dit, dans le référentiel.</span><span class="sxs-lookup"><span data-stu-id="8cb22-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="8cb22-127">Pour ajouter la fonctionnalité de tri, vous devez ajouter une nouvelle méthode à l’interface de référentiel et les classes du référentiel, ainsi que pour la classe de logique métier.</span><span class="sxs-lookup"><span data-stu-id="8cb22-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="8cb22-128">Dans le *ISchoolRepository.cs* , ajoutez un nouveau `GetDepartments` méthode qui accepte un `sortExpression` paramètre qui permet de trier la liste des services qui est retournée :</span><span class="sxs-lookup"><span data-stu-id="8cb22-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="8cb22-129">Le `sortExpression` paramètre spécifie la colonne de tri et le sens du tri.</span><span class="sxs-lookup"><span data-stu-id="8cb22-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="8cb22-130">Ajouter du code pour la nouvelle méthode le *SchoolRepository.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="8cb22-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="8cb22-131">Modifier existants sans paramètre `GetDepartments` méthode à appeler la nouvelle méthode :</span><span class="sxs-lookup"><span data-stu-id="8cb22-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="8cb22-132">Dans le projet de test, ajoutez la nouvelle méthode suivante à *MockSchoolRepository.cs*:</span><span class="sxs-lookup"><span data-stu-id="8cb22-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="8cb22-133">Si vous souhaitez créer des tests unitaires qui dépendaient de cette méthode qui retourne une liste triée, vous devez trier la liste avant de le renvoyer.</span><span class="sxs-lookup"><span data-stu-id="8cb22-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="8cb22-134">Vous n’allez pas créer les tests comme cela dans ce didacticiel, afin que la méthode puisse retourner simplement la liste non triée de services.</span><span class="sxs-lookup"><span data-stu-id="8cb22-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="8cb22-135">Dans le *SchoolBL.cs* , ajoutez la nouvelle méthode suivante à la classe de logique métier :</span><span class="sxs-lookup"><span data-stu-id="8cb22-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="8cb22-136">Ce code passe le paramètre de tri à la méthode de référentiel.</span><span class="sxs-lookup"><span data-stu-id="8cb22-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="8cb22-137">Exécutez le *Departments.aspx* page.</span><span class="sxs-lookup"><span data-stu-id="8cb22-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="8cb22-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8cb22-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="8cb22-139">Vous pouvez maintenant cliquer sur n’importe quel en-tête de colonne pour trier par colonne.</span><span class="sxs-lookup"><span data-stu-id="8cb22-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="8cb22-140">Si la colonne est déjà triée, en cliquant sur l’en-tête inverse le sens de tri.</span><span class="sxs-lookup"><span data-stu-id="8cb22-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="8cb22-141">Ajout d’une zone de recherche</span><span class="sxs-lookup"><span data-stu-id="8cb22-141">Adding a Search Box</span></span>

<span data-ttu-id="8cb22-142">Dans cette section, vous allez ajouter une zone de texte de recherche, liez-le à la `ObjectDataSource` contrôler à l’aide d’un paramètre de contrôle et ajoutez une méthode à la classe de logique métier pour prendre en charge le filtrage.</span><span class="sxs-lookup"><span data-stu-id="8cb22-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="8cb22-143">Ouvrez le *Departments.aspx* page et ajoutez le balisage suivant entre le titre et la première `ObjectDataSource` contrôle :</span><span class="sxs-lookup"><span data-stu-id="8cb22-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="8cb22-144">Dans le `ObjectDataSource` contrôle nommé `DepartmentsObjectDataSource`, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="8cb22-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="8cb22-145">Ajouter un `SelectParameters` , élément pour un paramètre nommé `nameSearchString` qui obtient la valeur entrée dans le `SearchTextBox` contrôle.</span><span class="sxs-lookup"><span data-stu-id="8cb22-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="8cb22-146">Modifier la `SelectMethod` valeur d’attribut `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="8cb22-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="8cb22-147">(Vous allez créer cette méthode ultérieurement.)</span><span class="sxs-lookup"><span data-stu-id="8cb22-147">(You'll create this method later.)</span></span>

<span data-ttu-id="8cb22-148">Le balisage de la `ObjectDataSource` contrôle ressemble maintenant à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="8cb22-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="8cb22-149">Dans *ISchoolRepository.cs*, ajoutez un `GetDepartmentsByName` méthode qui accepte à la fois `sortExpression` et `nameSearchString` paramètres :</span><span class="sxs-lookup"><span data-stu-id="8cb22-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="8cb22-150">Dans *SchoolRepository.cs*, ajoutez la nouvelle méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="8cb22-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="8cb22-151">Ce code utilise un `Where` (méthode) pour sélectionner les éléments qui contiennent la chaîne de recherche.</span><span class="sxs-lookup"><span data-stu-id="8cb22-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="8cb22-152">Si la chaîne de recherche est vide, tous les enregistrements seront sélectionnés.</span><span class="sxs-lookup"><span data-stu-id="8cb22-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="8cb22-153">Notez que lorsque vous spécifiez la méthode appelle ensemble dans une instruction comme celle-ci (`Include`, puis `OrderBy`, puis `Where`), la `Where` méthode doit toujours être le dernier.</span><span class="sxs-lookup"><span data-stu-id="8cb22-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="8cb22-154">Modifier existants `GetDepartments` méthode qui accepte un `sortExpression` paramètre pour appeler la nouvelle méthode :</span><span class="sxs-lookup"><span data-stu-id="8cb22-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="8cb22-155">Dans *MockSchoolRepository.cs* dans le projet de test, ajoutez la nouvelle méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="8cb22-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="8cb22-156">Dans *SchoolBL.cs*, ajoutez la nouvelle méthode suivante :</span><span class="sxs-lookup"><span data-stu-id="8cb22-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="8cb22-157">Exécutez le *Departments.aspx* page et entrez une chaîne de recherche pour vous assurer que la logique de sélection fonctionne.</span><span class="sxs-lookup"><span data-stu-id="8cb22-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="8cb22-158">Laissez la zone de texte vide et effectuez une recherche pour vous assurer que tous les enregistrements sont retournés.</span><span class="sxs-lookup"><span data-stu-id="8cb22-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="8cb22-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="8cb22-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="8cb22-160">Ajout d’une colonne de détails pour chaque ligne de grille</span><span class="sxs-lookup"><span data-stu-id="8cb22-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="8cb22-161">Ensuite, vous souhaitez afficher tous les cours pour chaque département, affiché dans la cellule de la grille de droite.</span><span class="sxs-lookup"><span data-stu-id="8cb22-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="8cb22-162">Pour ce faire, vous allez utiliser une liste imbriquée `GridView` contrôle et lier les données à des données à partir de la `Courses` propriété de navigation de la `Department` entité.</span><span class="sxs-lookup"><span data-stu-id="8cb22-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="8cb22-163">Ouvrez *Departments.aspx* et dans le balisage de la `GridView` contrôler, spécifiez un gestionnaire pour le `RowDataBound` événement.</span><span class="sxs-lookup"><span data-stu-id="8cb22-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="8cb22-164">Le balisage de la balise d’ouverture du contrôle ressemble maintenant à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="8cb22-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="8cb22-165">Ajouter un nouveau `TemplateField` élément après le `Administrator` champ de modèle :</span><span class="sxs-lookup"><span data-stu-id="8cb22-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="8cb22-166">Cette balise crée une liste imbriquée `GridView` contrôle qui affiche le numéro et le titre d’une liste de cours.</span><span class="sxs-lookup"><span data-stu-id="8cb22-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="8cb22-167">Il ne spécifie pas une source de données, car vous allez databind dans le code dans le `RowDataBound` gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="8cb22-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="8cb22-168">Ouvrez *Departments.aspx.cs* et ajoutez le gestionnaire suivant pour le `RowDataBound` événement :</span><span class="sxs-lookup"><span data-stu-id="8cb22-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="8cb22-169">Ce code obtient le `Department` convertit d’entité à partir des arguments d’événement, le `Courses` propriété de navigation pour un `List` collection et établit l’imbriquée `GridView` à la collection.</span><span class="sxs-lookup"><span data-stu-id="8cb22-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="8cb22-170">Ouvrez le *SchoolRepository.cs* fichier et spécifiez un chargement hâtif pour le `Courses` propriété de navigation en appelant le `Include` méthode dans la requête d’objet que vous créez dans le `GetDepartmentsByName` (méthode).</span><span class="sxs-lookup"><span data-stu-id="8cb22-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="8cb22-171">Le `return` instruction dans le `GetDepartmentsByName` méthode ressemble maintenant à l’exemple suivant.</span><span class="sxs-lookup"><span data-stu-id="8cb22-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="8cb22-172">Exécutez la page.</span><span class="sxs-lookup"><span data-stu-id="8cb22-172">Run the page.</span></span> <span data-ttu-id="8cb22-173">Outre le tri et filtrage que vous avez ajouté précédemment, le contrôle GridView affiche maintenant les détails du cours imbriquée pour chaque service.</span><span class="sxs-lookup"><span data-stu-id="8cb22-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="8cb22-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="8cb22-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="8cb22-175">Cette étape termine l’introduction aux scénarios de tri, filtrage et maître / détail.</span><span class="sxs-lookup"><span data-stu-id="8cb22-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="8cb22-176">Dans l’étape suivante du didacticiel, vous allez apprendre à gérer l’accès concurrentiel.</span><span class="sxs-lookup"><span data-stu-id="8cb22-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8cb22-177">[Précédent](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Suivant](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="8cb22-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>